# Adicionando Autenticação com JWT

Para qualquer aplicação baseada em servidor, um problema que sempre surge é sobre a autenticação. Em páginas web tradicionais, sessões e cookies podem ser utilizadas mas, se você estiver usando uma API, não há garantia de que os pedidos virão de um navegador; na verdade, eles podem muito bem vir de outro servidor. Acrescentando isto ao fato de que o HTTP é stateless, e que os serviços RESTful também são planejados para ser assim, precisamos de outro mecanismo, e **JSON Web Tokens****(**JWT***) é uma solução adotada por muitos desenvolvedores.

> JWT is sometimes read aloud as *JOT*; see Section 1 of the RFC at https://www.rfc-editor.org/info/rfc7519.

A ideia com a JWT é que o usuário primeiro troque credenciais válidas (como nome de usuário e senha) com um servidor e recupere um token, que depois servirá como chave de autenticação. Os Tokens são criados usando métodos criptográficos e são muito mais longos e seguros do que as senhas usuais. No entanto, os Tokens são suficientemente pequenos para serem enviados como parâmetros do corpo ou como um cabeçalho HTTP (o mais utilizado). 

>Enviar o token na URL como um parâmetro é considerada uma péssima prática de segurança! E dado que o token não é realmente uma parte de uma solicitação, também não é recomendado colocá-lo no corpo da requisição. Portanto, sempre opte por colocá-lo nos cabeçalhos da chamada

Depois de obter o token ele deve ser enviado no cabeçalho em cada chamada de API, e o servidor irá verificá-lo antes de continuar. O token pode incluir todas as informações sobre o usuário para que o servidor não tenha que consultar um banco de dados novamente para revalidar a solicitação. Nesse sentido, um token funciona como os passes de segurança que você recebe na recepção de um prédio restrito; você tem que provar sua identidade uma vez para o oficial de segurança, mas depois você pode mover-se pelo prédio mostrando apenas o passe (que será reconhecido e aceito) ao invés de ter que passar por todo o processo de identificação de novo e de novo e de novo. 

> Check out https://jwt.io/ for online tools that allow you to work with JWT, and also lots of information about tokens.



### Vamos para a programação

Vamos ver como podemos adicionar autenticação ao nosso servidor. Para trabalhar com JWT, vamos utilizar a biblioteca jsonwebtoken de https://github.com/auth0/node-jsonwebtoken:

```bash
npm install jsonwebtoken --save
```

Nosso código para JWT será maior do que os laboratórios anteriores e normalmente é separado em muitos arquivos (mas resolvi reduzir ao máximo o número para facilitar na implementação). Primeiro, precisamos fazer algumas declarações:

```js
/* @flow */
"use strict";

const express = require("express");
const app = express();
const jwt = require("jsonwebtoken");
const bodyParser = require("body-parser");

const validateUser = require("./validate_user.js");

const SECRET_JWT_KEY = "modernJSbook";

app.use(bodyParser.urlencoded({ extended: false }));
```

Quase tudo que vamos utilizar aqui é uma implementação padrão da biblioteca, exceto a função validateUser() e a string SECRET_JWT_KEY. O último será usado para assinar os tokens e definitivamente não deve estar no próprio código! (Se alguém invadir o código fonte do servidor, sua chave estaria exposta e seria possível então emitir tokens para qualquer usuário; em vez disso, é uma boa prática definir a chave em uma variável de ambiente e obter o valor de lá).

Para facilitar a implementação da verificação do usuário, vamos utilizar um `userName` e `password` padrões, no caso, `usuário` e `password`.

```js
/* @flow */
"use strict";

/*
    In real life, validateUser could check a database,
    look into an Active Directory, call another service,
    etc. -- but for this demo, let's keep it quite
    simple and only accept a single, hardcoded user.
*/

const validateUser = (
    userName: string,
    password: string,
    callback: (?string, ?string) => void) => {
    if (!userName || !password) {
        callback("Missing user/password", null);
    } else if (userName === "usuario" && password === "password") {
        callback(null, "usuario"); // OK, send userName back
    } else {
        callback("Not valid user", null);
    }
};

module.exports = validateUser;
```

