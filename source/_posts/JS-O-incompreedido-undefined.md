title: '[JS] O incompreendido undefined'
tags:
  - JavaScript
categories:
  - Programação
date: 2015-11-21 00:35:00
---
Quando estamos aprendendo uma linguagem de programação uma das primeiras coisas que devemos aprender são os tipos de dados nativos que a linguagem lhe proporciona… certo?

Se você aprendeu **Visual C#** você deve ter visto os tipos por valor essenciais da linguagem *(bool, byte, short, int, long, float, double, decimal, enum, struct, char e etc)* além dos tipos por referência que lhe permitem maior flexibilidade na criação de seus próprios tipos (*class*) e na criação de cadeias de caracteres (*string*).

Caso tenha sido **Java** então você deve saber qual a diferença entre um *int* e um *Integer*, e caso você saiba o conjunto dessas duas linguagens, você deveria saber a diferença entre os tipos de dados das duas linguagens, inclusive do tipo *enum*, que existe em ambas as linguagens, mas possuem maneiras bem diferentes de se trabalhar.

No caso de **Ruby**, deveria saber a diferença entre um *Fixnum* e de um *Float* e como diferenciar um valor do outro em uma comparação igualitária.

Se você diz que aprendeu **JavaScript**, então deve saber os tipos primitivos da linguagem *(boolean, number, string, object, function, null e etc)*. Inclusive se você vem de outra linguagem, provavelmente achará estranho algumas coisas no **JavaScript**, como o fato de que qualquer número aqui é representado simplesmente pelo tipo *number*. Aqui temos somente objetos, não temos classes (no caso de ES5). E quem vai atrás de uma maneira de emular uma classe, acaba se encontrando com um conceito chamado *constructor function*. Até aqui tudo bem, é só uma questão de adaptação… o problema é quando o desenvolvedor se encontra com os tipos *null* e *undefined*.

Primeiramente, acho que *undefined* torna-se algo mais abstrato para os programadores que vem de outras linguagens como **Visual C#/Java**, nessas linguagens o *null* serve para representar uma ausência de valor para instâncias de classe.


```cs
Cliente fulano = null;
```

E ai que começa o problema, porque no **JavaScript** também possuímos o *null*.


```javascript
var fulano = null;
```

Aqui o *null* também serve para representar uma ausência de valor, enquanto *undefined* serve para representar simplesmente que a variável não teve um valor ainda definido.

O valor *undefined* pode ser encontrado de diversas maneiras:

```javascript
// 1: declarando uma variável sem atribuição
var xpto;
console.log(xpto); // undefined

// 2: parâmetro que não recebeu um valor na
// chamada do método
(function(xpto) {
  console.log(xpto); // undefined
})();

// 3: funções sem retorno explícito
var xpto = (function() {
})();
console.log(xpto) // undefined

// 4: funções com retorno sem valor definido
var xpto = (function() {
	return;
})();
console.log(xpto); // undefined

// 5: o operador unário void analisa uma
// expressão a sua direita e retorna undefined
var xpto = void 0;
console.log(xpto); // undefined
```

O primeiro e segundo caso é bem intuitivo. O terceiro e o quarto caso basta entender que funções sempre retornam um valor, e então naqueles casos o retorna está indefinido. O quinto caso é mais uma questão de memorizar a *feature* da linguagem. O problema acontece nesse “sexto” caso:

```javascript
var xpto = undefined;
```

Antes de tudo, vamos analisar a situação. Eu estou definindo que a variável xpto receberá o valor indefinido. Isso é estranho, não? É claro… mas apesar dessa análise, ela está incorreta. Isso porque aquele *undefined* não é o valor indefinido, ele é a variável *undefined*. E o que acontece é que não temos como possuir o valor *undefined* diretamente. É necessário fazer algo como as cinco soluções anteriores para receber o valor *undefined*.

Confuso, não? Mas eu vou lhe provar que *undefined* naquele contexto é uma variável e não um valor.

```javascript
// 1: 'in' retorna um valor booleano indicando se
// uma propriedade existe em um objeto
'undefined' in window; // true

// 2: 'hasOwnProperty' vem de Object.prototype,
// ele é parecido com o 'in', mas ele verifica somente
// o próprio objeto nessa validação, desconsiderando a
// prototipagem do mesmo
window.hasOwnProperty('undefined'); // true

// 3: operação de atribuição e soma simples
undefined = undefined + 1; // NaN

// 4: undefined não existe em JSON
var obj = { a: void(0) };
console.log(JSON.stringify(obj)); // "{}"
```

O que acontece é que *undefined* é uma variável de contexto global, ela pertence ao objeto *window* no caso dos browsers. Como o escopo global é herdado para toda a aplicação, todos tem acesso a variável *undefined*, e como não é necessário fazer *window.algumacoisa* para acessar uma propriedade de contexto global, basta usar *algumacoisa*, que quando escrevemos *undefined* parece que estamos usando um valor, quando na verdade, estamos usando uma propriedade ligada ao window.

Para provar que *undefined* é uma propriedade de window, os dois primeiros testes irão retornar true, enquanto o terceiro teste é mais interessante para ser feito uma análise. Se *undefined* fosse um valor em vez de uma variável, como poderia ser feito uma atribuição para um valor? Lembre-se: você atribuí valores para variáveis, não atribuí valores para valores. Provando mais uma vez:

```javascript
1 = 3 // Error!
```

Como 1 realmente é um valor, essa operação ocasionará erro, visto que um valor não pode estar ao lado esquerdo de uma atribuição. Já no caso do *undefined* não temos um erro. Outra coisa interessante é que você deve ter percebido que *undefined* não mudou de valor... a não ser que seu browser seja bem antigo. Mas por quê? Bem, na versão do ECMAScript 3 a propriedade *undefined* podia ter seu valor alterado, permitindo coisas assim acontecerem:

```javascript
undefined = "surpresa!";
console.log(undefined); // "surpresa!"

var xpto;
console.log(xpto === undefined); // false
```

Nada agradével, certo? Por isso é aconselhável que quando você escrever um código que precise de compatibilidade com browsers antigos feche o escopo do seu código e tenha seu *undefined* garantido.

```javascript
(function(xpto, undefined) {
  // seu código aqui
})(xpto); // suas dependências passadas por parâmetro aqui

// ou
(function(xpto) {
  var undefined;
})(xpto);
```

Outra opção é não usar undefined para atribuir valores e no caso de verificações, use o operador *typeof* para ver se os valores estão indefinidos realmente.

```javascript
(function(xpto) {
  var teste; // não atribua 'undefined'
  
   // use sempre o 'typeof' para verificar
   // se o valor está indefinido
  console.log(typeof teste === 'undefined');
})(xpto);
```

Eu prefiro a última opção, e você?