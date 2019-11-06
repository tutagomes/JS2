### Nossa primeira interface

> Diversas vezes precisamos criar estuturas mais complexas, tipagens mais complexas, pra isso existem as interfaces, com elas podemos criar nossos próprios tipos:

A maneira mais simples para entender como as interfaces trabalham é começando com um exemplo simples, sem interface.

```typescript
    function printLabel(labelledObj: {label: string}) {
        console.log(labelledObj.label);
    }

    var myObj = {size: 10, label: "Size 10 Object"};
    printLabel(myObj);
```

Note que a função `printLabel()` recebe um objeto `labelledObj` que deverá ter um parâmetro `label` do tipo `string`, note que o objeto tem mais propriedades, mas o TypeScript verifica apenas o que foi solicitado.

Podemos reescrever este exemplo novamente, mas desta vez usando uma interface para descrever as exigências necessárias.

```typescript
    interface LabelledValue {
        label: string;
    }

    function printLabel(labelledObj: LabelledValue) {
        console.log(labelledObj.label);
    }

    var myObj = {size: 10, label: "Size 10 Object"};
    printLabel(myObj);
```

A interface `LabelledValue` é o nome que usamos para descrever as exigências do exemplo anterior. Ainda exige que uma propriedade `label` do tipo string exista. Note que eu não precisei informar que `myObj` implemente a interface `LabelledValue` como acontece em outras linguagens, apenas preciso que atenda aos requisitos, o importante aqui é a "forma".

Vale ressaltar que a ordem também não importa muito, apenas que as propriedades informadas pela interface existam e seja do tipo especificado.



### Propriedades opcionais

Um detalha legal sobre interfaces no TypeScript é que nem sempre uma propriedade é obrigatória, ela pode ser opcional, por exemplo, quando você quer criar um "options" e alguns valores serão preenchidos apenas. Por exemplo:

```typescript
    interface SquareConfig {
        color?: string;
        width?: number;
    }

    function createSquare(config: SquareConfig): {color: string; area: number} {
        var newSquare = {color: "white", area: 100};
        if (config.color) {
            newSquare.color = config.color;
        }
        if (config.width) {
            newSquare.area = config.width * config.width;
        }
        return newSquare;
    }

    var mySquare = createSquare({color: "black"});
```

As propriedades opcionais são escritas como as obrigatórias, porém com uma interrogação (?) no final do nome, você ainda pode passar o tipo, desta forma, caso a propriedade seja preenchida, só será aceito aquele formato.

Outro ponto importante é que, caso você informe uma propriedade que não deveria estar disponível ele ainda vai retornar um erro pra você.

```typescript
    interface SquareConfig {
        color?: string;
        width?: number;
    }

    function createSquare(config: SquareConfig): {color: string; area: number} {
        var newSquare = {color: "white", area: 100};
        if (config.color) {
            newSquare.color = config.collor;  // Type-checker vai informar o nome errado aqui
        }
        if (config.width) {
            newSquare.area = config.width * config.width;
        }
        return newSquare;
    }

    var mySquare = createSquare({color: "black"});  
```



### Tipos para funções

E agora a coisa começa a ficar bacana, além de informar os tipos que objetos podem aceitar, você também pode definir quais parametros e os tipos que uma função terá, e, claro, o que o método deve retornar.

Para descrever tipos para funções no TypeScript, nos fazemos como a chamada de um método, assim:

```typescript
    interface SearchFunc {
        (source: string, subString: string): boolean;
    }
```

Ou seja, eu passo as propriedades, os tipos e o que deve ser retornado, como faria com um método, mas sem nome e sem corpo, apenas a assinatura da função.

Agora que tenho uma interface eu posso usar para definir os tipos da variável e forçar que ela tenha o formato esperado:

```typescript
    var mySearch: SearchFunc;
    mySearch = function(source: string, subString: string) {
        var result = source.search(subString);
        if (result == -1) {
            return false;
        }
        else {
            return true;
        }
    }
```

O nome dos parâmetros não precisam corresponder exatamente o que está definido na interface, eu poderia, por exemplo, fazer assim:

```typescript
    var mySearch: SearchFunc;
    mySearch = function(src: string, sub: string) {
        var result = src.search(sub);
        if (result == -1) {
            return false;
        }
        else {
            return true;
        }
    }
```

Também funciona.



### Arrays

Interfaces para definir arrays também são bem legais, podemos definir o tipo do índice e o tipo do valor:

```typescript
    interface StringArray {
        [index: number]: string;
    }

    var myArray: StringArray;
    myArray = ["Bob", "Fred"];
```

Dois tipos de indices são suportados: string e number.

Enquanto assinaturas de indice são uma maneira poderosa para descrever o array, eles também impõem que todas as propriedades devem coincidir com o seu tipo de retorno. Neste exemplo, a propriedade não corresponde ao índice e gera um erro.

```typescript
    interface Dictionary {
        [index: string]: string;
        length: number;    // error, the type of 'length' is not a subtype of the indexer
    } 
```



### Classes implementanto interfaces

Como em outras linguagens, como PHP, C# ou Java, também é possível fazer com que classes implementem interfaces, na verdade este é o uso mais comum das interfaces.

```typescript
    interface ClockInterface {
            currentTime: Date;
    }

    class Clock implements ClockInterface  {
            currentTime: Date;
            constructor(h: number, m: number) { }
    }
```

Da pra informar métodos a serem implementados:

```typescript
    interface ClockInterface {
            currentTime: Date;
            setTime(d: Date);
    }

    class Clock implements ClockInterface  {
            currentTime: Date;
            setTime(d: Date) {
                    this.currentTime = d;
            }
            constructor(h: number, m: number) { }
    }
```

Mas ainda temos um detalhe, quando definimos o contrutor em uma interface, não é possível implementar a interface direto na classe, isso porque quando uma classe implementa uma interface, apenas a instância é verificada, o construtor fica do lado estático.

```typescript
    interface ClockInterface {
            new (hour: number, minute: number);
    }

    class Clock implements ClockInterface  {
            currentTime: Date;
            constructor(h: number, m: number) { }
    }
```

O exemplo acima não funciona por causa do construtor definido na interface (através da palavra `new`), o correto é você implementar a interface na variável que vai receber a instância da classe:

```typescript
    interface ClockStatic {
            new (hour: number, minute: number);
    }

    class Clock  {
            currentTime: Date;
            constructor(h: number, m: number) { }
    }

    var cs: ClockStatic = Clock;
    var newClock = new cs(7, 30);
```

E como é esperado, as interfaces descrevem atributos e métodos públicos, não é possível implementar qualquer coisa privada.



### Estendendo interfaces

Assim como as classes, interfaces podem estender outras interfaces, assim você pode segrega-las (e seguir o I do SOLID), assim mantendo uma estrutura mais enxuta e organizada.

```typescript
    interface Shape {
            color: string;
    }

    interface PenStroke {
            penWidth: number;
    }

    interface Square extends Shape, PenStroke {
            sideLength: number;
    }

    var square = <Square>{};
    square.color = "blue";
    square.sideLength = 10;
    square.penWidth = 5.0;
```



### Tipos híbridos

Por conta da natureza dinâmica do JavaScript, você pode encontrar objetos que trabalham com uma combinação dos tipos informados acima.

Veja um exemplo de objeto que atua como função e como objeto:

```php
    interface Counter {
            (start: number): string;
            interval: number;
            reset(): void;
    }

    var c: Counter;
    c(10);
    c.reset();
    c.interval = 5.0;
```