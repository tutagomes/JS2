# Regex

Uma expressão regular é uma notação para representar **padrões** em strings. Serve para validar entradas de dados ou fazer busca e extração de informações em textos.

Por exemplo, para verificar se um dado fornecido é um número de 0,00 a 9,99 pode-se usar a expressão regular `\d,\d\d`, pois o símbolo `\d` é um curinga que casa com um dígito.

O verbo [*casar*](http://turing.com.br/material/regex/glossary.html#term-casar) aqui está sendo usado tradução para *match*, no sentido de *combinar*, *encaixar*, *parear*. Dizemos que a expressão `\d,\d\d` casa com 1,23 mas não casa com `123` (falta a vírgula) nem com `1,2c` (“c” não casa com `\d`, porque não é um dígito).

O termo em inglês é *regular expression* de onde vem as abreviações *regex* e `re` (o nome do módulo Python). Na ciência da computação, o termo tem um significado bem específico (veja [*expressão regular*](http://turing.com.br/material/regex/glossary.html#term-expressao-regular) no [*Glossário*](http://turing.com.br/material/regex/glossary.html#glossario)).



## Constructor

A primeira forma (e menos usada), é chamando o construtor do objeto **RegExp**

```typescript
const regex = new RegExp()
console.log(regex) // /(?:)/
```

O primeiro argumento que o objeto recebe (nosso padrão), pode ser qualquer coisa (JavaScript feelings). Porém, para o bom uso, ele deve ser uma string ou uma outra regex (sim, outra regex).

No caso da String, é necessário entender que as vezes precisaremos escapar os caracteres que possam ter outro significado. Quem aqui nunca usou um `\n` pra dar aquela quebra de linha?

Então, se quisermos considerar que a barra seja de fato uma barra, e não um "efeito especial", precisamos escapa-la com outra barra (._.):

```typescript
const regex = new RegExp('\\w')
```

O segundo parâmetro é a *flag* que adiciona um comportamento em nosso motor da Regex. A mais popular e já comentada bastante é a *flag* `g` , que permite que o motor continue procurando por todo o alvo o seu padrão. Mas também existe outras bem importantes, como:

- `i` : Ignora o **case,** ou seja, não difere letras maiúsculas ou minúsculas;
- `m` : Permite que o padrão seja aplicado em múltiplas linhas. Bem útil no caso onde temos que aplicar âncoras e ainda assim queremos aplicar em várias linhas;
- `y` : Força a regex só trazer os matches consecutivos, ou seja, se você tem um alvo que tem 2 resultados consecutivos e em seguida um caractere (ou conjunto) que não bate com seu padrão, ele só traz os primeiros resultados.
- `u` : Habilita a capacidade da Regex engine de entender caracteres unicode e captura-los corretamente (exemplo: 𝌆).

Esses são as flags que o JavaScript aceita, mas se você é de outra linguagem, talvez tenha mais tipos diferentes.

Passando então a nossa expressão e a flag, temos uma declaração semelhante a essa:

```typescript
const regex = new RegExp('\\w','g')
```



## Literal

A segunda forma de declarar uma expressão regular é fazendo da forma literal, ou seja, passando uma expressão entre duas barras `/` e em seguida, passando as flags:

```typescript
const regex = /\w/g
```

Bem mais fácil, não?



# Métodos

Finalmente vamos começar a ver os métodos dos objetos do tipo RegExp. Assim como a maioria dos tipos de dados em Javascript, RegExp também possui vários métodos, mas vamos nos focar nos dois principais, `exec` e `test` . E talvez o segundo seja ainda mais útil que o segundo do ponto de vista do dia-a-dia.

## Exec

Esse é o método que você irá utilizar quando você precisa recuperar o dado que está filtrando. Ele é aquele tipo de função em que precisamos iterar através de um `while` , pois, a cada vez que você executa o método, ele **pula** para o próximo resultado.

Para provar isso, veja o exemplo abaixo:

```typescript
const target = '22a33b44c'
const pattern = /\d{2}\w/g
let result = pattern.exec(target);

console.log(result) // [ '22a', index: 0, input: '22a33b44c' ]
console.log(pattern.exec(target)) // [ '33b', index: 3, input: '22a33b44c' ]
```



## Test

O método **test**, como disse anteriormente, talvez seja muito mais útil que o **exec** no dia-a-dia. Mas, porque?

Bem, o **test** apenas valida se aquele alvo contém o padrão que você definiu, retornando um `true` ou `false` .

Para entender melhor, vamos ver o exemplo abaixo:

```typescript
const alvo = '(16) 99999-9999'

const ddd = '\\(\\d{2}\\)'
const numero = '\\s+\\d{5}-\\d{4}'
const regexTelefone = new RegExp(ddd+numero,'g')

console.log(regexTelefone) // /\(\d{2}\)\s+\d{5}-\d{4}/g
console.log(regexTelefone.test(alvo)) // true
```



Agora que vimos um pouco sobre Regex, que tal fazermos alguns exercícios? Acesse o site do [RegexOne](https://regexone.com/)  e complete alguns dos desafios!



Caso prefira exercícios mais complexos, o site [Regex101](https://regex101.com/quiz) tem o que precisa!