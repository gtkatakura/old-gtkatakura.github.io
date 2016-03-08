title: '[OOP] Tipagem: estática, dinâmica, forte, fraca... wat?'
tags:
  - 'Visual C#'
categories:
  - Programação
date: 2015-11-23 20:24:00
---
Alguém já lhe perguntou se você sabe qual é a tipagem da linguagem de programação que você trabalha, ou de alguma linguagem de programação que você aprende por motivos terceiros?

Eu me lembro vagamente quando me perguntaram isso sobre o **JavaScript** algum tempo atrás, naquela época eu não fazia ideia sequer do que era *tipos de dados*, eu era aquele programador prático que sabia fazer as coisas, mas não sabia os termos técnicos.

Mas claro, como qualquer curioso eu fui pesquisar, porém era novo na área e achei que uma breve pesquisa seria o suficiente para entender esses conceitos, então acabei deixando o assunto de lado, afinal uma pesquisa superficial é tudo do que precisamos, certo?

Um tempo depois, outra pessoa acabou me fazendo a mesma pergunta, e dessa vez eu estava preparado, sabia as definições *decorebas*. Por azar, minha resposta *decoreba* foi o suficiente para satisfazer a dúvida do meu colega, talvez na época ele também não soubesse muito do assunto, ou ele simplesmente não queria perder o resto do dia conversando sobre isso... que pena.

## Visual C# - Estática e forte

```csharp
int inteiro = 4;
inteiro = "quatro"; // Erro

Pessoa joao = new Pessoa("João");
Console.WriteLine(joao + 4); // Erro
```