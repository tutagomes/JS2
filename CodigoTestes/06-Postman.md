# Automating API’s testing with Postman and Newman



Starting on the point that we have a server exposed by API’s, how can we make sure that it is working properly and making the correct assumptions? Or more, is it giving the correct response to our users and systems?

That’s where API testing enters.

> API testing is used to determine whether APIs return the correct response (in the expected format) for a broad range of feasible requests, react properly to edge cases such as failures and unexpected/extreme inputs, deliver responses in an acceptable amount of time, and respond securely to potential security attacks.
> [https://en.wikipedia.org/wiki/API_testing](https://en.wikipedia.org/wiki/API_testing)

## Before Starting

**API Server**

First of all, we need an API to test. Instead of building a complex server, implementing a lot of endpoints and rules, I will simply use [JSON-Server](https://github.com/typicode/json-server). As it says on its [GitHub](https://github.com/typicode/json-server) page:

> Get a full fake REST API with zero coding in less than 30 seconds (seriously)

**Starting Data**

With our server, we will need some data to begin with. We can generate it with [JSON-Generator](https://www.json-generator.com/), using any fields you may want. As this is a demonstration only, the fields will be simple, like:

    {
     "people" : [
      '{{repeat(7)}}',
      {
        id: '{{index()}}',
        isActive: '{{bool()}}',
        picture: '[http://placehold.it/32x32'](http://placehold.it/32x32'),
        name: '{{firstName()}} {{surname()}}',
        email: '{{email()}}',
        phone: '+1 {{phone()}}',
        greeting: function (tags) {
          return 'Hello, ' + this.name + '! You have ' + tags.integer(1, 10) + ' unread messages.';
        },
        favoriteFruit: function (tags) {
          var fruits = ['apple', 'banana', 'strawberry'];
          return fruits[tags.integer(0, fruits.length - 1)];
        }
      }
     ]
    }

Using this as a generator, it will generate 7 objects containing the fields that you specified, like name, email, phone (as stated above).

Now, simply download that data and start JSON-Server with the command:

    json-server — watch generated.json

It shall instantiate a full REST Server, based on the .json referenced as a parameter. Any requests that you make to the server shall immediately be visible on the file.

![](https://cdn-images-1.medium.com/max/6432/1*4Xu5YqfMJ3Ck_6Zab04tVA.png)

**Verifying our server**

Now, let’s just access [http://localhost:3000/people/0](http://localhost:3000/people/0) and make sure that the data returned is the same as our generated.json. Just for peace of mind. It would be a very simple to test a server that is not even working.

## Starting

To get all testing scripts at only one place, we will create a collection (that will hold all of our requests and tests) and an environment (that will hold all our URLs, credentials, userData, etc).

To give the collection an unmistakeable name, let’s call it Testing JSON-Server. Note that you can call it anything you’d like.

![](https://cdn-images-1.medium.com/max/2204/1*F2lKKbK1gkbik4dYWJ7gHg.png)

![](https://cdn-images-1.medium.com/max/3276/1*tjRptPCxgWno04C7ntk6Fg.png)

Creating an environment that will hold all our URLS and possible data that needs to be shared between tests — You may find tests that need to share data between them, like JWT, auth tokens or even IDs. To store those and make sure that everything is correct according to our environment, we use those environment variables.

![](https://cdn-images-1.medium.com/max/2000/1*i9p3XSI6qvE2UWhJm9M4Zg.png)

![](https://cdn-images-1.medium.com/max/2840/1*w-a5eUlDP5JEeYOR-24FdQ.png)

## Creating Tests

We will first test the GET at localhost:3000/people. It should return all our data stored at the database. Usually on REST servers, calling a collection like this will return you all their data. If it is long, it could also paginate.

First, let’s manually check what is returned by the call (again, we may first see what is expected to be returned prior to writing our tests):

![](https://cdn-images-1.medium.com/max/6480/1*MempLI3vyFykGTZiw0AcmQ.png)

Now we can start writing tests:

**First**

![](https://cdn-images-1.medium.com/max/6504/1*tKZVSBzXjv9NPdPkjzubfw.png)

1. The first test as described is simply a response delay check. As it is local served, the response should be less than 50ms, but this will change if you are calling a far away API or a slower server.

1. The second one is a simple assertion that just verifies if the response code is OK. HTTP (200). You can learn more about HTTP Response codes [here](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).

1. The third one is our data based test. As we generated 7 entities, it is expected to return 7 objects. As they are random, we cannot check here if it contains a name or an email, for instance.

1. The fourth one is our sanity check. As it contains 7 objects, it should not be less than 6. (this one is just an example of a negative assertion).

**Second**

Now we will create, read, update and delete an object with API calls and test if it will work as expected.

![](https://cdn-images-1.medium.com/max/2000/1*24QGXQgQRiDUGCl-JO4HIQ.png)

Note that after our insertion, we need to save the ID that was returned, as stated in the script below.

![](https://cdn-images-1.medium.com/max/2452/1*cUi3Eulrccho8QYnZBmObQ.png)

And then we can simply access it with {{id}}, the same way as we access the environment variable called url with {{url}}. Note that setting a variable is not considered an assertion, so that line of code should not appear on our tests results.

**Others**

All the other tests (collections and environments) are available at my GitHub repo [here](https://github.com/tutaarthur/Postman-Testing). You will find a full example of testing a CRUD with Postman.

## Automating tests with [Newman](https://www.npmjs.com/package/newman)

Imagine having a command to automatically run all of our previously written tests. That can give you feedback about it and possibly automate it with any program that can trigger a command, like Jenkins or Azure DevOps.

![](https://cdn-images-1.medium.com/max/6936/1*1M-QXSQ8GfuEhwsDabmVtg.png)

So you won’t need to open Postman and manually trigger those tests. You can simply use Newman.

First, we need to export the Environment and Testing Collection that we created. After this, we need to install **newman** as a global package, to be able to initiate it from every folder.

    npm install -g newman

Then we may start our tests, passing the test collection and environment files as parameters. Note that you can have various environment files, with each one pointing to a server, like development and production. Just don’t forget to point the right one when starting the test.

    newman run collection.json -e environment.json

![](https://cdn-images-1.medium.com/max/3804/1*Ws9DXG5WQsrkVec8PC7-ZQ.png)

After those tests had been run, it gives you a nice and clean result of every assertion that we wrote. Note that it even cares about turning the test name color to green!

And you can use the command with the bail flag so it returns a 1 instead of a 0 if your test fails, so you can automate it and give a feedback to your team.

    newman run collection.json -e environment.json --bail

Of course, everything else about Newman can be found at [https://learning.getpostman.com/docs/postman/collection_runs/command_line_integration_with_newman/](https://learning.getpostman.com/docs/postman/collection_runs/command_line_integration_with_newman/)

As usual, all the files used in this “tutorial” are available at [https://github.com/tutaarthur/Postman-Testing](https://github.com/tutaarthur/Postman-Testing)



## Exercício

Utilizando o mesmo servidor dos encontros anteriores, vamos adicionar um novo trecho ao código:

```js
app.get("/delay/:ms", (req, res) => {
  res.writeHead(200, {'Content-Type': 'application/json'})
  res.write('{ "message": ');
  setTimeout(() => {
    res.end('"Im alive!" }')
  }, req.params.ms)
})
```

Por exemplo, ao enviar uma requisição para `http://localhost:8080/

![image-20191124195925298](assets/image-20191124195925298.png)

E agora, vamos testar o funcionamento da API nos seguintes aspectos:

- Ao entrar com usuário inválido, deve ser gerado um erro
- Ao entrar com usuário válido deve ser retornado um Token
  - Obs: necessário guardar o Token em uma variável para que seja possível acessá-lo depois
- Ao recolher Tarefas, deve ser retornado um JSON válido
- Ao recolher Usuarios, deve ser retornado um JSON válido
- Deve ser possível adicionar tarefa através do método POST
- Deve ser possível editar apenas algumas informações através do método PATCH
- Deve ser possível excluir a tarefa através do método DELETE
- Verificar se a tarefa foi excluída
