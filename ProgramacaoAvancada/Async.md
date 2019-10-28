# Programação Assíncrona



Até pouco tempo atrás o sistema de promessas no JavaScript era um assunto fora da minha área de conhecimento. Com a oportunidade de trabalhar em um projeto isomórfico com Javascript, ou seja, a aplicação rodando no servidor (NodeJs) e cliente (browser), as coisas começaram a ficar mais claras.

Aproveitando este momento de “***aha\*!**”, espero ajudar quem ainda está no desafio de entender melhor o que é o sistema de promessas e o que elas tem a ver com a maneira como arquitetamos nossas aplicações.

Atualmente os computadores conseguem fazer “tarefas” (vamos chamá-las assim, é mais fácil) ao mesmo tempo, resultando em processos mais eficientes e rápidos.

Imagine um cenário onde você conseguisse levantar da cama, fazer café, tomar banho e escovar os dentes ao mesmo tempo; aqueles 45 minutos da manhã para “acordar” agora seriam por volta de 15 minutos.

Para você (humano deste universo) parece impossível, pois precisa primeiro levantar da cama para depois escovar os dentes, por exemplo. Existe uma ordem para que essas tarefas sejam executadas, ou seja, a tarefa de escovar os dentes fica bloqueada, esperando o processo de levantar da cama acabar e que, no meu caso demora, rsrs.

Existe a possibilidade de fazer algumas tarefas ao mesmo tempo. Por exemplo, imagine que minha maquina de café está programada para fazer café no horário que estou acordando, então uma tarefa como fazer café não fica impedida da tarefa levantar da cama.

Vamos definir dois tipos de tarefas (ou transações):



## Síncrona

Tarefa que impede (*blocking*) você de fazer outra tarefa, pois depende da conclusão de uma terceira tarefa.

```javascript
// nodejs
try {  
    var value = JSON.parse(fs.readFileSync("file.json"));
    console.log(value.success);
}
// Syntax actually not supported in JS but drives the point.
catch(SyntaxError e) {  
    console.error("invalid json in file");
}
catch(Error e) {  
    console.error("unable to read file")
}
```



## Assíncrona

Tarefa que não impede (*non-blocking*) você de realizá-la enquanto outras tarefas são feitas.

```javascript
// nodejs
fs.readFile("file.json", function(error, value) {  
    if ( error ) {
        console.error("unable to read file");
    }
    else {
        try {
            value = JSON.parse(val);
            console.log(val.success);
        }
        catch( e ) {
            console.error("invalid json in file");
        }
    }
});
```

Trabalhar com processos assíncronos não é simples mas vale a pena, já que ganhamos muito com a rapidez. Precisamos agora de ferramentas que nos ajudem a enfrentar os desafios que uma aplicação assíncrona traz.

Hoje existem algumas maneiras que os programadores vem adotando.



## Sistema de callbacks

