# TDD para Frontend

Eu não deveria falar disso nesse momento pois considero óbvio. Mas é sempre bom
reforçar a importância dos testes. **Escreva testes**.
Testes são extremamente importantes pois existem 2 verdades sobre seu software.

1. Seu software sempre vai ser testado, se você não testar o usuário vai.
1. Uma hora o teste vai falhar.

E acredite em mim, você não quer que seu software quebre só quando chegar no
usuário, se ele quebrar antes, você conseguirá arrumar ele antes.

## TDD

Antes de falarmos de ferramentas de testes para frontend, precisamos explicar o
que é TDD (e teremos evangelização aqui sim).

TDD é a sigla para Test Driven Development, ou em português, Desenvolvimento
Orientado a Testes. Falando de forma resumida é uma técnica onde se escrevem
primeiro os testes e depois escreve o código do programa. Mas na verdade não é
tão simples assim.

### TDD não é sobre testes, é sobre Desenvolvimento

O TDD não é uma metodologia de testes, é uma metodologia de desenvolvimento. Ele
te ensina como escrever um código. Ele te fala o que o código precisa fazer.

TDD é um processo muito natural no desenvolvimento. Antes de colocarmos as mãos
na massa, sempre paramos e pensamos o que vamos fazer, quais serão as entradas e
quais deveram ser as saídas do nosso código. A diferença é que quando estamos
fazendo TDD, paramos para documentar isso.

### Testes não são testes, são Specs

Ao escrever um arquivo de teste, temos que pensar nele não como um arquivo que
vai testar as funcionalidades do nosso código. Mas sim como especificação do que
o nosso código deverá fazer.

### Porque usar TDD

* Já é o processo que muitas pessoas fazem mentalmente sem nem mesmo perceber
* Seus testes não ficam enviesados
* É mais rápido do que escrever os testes depois do código

Por fim, existem mais coisas sobre TDD, como ciclo Read, Green,, Refactory que
não expliquei aqui para não me estender muito, mas existe muito material bom na
internet sobre isso.

## Mas como aplicar isso no frontend?

### Primeiros passos

A primeiro coisa que você precisa para desenvolver, é saber o que desenvolver.
No nosso exercício vamos criar um TodoList com VueJS, e para escrever nossos
testes utilizarei do Cypress, pois é uma ferramenta mais visual, completa e com
uma escrita ais simples

