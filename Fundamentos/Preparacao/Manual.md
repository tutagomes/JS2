# Manual e Especificações

Aqui no treinamento, cobriremos o necessário para compreender um pouco sobre javascript e permitir codar na linguagem, considerando algumas de suas particularidades, forças e fraquezas. Caso você queria aprender mais sobre a definição do JS e manuais formais, você pode conferir em:

- Especificação ECMA-262 (Nome formal do JS)
- [Manual da Mozilla](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference)
- [Manual da Microsoft](http://msdn.microsoft.com/)



## Origens

O JavaScript foi criado na década de 90 por Brendan Eich a serviço da Netscape. Essa década foi um período de revolução, pois os browsers ainda eram estáticos. O navegador mais popular dessa época era o Mosaic, da NCSA.

![NCSA Mosaic. O navegador mais popular da década de 90.](http://shipit.resultadosdigitais.com.br/images/posts/javascript-1-mosaic.png)

*NCSA Mosaic. O navegador mais popular da década de 90.*

A Netscape foi fundada em 1994 para explorar a Web que estava surgindo. Foi então criado o Netscape Navigator. Em pouco tempo, este se tornou o browser dominante nessa década. Muitos desenvolvedores da NCSA foram designados no projeto do Navigator.

![Netscape Navigator: tirou a coroa do NCSA  Mosaic.](http://shipit.resultadosdigitais.com.br/images/posts/javascript-1-navigator.png)

*Netscape Navigator: tirou a coroa do NCSA Mosaic.*

A Netscape chegou à conclusão que a web teria que se tornar mais dinâmica, pois o Navigator tinha sempre que fazer uma requisição ao servidor para obter uma resposta no navegador. Em 1995, a Netscape contratou Brendan Eich para criar uma linguagem que proporcionasse isso.

A proposta inicial era a implementação da linguagem [Scheme](http://www.schemers.org/), baseada em [LISP](https://common-lisp.net/), puramente funcional, no Navigator. Porém a Netscape tinha projetos anteriores em conjunto com a Sun Microsystems para colocar sua mais recente e promissora linguagem de programação, o [Java](https://www.java.com/pt_BR/), no Navigator. Isso elevou uma discussão interna do motivo de ter duas linguagens.

Obviamente predominou a escolha de uma única linguagem com a sintaxe baseada em Java. O argumento foi que o Scheme, por ter uma sintaxe e complexidade características de linguagens funcionais, se tornaria impopular (veja o código abaixo). O objetivo da Netscape com a nova linguagem era exatamente o oposto.

```
`(import  (rnrs)  (ironscheme clr)) ;Define a function write-ln (define (write-ln fmt . args)  (clr-static-call System.Console WriteLine (clr-cast System.String fmt)    (clr-cast System.Object[]      (list->vector args)))) ; And invoke it! (write-ln "{0}" "Hello World!")`
```

Mesmo com sintaxe “javanesa” e com outras características do [Java](https://www.java.com/pt_BR/) (valores primitivos e objetos), o JavaScript logo de início sofreu a influência funcional do [Scheme](http://www.schemers.org/), e mais tarde de linguagens como o [Self](http://www.selflanguage.org/) (protótipos), [Perl](https://www.perl.org/) e [Python](https://www.python.org/) (Strings, arrays e expressões regulares).

Para defender o JavaScript contra outras propostas, um protótipo foi criado por Eich em dez dias, em Maio de 1995. Marc Andreesen nomeou o protótipo de Mocha. O nome da linguagem mudou de novo para LiveScript por causa de patentes e porque vários produtos estavam levando o “Live” como sufixo.

![Brendan Eich: o criador do JavaScript e ex CEO da Mozilla.](http://shipit.resultadosdigitais.com.br/images/posts/javascript-1-brendan-eich.png)

*Brendan Eich: o criador do JavaScript e ex CEO da Mozilla.*

No final de Novembro de 1995, a versão 2.0B3 do Navigator saiu com a versão “de dez dias” sem muitas alterações.

No começo de Dezembro de 1995, o Java estava no seu ápice e a linguagem foi renomeada para JavaScript.

## A normativa ECMA

Depois que o JavaScript foi criado, a Microsoft criou, em Agosto de 1996, uma linguagem idêntica para ser usada no Internet Explorer 3. Para conter a ambição da Microsoft, a Netscape decidiu normatizar a linguagem através da organização ECMA International, companhia que era especializada em padrões e normativas.

Os trabalhos em cima da normativa [ECMA-262](http://www.ecma-international.org/publications/standards/Ecma-262.htm) começaram em Novembro de 1996. O nome JavaScript já era patenteado pela Sun Microsystems (hoje Oracle) e não poderia ser usado. Portanto, o nome composto por ECMA e JavaScript foi usado, resultando em ECMAScript.

Mesmo com esse nome, até hoje a linguagem é conhecida carinhosamente por JavaScript. ECMAScript só é usado para se referir às versões da linguagem.

A ECMA-262 é mantida pelo Comitê Técnico 39 (TC39). Este comitê é composto por especialistas de grandes empresas como Microsoft, Mozilla e Google. Entre os integrantes do comitê está, claro, Brendan Eich. As discussões são abertas através de listas de email e as reuniões físicas sempre tem convites para experts interessados em manter a normativa.

![Reunião do TC39 em Julho de 2015.](http://shipit.resultadosdigitais.com.br/images/posts/javascript-1-tc39.png) *Reunião do TC39 em Julho de 2015. ([Fonte](https://blogs.windows.com/msedgedev/2015/08/03/recapping-the-july-2015-tc39-committee-meeting-in-redmond/))*

## Histórico das versões

#### ECMAScript 1 (Junho de 1997)

A primeira versão (“de dez dias”).

#### ECMAScript 2 (Agosto de 1998)

Mudanças editoriais para alinhar a ECMA-262 com o padrão ISO/IEC 16262.

#### ECMAScript 3 (Dezembro de 1999)

Foi nessa versão onde o JavaScript ganhou implementações importantes como do-while, expressões regulares, novos métodos para o objeto string, tratamento de exceções, entre outras coisas.

#### ECMAScript 4 (abandonada em Julho de 2008)

A ECMAScript 4 foi desenvolvida para ser a nova versão do JavaScript com um protótipo escrito em ML. Porém, o TC39 rejeitou o protótipo por causa de suas novas implementações. A quantidade de novas funcionalidades tornaria a migração da ECMAScript 3 para a 4 muito disruptiva.

Para evitar impasses, o TC39 se reuniu e entrou em um acordo com os desenvolvedores em Julho de 2008. O acordo levava em consideração 4 pontos:

1. Desenvolver uma atualização incremental da ECMAScript 3 (que se tornou a ECMAScript 5);
2. Desenvolver uma versão que fizesse menos que a ECMAScript 4, mas mais do que a atualização incremental da ECMAScript 3. O nome dessa versão é Harmony e ela será dividida na ECMAScript 6 e ECMAScript 7;
3. As funcionalidades da ECMAScript 4 seriam descartadas;
4. Outras ideias deveriam ser desenvolvidas com total consenso do TC39.

#### ECMAScript 5 (Dezembro de 2009)

Modo estrito (‘use strict’), getters e setters, novos métodos de array, suporte a JSON entre outras coisas. Essa é a atualização incremental acordada no fechamento da ECMAScript 4.

#### ECMAScript 5.1 (Junho de 2011)

Mudanças editoriais para alinhar a ECMA-262 com a terceira versão do padrão ISO/IEC 16262:2011

#### ECMAScript 6 (Junho de 2015)

Também conhecida como ECMAScript 2015, é a primeira fase da versão Harmony. Inclui sintaxe muito mais enxuta e funcionalidades como arrow functions, binary data, arrays tipados, coleções (maps, sets e weak maps), promises, melhorias em numerais e matematica, reflection, e proxies.

#### ECMAScript 7 (Junho de 2016)

Também conhecida como ECMAScript 2016, é a ultima fase da versão Harmony. Inclui features como operadores exponenciais e o método Array.prototype.includes.

Alguns browsers ainda não suportam totalmente a versão 6 e 7 da ECMAScript. Porém é possível transpilar para ECMAScript 5 através de bibliotecas como o Babel ou Polyfills.

## Referências

Recomendo muito o [Livro Speaking JavaScript](http://speakingjs.com/) da O’reilly. Além da história detalhada, é um verdadeiro manual do JavaScript.

## Conclusão

Entender como a linguagem foi criada ajuda a entender porque ela é desse jeito hoje e como ela vai evoluir. O JavaScript, graças à colaboração de grandes empresas e de experts, é uma linguagem aberta de verdade.

Eu costumava pensar, antes de conhecer a história, que o JavaScript havia sofrido um renascimento com o interpretador V8, da Google. Mas analisando a história, ela nunca morreu. Sempre foi mantida. A V8 foi apenas um dos passos para torná-la cada vez mais promissora e popular.



> http://shipit.resultadosdigitais.com.br/blog/javascript-1-uma-breve-historia-da-linguagem/