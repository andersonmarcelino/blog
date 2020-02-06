# TDD para Frontend

Eu não deveria falar disso nesse momento pois considero óbvio. Mas é sempre bom
reforçar a importantia dos testes. **Escreva testes**.
Testes são extremamente importantes pois existem 2 verdades sobre seu software.

1. Seu software sempre vai ser testado, se você não testar o usuário vai.
1, Uma hora o teste vai falhar.

E acredite em mim, você não quer que seu software quebre só quando chegar no
usuário, se ele quebrar antes, você consiguirá arrumar ele antes.

## TDD

Antes de falarmos de ferramentas de testes para frontend, precisamos explicar o
que é TDD (e teremos evangelização aqui sim).

TDD é a sigla para Test Driven Development, ou em português, Desenvolvimento
Orientado a Testes. Falando de forma resumida é uma tecnica onde se escrevem
primeiro os testes e depois escreve o código do programa. mas na verdade não é
tão simples assim.

### TDD não é sobre testes, é sobre Desenvolvimento

O TDD não é uma metodologia de testes, é uma metodologia de desenvolvimento. ele
te ensina como escrever um código. ele te fala o que o código precisa fazer.

TDD é um processo muito natural no desenvolvimento. Antes de colocarmos as mãos
na massa, sempre paramos e pensamos o que vamos fazer, quais serão as entradas e
quais deveram ser as saidas do nosso código. A diferença é que quando estamos
fazendo TDD, paramos para documentar isso.

### Testes não são testes, são Specs

Ao escrever um arquivo de teste, temos que pensar nele não como um arquivo que
vai testar as funcionalidades do nosso código. mas sim como especificação do que
o nosso código deverá fazer.

### Porque usar TDD

* Ja é o processo que muitas pessoas fazem mentalmente sem nem mesmo perceber
* Seus testes não ficam enviezados
* É mais rapido do que escrever os testes depois do código

Por fim, existem mais coisas sobre TDD, como ciclo Read, Green,, Refactory que
não expliquei aqui para não me estender muito, mas existe muito material bom na
internet sobre isso.

## Mas como aplicar isso no frontend?

### Primeiros passos

A primeiro coisa que você precisa para desenvolver, é saber o que desenvolver.
No nosso exercicio vamos criar um TodoList com VueJS, e para escrever nossos
testes utilizarei do Cypress, pois é uma ferramenta mais visual, completa e com
uma escrita ais simples

Todo o codigo pode ser encontratado em meu repositorio do [github](https://github.com/andersonmarcelino/todovue)

## Levantando os requisitos

O primeiro requisito de um todo list é a possibilidade de criar um item.
O processo de criar um item consiste em digitar o item em um campo de texto e
apertar Enter (ou  botão de add)