Todo o código pode ser encontrado em meu repositório do [github](https://github.com/andersonmarcelino/todovue)

## Mãos na massa

O primeiro requisito de um todo list é a possibilidade de criar um item.
O processo de criar um item consiste em digitar o item em um campo de texto e
apertar Enter (ou  botão de add)

O teste para isso ficaria assim

``` javascript
it('Adds a item on todolist', () => {
  cy.visit('/')
  cy.get('[data-test="input-add"]').type('Primeira Tarefa{enter}')
  cy.get('[data-test="todo-list"]').contains('Primeira Tarefa')
})
```

Literalmente pegamos  primeiro input e digitamos o que queremos e aperamos enter.

Um detalhe desse código é o uso desse ``data-test``. Existe uma grande discussão
se pegamos os elementos utilizando classes ou id, e um argumento que existe na
comunidade é que nenhum deles é saudável. Classes são referentes a estilo e se o
estilo do elemento muda, o teste quebra, e id não são tão usado assim. Então o
que eu me  sinto mais confortável de usar é o data-test

E quando rodamos o teste, isso mesmo, ele **falha**

E falha por que não implementamos o função, então esse é o próximo passo do TDD,
fazer o teste passar. Então vamos no nosso component TodoList e colocamos o
seguinte código

``` javascript
export default {
  name: 'TodoList',
  data () {
    return {
      todoList: [],
      inputAdd: '',
    }
  },
  methods: {
    addItem () {
      this.todoList.push({
        id: Math.random(),
        label: this.inputAdd
      })
    }
  }
}
```

Aqui implementamos, usando Vue, os dados que precisamos para nosso teste passar.
criamos a todoList e o input Add, e logo depois o método que adiciona o inputAdd
na lista.

e depois disso colocamos os elemento no html

```html
<section class="todo-list">
  <input data-test="input-add" @keyup.enter="addItem" v-model="inputAdd"/>
  <ul data-test="todo-list">
    <li v-for="item in todoList" :key="item.id">
      {{item.label}}
    </li>
  </ul>
</section>
```

Onde adicionamos os elementos (com o data-test) e anexamos com os elementos do
data no script acima

E ao rodar... Nosso teste passa.

E quando vemos a tela percebemos que o texto continua no campo. O que não da uma
experiencia legal. Então criamos nosso segundo caso de teste. Queremos que o
campo seja limpo ao finalizar.

``` javascript
it('Clear the input', () => {
  cy.visit('/')
  cy.get('[data-test="input-add"]').type('Primeira Tarefa{enter}')
  cy.get('[data-test="input-add"]').should('have.value', '')
})
```

O que dizemos aqui é que queremos que após digitar o que foi digitado o campo
fique vazio.
Novamente o teste falha por que está escrito "Primeira Tarefa no campo"

Para resolver adicionamos ``this.inputAdd = ''`` no método que cria a tarefa.

O proximo passo consiste em marcar as tarefas como Done. mas a partir daqui
precisamos de varias tarefas criadas. então criaremos um setup para nosso teste

``` javascript
function setup (cy) {
  cy.visit('/')
  cy.get('[data-test="input-add"]').type('Criar Lista{enter}')
  cy.get('[data-test="input-add"]').type('Executar a Lista{enter}')
  cy.get('[data-test="input-add"]').type('Finalizar a Lista{enter}')
}
```

E escrevemos nosso teste

``` javascript
it('Move item to list below', () => {
  setup(cy)

  cy.get('[data-test="todo-list"] li:nth-child(2) [type=checkbox]')
    .click()

  cy.get('[data-test="done-list"] li')
    .should('have.length', 1)
```

Aqui clicamos em um checkbox e esperamos que ele vá para a tela de baixo.
Rodamos e o teste falha por que não colocamos um checkbox. Então fazemos isso

Criamos primeiro a lista de Done

``` html
<ul data-test="done-list">
  <li v-for="item in doneList" :key="item.id">
    <input type="checkbox" v-model="item.done">
    {{item.label}}
  </li>
</ul>
```
E adicionamos o checkbox na lista de todo também para que o possa ser clicável
e depois disso criamos nosso computed e alteramos a função de adicionar para
contemplar a função de checked

``` javascript
computed: {
  doneList () {
    return this.todoList.filter((item) => item.done)
  }
},
methods: {
  addItem () {
    this.todoList.push({
      id: Math.random(),
      label: this.inputAdd,
      done: false
    })
    this.inputAdd = ''
  }
}
```

E novamente o teste passa. Mas temos outro comportamento estranho, o item fica
duplicado em ambas as listas

O que nos leva ao nosso ultimo spec

``` javascript
it('Remove item from todo list', () => {
  setup(cy)

  cy.get('[data-test="todo-list"] li:nth-child(2) [type=checkbox]')
    .click()

  cy.get('[data-test="todo-list"] li')
    .should('have.length', 2)
})
```

E adivinhem. Isso mesmo. O teste falha.

E pra passar, fazemos uma pequena refatorada na nossa lista
chamamos a todolist de fulllist, e criamos um computed só com os toDos

Deixando nossa todoList assim

``` javascript
todoList () {
  return this.fullList.filter((item) => !item.done)
}
```

E com isso nosso teste passa!

Exitem mais funcionalidades para nosso Todolist ficar completo, mas acredito que
com esses casos já deu para pegar a ideia.

Caso queiram entender o código e ir brincando, podem baixar o repositório e se
orientar pelas tags para entender o processo
