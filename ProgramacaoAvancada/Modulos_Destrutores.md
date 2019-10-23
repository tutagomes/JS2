# Organizando Código em Módulos

À medida que os aplicativos JS de hoje se tornam cada vez mais complexos, trabalhar com namespaces e dependências se torna cada vez mais difícil. Uma solução para este problema foi a introdução do conceito de *módulos*, que permite que você particione sua código em partes independentes, aproveitando o encapsulamento para evitar conflitos entre diferentes módulos. Nesta seção, vamos ver como trabalhar desta forma. No entanto, vamos ver como a divisão em módulos era feita nos primórdios do JS.

### Como fazer

Organizar código é uma necessidade básica quando se lida com centenas ou milhares de linhas de código e muitas maneiras de lidar com o problema foram inventadas antes do JS finalmente definir um padrão. Primeiro, vamos olhar para a maneira mais clássica - *iffy* (vamos ver o que isso significa em breve) e, em seguida, passar para soluções mais modernas, mas esteja ciente de que você pode encontrar todos esses jeitos ao ler o código de outras pessoas!



### O modo IIFE

Antes dos módulos se tornarem amplamente utilizados, havia um padrão bastante comum em uso, que basicamente fornecia os mesmos recursos que os módulos de hoje. Vamos primeiro ver o código e depois analisar o que acontece:

```js
/* @flow */

/*
    In the following code, the only thing that needs
    an explicit type declaration for Flow, is "name".
    Flow can work out on its own the rest of the types.
*/

const myCounter = ((name: string) => {
    let count = 0;

    const inc = () => ++count;

    const get = () => count; // private

    const toString = () => `${name}: ${get()}`;

    return {
        inc,
        toString
    };

})("Clicks");

console.log(myCounter); // an object, with methods inc and toString

myCounter.inc(); // 1
myCounter.inc(); // 2
myCounter.inc(); // 3

myCounter.toString(); // "Clicks: 3"
```

Definir uma função e imediatamente chamá-la é chamado de IIFE, pronunciado *iffy*, e significa *Imediatamente Invoked Function Expression*. 

IIFEs também são conhecidos como *Self-Executing Anonymous Functions*, que não soa tão bom quanto *iffy*!

Nós definimos uma função (a que começa com nome => ...), mas imediatamente a chamamos (com ("Clicks") depois). Portanto, o que é atribuído a myCounter não é uma função, mas o seu valor retornado, ou seja, um objeto. Vamos analisar o conteúdo deste objeto. Por causa das regras de escopo para funções, o que quer que você defina dentro não é visível do lado de fora. No nosso caso particular, isto significa que count, get(), inc(), inc() e toString() não serão acessíveis. No entanto, como o nosso IIFE retorna um objeto incluindo as duas últimas funções, essas duas (e somente essas duas) são utilizáveis do lado de fora: isso é chamado de *padrão de módulo revelador*.

Se você seguiu até agora, o seguinte deve ser claro para você:

- Quaisquer que sejam as variáveis ou funções definidas no módulo, não são visíveis ou acessíveis do exterior, a menos que você explicitamente as retorne
- Quaisquer nomes que você decida usar no seu módulo não entram em conflito com nomes externos por causa das regras de escopo
- As variáveis passadas como parâmetro (no nosso caso, nome) persistem para que o módulo possa armazenar informações e usá-las posteriormente



Em suma, devemos concordar que os IIFEs são uma gambiarra para resolver um problema da linguagem. Ao analisar códigos de algumas bibliotecas mais antigas, é bem provável que você irá encontrar inúmeros exemplos disso.  No entanto, o ES6 introduziu uma forma mais geral (mais clara e fácil de entender) de definir módulos, que é o que vamos usar.

### Trocando IIFE por Módulos

O conceito chave em módulos é que você terá arquivos separados, cada um dos quais representará um módulo. Existem dois conceitos complementares: importação e exportação. Logo, caso você esteja criando um módulo e deseja que algumas funcionalidades estejam visíveis para o mundo externo, é necessário exportá-las.

Primeiro, vejamos o equivalente ao nosso módulo contador da secção anterior, e depois comentemos as funcionalidades extra que podemos usar:

```js
/* @flow */

let name: string = "";
let count: number = 0;

let get = () => count;
let inc = () => ++count;
let toString = () => `${name}: ${get()}`;

/*
    Since we cannot initialize anything otherwise,
    a common pattern is to provide a "init()" function
    to do all necessary initializations.
*/
const init = (n: string) => {
    name = n;
};

export default { inc, toString, init }; // everything else is private
```

Para utilizar este módulo em algum outro arquivo da nossa aplicação, poderemos escrever algo do tipo:

```js
import myCounter from "module_counter";

/*
    Initialize the counter appropriately
*/
myCounter.init("Clicks");

/*
    The rest would work as before
*/
myCounter.inc(); // 1
myCounter.inc(); // 2
myCounter.inc(); // 3
myCounter.toString(); // "Clicks: 3"
```

OK, então usar este módulo para criar um contador não é tão diferente da primeira abordagem. A principal diferença com a versão do IIFE é que aqui não podemos fazer uma inicialização. Um padrão comum para fornecer isso é exportar uma função init() que fará o que for necessário. Quem usa o módulo deve, em primeiro lugar, chamar init() para configurar as coisas corretamente.



### Adicionando checkagens de inicialização

