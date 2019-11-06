## Boolean

O bom e velho true/false (verdadeiro e falso), comum no Javascript e em outras linguagens de programação (me arrisco a dizer todas?).

```typescript
var isDeath:boolean = false;
```



## Number

Assim como no Javascript, todos os números são pontos flutuantes (float).

```typescript
var age:number = 21;
```



## String

Valores do tipo texto, você pode usar aspas duplas ("meu texto") ou simples ('meu texto') para definir uma string.

```typescript
var fullname: string = "Erik Figueiredo";
```



## Array

Array ficou bem legal com TypeScript, ao especificar um array você pode informar o tipo dos elementos dentro dele. Existem duas formas de fazer isso:

Usando colchetes apos o tipo:

```typescript
var persons: string[] = ['Erik', 'Fernanda'];
```

Usando a palavra-chave Array seguido do tipo entre maior e menor:

```typescript
var persons: Array<string> = ['Erik', 'Fernanda'];
```

Claro que isso funciona com number, boolean...

Se você for usar vários tipos de valores dentro do array vai poder usar o tipo any, vou falar dele a logo mais.

var persons: any[] = ['Erik', 30, true];



## Tuple(Tupla)

Os tipos de tupla permitem que você expresse um array onde o tipo e um número fixo de elementos é conhecido, mas não precisa ser o mesmo.

```typescript
let tuple: [string, number, string, number];
tuple = ['hello', 1, 'world', 2];

console.log(tuple[0]);
console.log(tuple[1]);
```

No exemplo acima temos um número definido de elementos na array, 4 e ele são duas strings e dois números



## Enum

Este é novo, digo, novo pra você que trabalha com Javascript, o tipo enum (enumeração) é ideal para trabalhar com listas, ele nada mais é que um objeto, mas com a chave dos elementos definida de forma mais amigável.

Por padrão ele trabalha com 0.

```typescript
enum Color {Red, Green, Blue};
var c: Color = Color.Green;

console.log(c);
```

Isso deve retornar 1.

Mas você também pode indicar o valor inicial:

```typescript
enum Color {Red=1, Green, Blue};
var c: Color = Color.Green;

console.log(c);
```

Agora teremos o valor 2 como retorno.

E ainda podemos pegar o valor inverso:

```typescript
enum Color {Red = 1, Green, Blue};

var number: Color = Color.Green;
var name: string = Color[2];

console.log(name);
console.log(number);
```

Isso deve retornar Green e 2, nesta ordem.

Isso é possível porque o objeto Color guarda os dados das duas formas, ou seja, tanto com indice 2 e valor Green, quanto indice Green e valor 2, veja o retorno do Color:

```typescript
Object {1: "Red", 2: "Green", 3: "Blue", Red: 1, Green: 2, Blue: 3}
```

Bem bacana, né.



## Any

Muitas vezes não podemos forçar o tipo do dado, talvez ele venha do banco ou de uma api de terceiros. Qualquer que seja o motivo, você pode rotular sua variável com o tipo "any", assim ela vai aceitar qualquer coisa.

```typescript
var notSure: any = 4;
notSure = "maybe a string instead";
notSure = false; // okay, definitely a boolean
```

Vale informar que o tipo any é ideal para você que vai transpor um Javascript existente para TypeScript e não quer se preocupar com os erros na compilação, assim você pode fazer isso de forma gradual.



## Void

Ao contrário do "any" (qualquer tipo de dado), você pode especificar um tipo void (nenhum tipo de dado), ele é muito usado em funções, para informar que ela não retorna nada.

```typescript
function warnUser(): void {
    alert("This is my warning message");
}
```



## Retorno de dados

Como vimos no item anterior, também podemos informar um tipo para o retorno de funções, isso pode ser feito com qualquer um dos data types informados aqui.

```typescript
function isChecked(): boolean {
    return true;
}
```



## Inferência de Tipos

Mesmo que cada local de armazenamento tenha um tipo estático no TypeScript, nem sempre é necessário especificá-lo explicitamente. O TypeScript geralmente pode inferir isso. Por exemplo, se você escrever:

```
let x = 123;
```

Em seguida, o TypeScript infere que ***x\*** tem o tipo estático ***number\***.