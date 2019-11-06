## Exercício Final

Agora que já vimos como o TypeScript funciona, incluindo a documentação, vamos reimplementar o nosso gerenciador de tarefas utilizando suas facilidades.

Como assim reimplementar? Vamos definir interfaces de Tarefa e User, já que agora cada tarefa tem um dono.

O código do servidor e do cliente já está parcialmente implementado em JS, portanto, basta baixar do Google Classroom o Zip (Comeco_Exercicio_TypeScript.zip) e subir a interface e o Servidor. Não se esqueça de instalar as dependências com o comando `npm install` (as dependências de teste, documentação e TypeScript já estão instaladas e configuradas).

O objetivo deste exercício é que seja possível adicionar ou remover tarefas para um usuário qualquer (não se preocupe com a autenticação).

**Não se esqueça de documentar seus métodos!** 



### Sobre a API

A implementação do Servidor contempla um Json-Server embutido, contendo uma base de dados de tarefas e usuários já inseridos.

Para buscar tarefas de um usuário específico, basta acessar a URL:

```html
http://localhost:8080/api/users/{userId}/tarefas
```

> O comando POST também está disponível para a URL acima.