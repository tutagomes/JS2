# O que é uma Generic em TypeScript?

A documentação do TypeScript explica o Generics como “sendo capaz de criar um componente que pode funcionar em vários tipos, em vez de em um único”.

Ótimo! Isso nos dá uma ideia básica. Nós vamos usar o Generics para criar algum tipo de componente reutilizável que pode funcionar para uma variedade de tipos. Mas como isso acontece? Aqui está como eu gosto de pensar sobre isso:

**Generics são para tipos o que valores são para argumentos de função — eles são uma maneira de dizer aos nossos componentes (funções, classes ou interfaces) o tipo que queremos usar quando executarmos esse pedaço de código, da mesma forma como dizemos a uma função quais valores usar como argumentos quando nós a executamos.**

A melhor maneira de entender o que essa declaração significa é escrever uma função de identidade genérica. A função identidade é uma função que simplesmente retorna qualquer argumento que seja passado para ela. Em JavaScript simples, isso seria:

```typescript
function identity (value) {
    return value;
}

console.log(identity(1)) // 1
```

Agora, vamos adaptar isso para trabalhar em um número no TypeScript:

```typescript
function identity (value: Number) : Number {
    return value;
}

console.log(identity(1)) // 1
```

É bom que tenhamos um tipo definido, mas a função não é muito flexível. A função de identidade deve funcionar para qualquer valor passado, não apenas números. É aqui que os genéricos entram. Os genéricos nos permitem escrever uma função que pode assumir qualquer tipo e transformar nossa função com base nesse tipo.

```typescript
function identity <T>(value: T) : T {
    return value;
}

console.log(identity<Number>(1)) // 1
```

Existe essa sintaxe `` desconhecida! Mas não é nada para se ter medo. Assim como se estivéssemos passando um argumento, passamos o tipo que queremos usar para essa chamada específica da função.

![img](https://miro.medium.com/proxy/1*5GZ39qVIwNGWJUsY1ekZaw.png)

*Os tipos genéricos são preenchidos da mesma forma como preenchemos os argumentos da função ao chamá-la.*

Baseado na imagem acima, quando chamamos `identity(1)`, o tipo `Number` é um argumento como o `1`. Ele preenche o valor `T` onde quer que ele apareça. Ele também pode receber vários tipos, assim como podemos ter vários argumentos.

![img](https://miro.medium.com/proxy/1*v68QEnkC4qbsdcLOrBj47g.png)

*Uma função pode ter vários tipos genéricos, assim como pode ter vários argumentos.*

Vale notar como estamos chamando a função. A sintaxe deve começar a fazer sentido para você agora. **Não há nada de especial em** `**T**` **ou** `**U**`**, eles são apenas nomes de variáveis que escolhemos. Nós os preenchemos com valores de tipo quando chamamos a função e ela usa esses tipos.**

Outra maneira de pensar em genéricos é que eles transformam uma função com base no tipo de dados que você passa para ela. A animação abaixo mostra como a função de identidade é alterada com diferentes tipos de dados.

![img](https://miro.medium.com/proxy/1*Zz4Y9ScEbGbRrtIWby4msg.gif)

*Um tipo genérico irá se transformar em qualquer tipo que seja passado para ele.*

Como você pode ver, a função assume qualquer tipo que seja passada para ela, permitindo criar componentes reutilizáveis para diferentes tipos, exatamente como a documentação nos prometeu.

**Observe atentamente a segunda instrução de log na animação**. Nós não fornecemos um tipo. Nesse caso, o TypeScript tentará inferir o tipo com base nos dados. Tenha cuidado — a inferência de tipos só funciona para dados simples. Se você passar algo mais complexo como um array de objeto ou multi-tipo, ele irá inferir que o tipo como `any`, o que quebra nossas verificações de segurança de tipo.

------



# Genéricos para Classes e Interfaces funcionam exatamente como em Funções

Agora que sabemos que genéricos são apenas uma maneira de passar tipos para um componente. Acabamos de ver como isso funciona para funções e a boa notícia é: interfaces e classes funcionam exatamente da mesma maneira! No caso deles, colocamos os tipos logo após o nome da nossa interface ou nome da classe.

Veja se o seguinte bloco de código faz sentido para você agora (espero que sim!):

```typescript
interface GenericInterface<U> {
  value: U
  getIdentity: () => U
}

class IdentityClass<T> implements GenericInterface<T> {
  value: T

  constructor(value: T) {
    this.value = value
  }

  getIdentity () : T {
    return this.value
  }

}

const myNumberClass = new IdentityClass<Number>(1)
console.log(myNumberClass.getIdentity()) // 1

const myStringClass = new IdentityClass<string>("Hello!")
console.log(myStringClass.getIdentity()) // Hello!
```

Se isso não fizer sentido imediatamente para você, tente rastrear os tipos (`T`) na cadeia de chamadas de função. Funciona assim:

1. Instanciamos uma nova instância de `IdentityClass`, passando `Number` e `1`
2. Na classe de identidade, `T` torna-se `Number`
3. `IdentityClass` implementa `GenericInterface` e sabemos que `T` é `Number`, por isso é como se estivéssemos implementando `GenericInterface`
4. Em `GenericInterface`, `U` torna-se `Number`. Eu propositadamente usei nomes de variáveis diferentes aqui para mostrar que o valor do tipo se propaga pela cadeia e o nome da variável não importa

------



# Caso de uso prático: indo além dos tipos primitivos

Todos os exemplos fornecidos acima usam tipos primitivos, como `Number` e `string`. Estes são bons para usar como exemplos, mas falando praticamente, você provavelmente não estará usando genéricos para tipos primitivos. O poder real dos genéricos vem quando temos tipos ou classes personalizadas que formam uma árvore de herança.

Considere o exemplo clássico de herança de um carro. Nós temos uma classe base `Car` que é usada como base para `Truck` e `Vespa`. Em seguida, escrevemos uma função de utilidade `washCar` que recebe uma instância genérica `Car` e depois a retorna.

```typescript
class Car {
  label: string = 'Generic Car'
  numWheels: Number = 4
  horn() {
    return "beep beep!"
  }
}

class Truck extends Car {
  label = 'Truck'
  numWheels = 18
}

class Vespa extends Car {
  label = 'Vespa'
  numWheels = 2
}

function washCar <T extends Car> (car: T) : T {
  console.log(`Received a ${car.label} in the car wash.`)
  console.log(`Cleaning all ${car.numWheels} tires.`)
  console.log('Beeping horn -', car.horn())
  console.log('Returning your car now')
  return car
}

const myVespa = new Vespa()
washCar<Vespa>(myVespa)

const myTruck = new Truck()
washCar<Truck>(myTruck)
```

Ao dizer à nossa função `washCar` que `T` deve estender `Car`, sabemos quais funções e propriedades podemos chamar dentro da função. Usar um tipo genérico também nos permite retornar o tipo específico que passamos, ao invés de apenas um não específico tipo de `Car`.

A saída para este código é:

```typescript
Received a Vespa in the car wash.
Cleaning all 2 tires.
Beeping horn - beep beep!
Returning your car now
Received a Truck in the car wash.
Cleaning all 18 tires.
Beeping horn - beep beep!
Returning your car now
```