Este conceito é considerado, por enquanto, o padrão do NodeJS, mas também é conhecido como “**[Callbacks Hell](https://blog.jcoglan.com/2013/03/30/callbacks-are-imperative-promises-are-functional-nodes-biggest-missed-opportunity/)**”.

```javascript
function getUserPage(callback) {  
  fetchUsers(function() {
    renderUsersOnPage(function() {
      fadeInUsers(function() {
        loadUserPhotos(function() {
          // you get the idea…
          // Indicate the User page is done.
          callback(null, page);
        });
      });
    });
  });
}

getUserPage(function (error, page) {  
  // render page when its done.
});
```

Você também pode utilizar uma biblioteca chamada Async.js que ajudará a manter a ordem de execução de cada função, por exemplo:

```javascript
var users = [];  
async.series([  
  fetchUsers,
  renderUsersOnPage,
  fadeInusers,
  loadUserPhotos
]);
```

***Callbacks\*** para realizar tarefas assíncronas nos obriga a sacrificar alguns dos benefícios de quando programamos tarefas síncronas, como por exemplo:

- Funções retornam valores ou exceções (erros) quando concluídas.
- Lançar uma excessão (throw) sem precisar usar try e catch prévio.

Estas questões são causadas porque o conceito de ***callbacks\*** te obriga, de certa maneira, a mudar o *workflow* conhecido, pois agora você passa a ter funções vazias com erros não localizados, dificultando na hora de debugar e entendimento do sistema.



## Promessas em Javascript

O surgimento das promessas nos permitiu escrever sistemas assíncronos sem sacrificar *features* nativas e esperadas pela linguagem, ou seja, escrever código como se fosse síncrono.

Atualmente, graças à comunidade do Javascript, existe uma especificação chamada [“Promises/A”](http://wiki.commonjs.org/wiki/Promises/A) e uma extensão [“Promises/A+”](https://promisesaplus.com/)onde se define o que é necessário para uma biblioteca se auto proclamar baseada em promessas. Vale a pena a leitura!

Uma api de promessas vai vir nativamente no novo [ES6](http://davidwalsh.name/es6-generators), então é bom já ir se acostumando com elas, e também ficar atento a nova funcionalidade “*yield*” que vai possibilitar bibliotecas como [Task.js](http://taskjs.org/) usando *[functions generators](http://davidwalsh.name/es6-generators)*. Mas chega disso e vamos focar nas promessas por agora.

Uma boa definição, feita por Kris Kowal na JSJ, é:

> A promise is an abstraction for asynchronous programming. It’s an object that proxies for the return value or the exception thrown by a function that has to do some asynchronous processing.

Traduzindo, promessa é uma abstração para programação assíncrona, que recebe algum *input* (dados) e retorna uma promessa do *output*(resultado) final. Diferente de um sistema baseado em *callbacks*, onde recebe um *input* e um *callback* (função), que é executado passando algum *output*.

Basicamente, uma promessa deve conter o método “*then*” em 3 estados:

- **Pendente** (*pending*) – Podendo mudar para realizada ou rejeitada.
- **Realizada** (*fulfilled*) – Não pode mudar, e precisa retornar um valor/dado/output.
- **Rejeitada** (*rejected*) – Não pode mudar, e precisa retornar uma razão pela qual a promessa foi rejeitada.

O método “*then*” é executado quando a promessa se encontra no estado “*fulfilled*” (realizada) passando duas funções como argumentos, e retornando uma promessa, possibilitando a criação de uma cadeia de promessas (*promise chaining*) onde a próxima promessa sempre poderá utilizar os resultados das promessas anteriores:

```javascript
getUser(21)  
.then(getStarredRepos(’sebas5384’))
.then(onFulfilled, onRejected);
```

![img](https://i0.wp.com/blog.taller.net.br/content/images/2015/06/promises-1.png?w=700&ssl=1)

Vamos supor que precisamos ler vários arquivos ao mesmo tempo:

```javascript
// nodejs
function readFile(filename, enc) {  
  // Return a promise of the result.
  return new Promise(function (resolve, reject) {
    // Do the async I/O task, reading the file.
    fs.readFile(filename, enc, function (error, result) {
      if (error) {
        // In case of error, reject the promise passing the reason.
        reject(error);
      }
      else {
        // All good, resolve the promise returning the result.
        resolve(result);
      }
    });
  });
}

// Parallel file reading.
Promise.all([  
  readFile('article1.txt', 'UTF-8'),
  readFile('article2.txt', 'UTF-8'),
  readFile('article3.txt', 'UTF-8')
])
.done(function (results) {
  // All the promises where completed.  
}, function (error) {
  // Something goes wrong.
});

// Or in series, in order, one after the other.
readFile('article1.txt', 'UTF-8')  
.then(readFile('article2.txt', 'UTF-8'))
.then(readFile('article3.txt', 'UTF-8'))
.then(function (results) {
  // All the promises where completed.
});
```

Existem [várias bibliotecas](https://promisesaplus.com/implementations) que podemos usar em Javascript, e o conceito se aplica para outras linguagens.





## Exemplo

>  Agora, vamos ver um exemplo utilizando chamadas `ajax` em no lugar da leitura de arquivos.

Para quem vive em Belo Horizonte há um tempo, deve ter percebido que a média de temperaturas está sempre a aumentar. Aos fins de semana, é comum as pessoas irem aos clubes ou até mesmo em sítios nas redondezas, procurando por piscinas ou climas mais amenos. Nesta parte, vamos utilizar uma API de Tempo e verificar a temperatura em três cidades, BH, Ouro Preto e Governador Valadares, encontrando a com menor temperatura e calculando a diferença.

```js
import axios from 'axios';

const BASE_URL = function (id, apiKey) {
	return `https://api.openweathermap.org/data/2.5/weather?id=${id}&APPID=${apiKey}`;
}

// latitude and longitude data for our three cities
const BH = "3470127"; // id da cidade de Belo Horizonte
const GV = "3462315"; // id da cidade de Governador Valadares
const OURO_PRETO = "3455671"; // id da cidade de Belo Horizonte

const getWeather = (cityId, apiKey) => axios.get(BASE_URL(id, apiKey));
```

Um exemplo de chamada para Belo Horizonte (lembrando que as temperaturas estão em Kelvin -> ºC + 273.15 ):

```json
{
  "coord": {
    "lon": -43.94,
    "lat": -19.92
  },
  "weather": [
    {
      "id": 802,
      "main": "Clouds",
      "description": "scattered clouds",
      "icon": "03n"
    }
  ],
  "base": "stations",
  "main": {
    "temp": 294.62,
    "pressure": 1015,
    "humidity": 73,
    "temp_min": 294.15,
    "temp_max": 295.15
  },
  "visibility": 10000,
  "wind": {
    "speed": 4.6,
    "deg": 100
  },
  "clouds": {
    "all": 47
  },
  "dt": 1571798851,
  "sys": {
    "type": 1,
    "id": 8329,
    "country": "BR",
    "sunrise": 1571732422,
    "sunset": 1571777992
  },
  "timezone": -10800,
  "id": 3470127,
  "name": "Belo Horizonte",
  "cod": 200
}
```

A função BASE_URL retorna o endereço da chamada já pronto, com o APPKey e o ID da cidade, bastando apenas realizar o GET através do AXIOS.



# Axios e Promessas

A primeira maneira que vamos ver para realizar várias chamadas assíncronas é utilizando Promessas.

Para recolher as informações para Belo Horizonte é bem simples, basta:

```js
function getBH() {
    getWeather(BH, apikey)
        .then(result => {
            console.log("BH, with promises");
            console.log(result.data);
        })
        .catch(error => console.log(error.message));
}
```

A função `getWeather()` na verdade retorna uma promessa, por isso utilizamos os termos `.then()` - em caso de sucesso - e `.catch()` - em caso de erro.

Para recolher os dados para duas cidades em sequência, vamos aninhar as chamadas à API, de forma que a segunda só aconteça quando a primeira acontecer.

```js
function getBH_OPInSeries() {
    getWeather(BH, apiKey)
        .then(bhData => {
            getWeather(OURO_PRETO, apiKey)
                .then(opData => {
                    console.log("BH and OP, in series");
                    console.log("BH", bhData.data);
                    console.log("OP", opData.data);
                })
                .catch(error => {
                    console.log("Error getting OP...", error.message);
                });
        })
        .catch(error => {
            console.log("Error getting BH...", error.message);
        });
}
```

Ao ver o código acima, talvez fique mais claro como as coisas irão ficar complicadas bem rapidamente. Imagine que em vez de 2 cidades, você precise pegar os dados de 20 cidades, o código ficaria enorme e seria difícil distinguir os `catchs` no final!

Para facilitar a chamada em paralelo então, vamos utilizar um recurso das promessas em JS chamado `Promise.all()`, que irá executar todas as promessas passadas como parâmetro. Para mais informação sobre o método, basta visitar https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/all. 

>  Caso você queira uma resolução de promessas em que apenas uma precise terminar em sucesso, dê uma olhada em Promise.race() -> https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/race.



```js
function getCitiesInParallel() {
    const bhGet = getWeather(BH, apiKey);
    const gvGet = getWeather(GV, apikey);
    const opGet = getWeather(OURO_PRETO, apiKey);

    Promise.all([bhGet, gvGet, opGet])
        .then(([bhData, gvData, opData]) => {
            console.log("All three cities in parallel, with promises");
            console.log(bhData.data);
            console.log(gvData.data);
            console.log(opData.data);
        })
        .catch(error => {
            console.log(error.message);
        });
}
```



# Axios com Async/Await

Uma maneira mais atual de se executar um código assíncrono e esperar pela sua resposta é utilizando os métodos Async/Await nativos do JS. Embora seja bem mais fácil de programar que as promessas, é necessário ter alguns cuidados:

- Uma expressão `await` pausa a execução da função até a resolução da promessa;
- Depois da resolução da promessa, a execução é retomada após o valor ser retornando
- Se um erro acontecer, pode/deve ser tratado com um try... catch
- O operador `await` só pode ser utilizado em funções `async`

Como isso afeta nosso código? Bom, vamos ver como ficaria uma chamada para recolher os dados de BH com Async/Await:

```js
async function getBH() {
    try {
        const bhData = await getWeather(BH, apiKey);
        console.log("Montevideo, with async/await");
        console.log(bhData.data);
    } catch (error) {
        console.log(error.message);
    }
}
```

Embora no fundo nós continuamos utilizando promessas, o código fica um pouco mais simples e fácil de entender. É como se transformássemos uma função assíncrona em uma execução síncrona, já que sempre a esperamos acontecer para continuar a função.

Vamos recolher mais dados então:

```js
async function getBH_GVInSeries() {
    try {
        const bhData = await getWeather(BH, apiKey);
        const gvData = await getWeather(GV, apikey);
        console.log("BH and GB, in series");
        console.log(bhData.data);
        console.log(gvData.data);
    } catch (error) {
        console.log(error.message);
    }
}
```



### Exercício 1

Agora é a sua vez de montar as três chamadas para as cidades, além de calcular as diferenças de temperatura entre elas e exibir na tela.



> https://blog.taller.net.br/o-que-sao-promessas-javascript/




### Problemas com Await/Async

```sh
npm install --save-dev @babel/plugin-transform-runtime
```

Adicionar ao arquivo `.babelrc` (se não existir na raiz do projeto, crie)

```json
{
  "plugins": ["@babel/plugin-transform-runtime"]
}
```

