### Pull Request

![bitbucket411-blog-1200x-branches2](https://img.mandic.com.br/blog/2017/07/bitbucket411-blog-1200x-branches2.png)

Quando você tem várias pessoas trabalhando em cima de uma mesma base de arquivos texto, o ideal é que cada pessoa tenha uma cópia do código para trabalhar. Não acredite apenas porque eu estou dizendo, tente abrir *online* um arquivo .doc compartilhado e editar ele simultaneamente com várias outras pessoas. Depois tome um banho de sal grosso.

Para organizar o trabalho em equipe, foi criado há muito tempo uma coisa chamada "controle de versão". Até hoje são populares alguns sistemas antigos, como o Source Safe da Microsoft e o SVN.

Os sistemas de controle de versão que usam git em geral têm como diferencial em relação aos sistemas mais antigos uma funcionalidade bem específica, essa chamada `pull request`. É basicamente uma forma de você dizer: acabei o meu trabalho, preciso integrá-lo no ramo principal e eis aqui o que eu fiz. Ao utilizar essa funcionalidade, é gerado um registro no servidor (por exemplo, o *GitHub*, que é bem popular) que destaca as diferenças de código entre o ramo no qual você trabalhou e o ramo alvo da entrega. Assim fica fácil para o administrador do repositório revisar o seu trabalho. Também dá para iniciar discussões sobre trechos específicos das alterações. Por fim, o próprio git facilita a mesclagem do código.

Isso incentiva a revisão em pares e o trabalho ordenado em equipe, e no caso de projetos open source incentiva também a contribuição de por parte da comunidade.

![pull-illustration](https://railsware.com/blog/wp-content/uploads/2018/09/pull-illustration.jpg)

https://www.digitalocean.com/community/tutorials/como-criar-um-pull-request-no-github-pt

https://blog.da2k.com.br/2015/02/04/git-e-github-do-clone-ao-pull-request/