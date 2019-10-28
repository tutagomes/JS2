# Consulta

Agora que você chegou até aqui, vamos juntar os dois projetos.

>  Quais dois projetos?

O projeto que criamos na aula anterior que consumia um servidor REST!

Vamos apenas implementar um gateway que utiliza o Express para redirecionar chamadas ao JSON-Server.

Essas rotas devem estar protegidas com um JWT, como vimos anteriormente. Também será necessário adicionar campos para que o usuário faça login.

Uma dica: antes de inicializar qualquer chamada, peça ao usuário pelo login/senha e armazene o JWT no escopo global, que possa ser utilizado por todas as requests! Ou se preferir, também pode especificar para o `axios` utilizar sempre o mesmo header, com a linha:

```js
axios.defaults.headers.common['Authorization'] = 
                                'Bearer ' + jwtToken);
```

