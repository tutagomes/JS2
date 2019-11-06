# Classes

A definição de classes no TypeScript segue o mesmo padrão de outras linguagens de programação, e tem grande semelhança com C#.

```typescript
class Greeter {
    private greeting: string;
    constructor(message: string) {
        this.greeting = message;
    }
    public greet():string {
        return "Hello, " + this.greeting;
    }
}

let greeter = new Greeter("world");
console.log(greeter.greet());
```

Foi declarada a classe **Greeter** com um atributo chamado **greeting,** um método **construtor** e o método **greet** que tem um retorno do tipo String. Você pode definir também a visibilidade doe atributos e métodos assim como em outras linguagens de programação, usando private, public ou protected.

Também no exemplo acima criamos uma instancia da classe **Greeter,** e depois chamamos um método da classe.

# Herança

Para se herdar métodos e atributos de uma classe para outra, como você pode imaginar, basta usar **extends**. Se um método da classe pai for subscrito na classe filha, você pode chamar o método da classe pai usando **super**, como vemos a seguir:

```typescript
//Classe pai
class Animal {
    name: string;
    constructor(theName: string) { this.name = theName; }
    move(distanceInMeters: number = 0) {
        console.log(`${this.name} moved ${distanceInMeters}m.`);
    }
}

//Classe filha
class Snake extends Animal {
    constructor(name: string) { super(name); }
    move(distanceInMeters = 5) {
        super.move(distanceInMeters);
    }
}
```



Veja que é usado **super** na classe Snake para chamar um método da classe Animal de quem ela herda, mas subscreve o método.

# Acessors (get/set)

TypeScript suporta getters/setters que são usados para acesso a atributos protegidos da classe.

Criando uma propriedade ou atributo como **private,** você deve ter um método **get** e um método **set** para alterar os valores desse atributo. Como no exemplo a baixo:

```typescript
class Pessoa {
    private _name: string;
 
    get name(): string {
        return this._name;
    }
	
    set name(p : string) {
        this._name = p;
    }
}
 
var p = new Pessoa(); //Instanciando classe
p.name = "123456"; //set
console.log(p.name); //get
```



No exemplo acima se o método **set** não existir a linha **14** retorna um erro. E se **get** não existir, a linha **15** retorna ***undefined.\***

# Classes Abstratas

Classes abstratas servem como base para a construção de outras classes, dando-lhes a estrutura padrão de métodos e atributos.

Os métodos marcados com **abstratc** na classe abstrata deve ser implementado na classe filha. Veja exemplo:

```typescript
abstract class Department {

    constructor(public name: string) {
    }

    printName(): void {
        console.log('Department name: ' + this.name);
    }

    abstract printMeeting(): void; // tem que ser implementado nas classes filhas
}

class AccountingDepartment extends Department {

    constructor() {
        super('Accounting and Auditing'); // constructors in derived classes must call super()
    }

    printMeeting(): void {
        console.log('The Accounting Department meets each Monday at 10am.');
    }

    generateReports(): void {
        console.log('Generating accounting reports...');
    }
}

var department = new AccountingDepartment(); // ok to create and assign a non-abstract subclass
department.printName();
department.printMeeting();
department.generateReports();
```



Perceba que na classe abstrata Departament o método printMeeting foi declarado como abstrato, por isso na classe filha que é AccountingDepartment ele precisa ser implementado. Enquanto o método printName, da classe Pai não precisa ser implementado na classe filho.

# Interfaces

Uma interface também define a estrutura das classes que a implementam, mas a diferença em relação às Classes Abstratas é que todos os métodos e atributos devem ser implementados de alguma forma, a não ser quando você definir o atributo como opcional, como vemos a seguir:

```typescript
interface Point{
    x: number;
    y: number;
    z?: number;
    showX();
}

class Point3d implements Point{
    x: number = 1;
    y: number = 2;
    showX() {
        console.log(this.x)
    }
}

var point = new Point3d();
point.showX();
```



Note que o atributo **z** na interface **Point** está marcado com um **?** que sinaliza que ele é opcional. Note também que na classe **Point3d** o atributo **z** não é implementado.

 

# Módulos

Os módulos no TypeScript, ajudam a separar as classes. Pense em algo parecido com “namespace” do C#. Assim é possível distribuir melhor as classes, alem de classificar as classes por segmento, por exemplo. Classes sem módulo, tende a se espalhar pelo projeto, fica difícil localizar uma funcionalidade especifica. Módulos ajudam a manter o código com um papel específico contido no interior do módulo.
No TypeScript, existe módulos internos e externos.
Nós já utilizamos o módulo interno, de forma implícita. Quando não especificamos um módulo explicitamente. O TypeScript adiciona ao módulo “window”.
Vamos ver um exemplo um exemplo de módulo interno nomeado.

```typescript
`module Tenant {``  ``export` `class` `Product {``    ``name: string;``    ``price: number;` `  ``constructor(name: string, price: number) {``      ``this``.name = name;``      ``this``.price = price;``    ``}` `    ``getPriceWithDiscount(): number {``      ``var` `date: Date = ``new` `Date();``      ``var` `discount: number = 0.10;``      ``if` `(date.getDay() == 1 || date.getDay() == 6)``        ``discount = 0.12;` `      ``return` `this``.price - (``this``.price * discount);``    ``}``  ``}``}` `window.onload = () => {``  ``var` `product: Tenant.Product = ``new` `Tenant.Product(``"Table"``, 100.10);``  ``console.log(``"Preço da mesa"``, product.getPriceWithDiscount());``};`
```

No exemplo acima, modificamos um exemplo de um código anterior. Com a diferença, que agora a classe “Product” está envolta de um módulo interno chamado “Tenant”. Também adicionamos a palavra chave “export” antes de “class” da classe “Product”, isso é necessário quando desejamos expor a classe, para se utilizada, fora do módulo, caso contrário, ela não ficará visível. E por fim, adicionamos o nome do módulo, ao chamar a classe “Product”, chamando sua referencia assim “Tenant.Product”, em vez de somente “Product”.
É comum separar os módulos em arquivos separados. Não se esqueça de referenciar os arquivos de módulos, para que seja possível localizar seus membros, que estão denotados com “export”.