Se você quiser é possível tornar a função `.init()` mais poderosa (mais robusta) fazendo com que o módulo não funcione enquanto a função `init` não for chamada.

```js
// Source file: module_counter.2.js

/* @flow */

let name = "";
let count = 0;

let get = () => count;

let throwNotInit = () => {
    throw new Error("Not initialized");
};
let inc = throwNotInit;
let toString = throwNotInit;

/*
    Since we cannot initialize anything otherwise,
    a common pattern is to provide a "init()" function
    to do all necessary initializations. In this case,
    "inc()" and "toString()" will just throw an error 
    if the module wasn't initialized.
*/
const init = (n: string) => {
    name = n;
    inc = () => ++count;
    toString = () => `${name}: ${get()}`;
};

export default { inc, toString, init }; // everything else is private
```

Desta forma, podemos garantir a utilização adequada do nosso módulo. Note que a ideia de atribuir uma nova função para substituir uma antiga é muito típica do estilo de Programação Funcional; as funções são objectos de primeira classe que podem ser passados, devolvidos ou armazenados.



### Using more import/export possibilities

Há também outro tipo de exportação, *nomeado* de exportação, da qual você pode ter vários por módulo. Por exemplo, digamos que você precisava de um módulo para fazer algumas conversões de distância e peso. O seu módulo pode ser o seguinte:

```js
/* @flow */

type conversion = number => number;

const SPEED_OF_LIGHT_IN_VACUUM_IN_MPS = 186282;
const KILOMETERS_PER_MILE = 1.60934;
const GRAMS_PER_POUND = 453.592;
const GRAMS_PER_OUNCE = 28.3495;

const milesToKm: conversion = m => m * KILOMETERS_PER_MILE;
const kmToMiles: conversion = k => k / KILOMETERS_PER_MILE;

const poundsToKg: conversion = p => p * (GRAMS_PER_POUND / 1000);
const kgToPounds: conversion = k => k / (GRAMS_PER_POUND / 1000);

const ouncesToGrams: conversion = o => o * GRAMS_PER_OUNCE;
const gramsToOunces: conversion = g => g / GRAMS_PER_OUNCE;

/*
 It's usually preferred to include all "export"
 statements together, at the end of the file.
 You need not have a SINGLE export, however.
*/
export { milesToKm, kmToMiles };
export { poundsToKg, kgToPounds, gramsToOunces, ouncesToGrams };
export { SPEED_OF_LIGHT_IN_VACUUM_IN_MPS };
```

Você pode ter quantas definições quiser e pode exportar qualquer uma delas; no nosso caso, estamos exportando seis funções e uma constante. Você não precisa empacotar tudo em uma única exportação; você pode ter várias, como já visto anteriormente. As exportações são normalmente agrupadas no final de um módulo para ajudar o leitor a encontrar rapidamente tudo o que o módulo exporta. Você também pode exportar algo na mesma linha onde você o define, como `export const LENGTH_OF_OF_YEAR_IN_DAYS = 365.2422`, mas também não vamos usar esse estilo.

Ao importar um módulo com exportações nomeadas você só precisa dizer qual das exportações você quer.  É uma prática padrão agrupar todos eles no início do seu arquivo fonte. Você também pode renomear uma importação, como no caso do `poundsToKg` no código abaixo, que usaremos como p_to_kg. Na realidade, você faria isso se tivesse nomeado identicamente importações de dois módulos diferentes; no nosso exemplo particular, a troca de nome é apenas ilustrativa.

```js
/* @flow */

import {
    milesToKm,
    ouncesToGrams,
    poundsToKg as p_to_kg
} from "./module_conversions.js";
console.log(`A miss is as good as ${milesToKm(1)} kilometers.`);

console.log(
    `${ouncesToGrams(1)} grams of protection `,
    `are worth ${p_to_kg(1) * 1000} grams of cure.`
);
```



### Utilizando tipos do Flow com Módulos

A exportação de tipos de dados (incluindo genéricos, interfaces, etc.) é bastante semelhante às exportações normais, a única diferença que você deve incluir a palavra `type`. Se você quiser usar o tipo de conversão em outro lugar, a exportação ficaria assim:

```js
export type { conversion };
```

E claro, ao importá-lo, também é preciso utilizar a palavra `type`:

```js
import type { conversion } from "./module_conversions.js";
```



> Note, however, an important detail: you cannot export or import data types in the same sentence in which you deal with standard JS elements: export and export type are distinct, separate statements, and so are import and import type.



### Exercício

Agora que conhecemos um pouco mais sobre módulos, vamos criar um módulo para gerenciar as tarefas outro módulo para comunicar com o endpoint do JSON-Server, utilizando Promessas quando achar necessário.

E claro, para utilizar os métodos de programação assíncrona aprendidos anteriormente, sempre que for enviar uma tarefa, adicione um campo chamado `UTCTime` e estabeleça o valor dele como o valor `currentDateTime` obtido no retorno da api `http://worldclockapi.com/api/json/est/now`

Um exemplo do retorno da API de tempo:

```json
{
  "$id": "1",
  "currentDateTime": "2019-10-22T23:17-04:00",
  "utcOffset": "-04:00:00",
  "isDayLightSavingsTime": true,
  "dayOfTheWeek": "Tuesday",
  "timeZoneName": "Eastern Standard Time",
  "currentFileTime": 132162598477633520,
  "ordinalDate": "2019-295",
  "serviceResponse": null
}
```