Vamos voltar à implementação do servidor. Após as definições iniciais podemos colocar as rotas que não necessitam de autenticação. Vamos ter uma rota /public, e também uma rota /gettoken para obter um JWT. Neste último, veremos se o POST inclui valores de userName e password no seu corpo e se os valores correspondem aos definidos através da função validateUser(). Caso ocorra algum problema na validação do usuário, é retornado um status 401. Caso o usuário e senha estejam corretos, um token será criado com tempo de expiração de um hora.

```js
app.get("/public", (req, res) => {
    res.send("the /public endpoint needs no token!");
});

app.post("/gettoken", (req, res) => {
    validateUser(req.body.user, req.body.password, (idErr, userid) => {
        if (idErr !== null) {
            res.status(401).send(idErr);
        } else {
            jwt.sign(
                { userid },
                SECRET_JWT_KEY,
                { algorithm: "HS256", expiresIn: "1h" },
                (err, token) => res.status(200).send(token)
            );
        }
    });
});
```

Agora que as rotas desprotegidas estão prontas, vamos adicionar a verificação de token ao middleware. Esperamos, de acordo com a RFC JWT, ter no cabeçalho a propriedade: `Authorization: Bearer ${TOKEN}`. Se tal cabeçalho não estiver presente, ou se não estiver no formato correto, um status 401 será enviado. Se o Token estiver presente, mas tiver expirado ou tiver qualquer outro problema, um status 403 será enviado. Finalmente, se não houver nada de errado, o campo userid será extraído das propriedades do Token e anexado ao objeto request para que outras partes do código possam utilizá-lo:

```js
app.use((req, res, next) => {
    // First check for the Authorization header
    const authHeader = req.headers.authorization;
    if (!authHeader || !authHeader.startsWith("Bearer ")) {
        return res.status(401).send("No token specified");
    }

    // Now validate the token itself
    const token = authHeader.split(" ")[1];
    jwt.verify(token, SECRET_JWT_KEY, (err, decoded) => {
        if (err) {
            // Token bad formed, or expired, or other problem
            return res.status(403).send("Token expired or not valid");
        } else {
            // Token OK; get the user id from it
            req.userid = decoded.userid;
            // Keep processing the request
            next();
        }
    });
});
```

Agora, vamos ter algumas rotas protegidas (de fato, uma única, /private, apenas para este exemplo), seguidas de verificação de erros e configuração de todo o servidor:

```js
app.get("/private", (req, res) => {
    res.send("the /private endpoint needs JWT, but it was provided: OK!");
});

// eslint-disable-next-line no-unused-vars
app.use((err, req, res, next) => {
    console.error("Error....", err.message);
    res.status(500).send("INTERNAL SERVER ERROR");
});

app.listen(8080, () =>
    console.log("Mini JWT server ready, at http://localhost:8080/!")
);
```

Tudo pronto! Agora vamos ver o funcionamento.



# Executando

Podemos começar testando as rotas `/public` e `/private` sem nenhum token e verificar o resultado:

```js
> curl "http://localhost:8080/public"  
the /public endpoint needs no token!

> curl "http://localhost:8080/private" 
No token specified
```

Agora, vamos tentar obter um token e adicionar ao cabecalho

```js
> curl http://localhost:8080/gettoken -X POST -d "user=usuario&password=password"     
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyaWQiOiJma2VyZWtpIiwiaWF0IjoxNTI2ODM5MDEwLCJleHAiOjE1MjY4NDI2MTB9.cTwpL-x7kszn7C9OUXhHlkTGhb8Aa7oOGwNf_nhALCs
```

Ao verificar o Token através do site [https://jwt.io obtemos as seguintes informações:

```json
{
  "userid": "usuario",
  "iat": 1526839010,
  "exp": 1526842610
}
```

O atributo `iat` carrega a informação de quando o token foi emitido e o atributo `exp` a informação de quando o token expira.  Se repetirmos agora o comando curl para `/private`, mas adicionando ao cabeçalho o token, obteremos uma resposta de sucesso. Se você esperar (pelo menos uma hora!), o resultado será diferente; o middleware de verificação JWT detectará que o token está expirado e um erro 403 será produzido.

```json
> curl "http://localhost:8080/private" -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyaWQiOiJma2VyZWtpIiwiaWF0IjoxNTI2ODM5MDEwLCJleHAiOjE1MjY4NDI2MTB9.cTwpL-x7kszn7C9OUXhHlkTGhb8Aa7oOGwNf_nhALCs"
the /private endpoint needs JWT, but it was provided: OK!
```

