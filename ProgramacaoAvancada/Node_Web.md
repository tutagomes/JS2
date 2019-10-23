

**Node.js é uma tecnologia assíncrona que trabalha em uma única thread de execução.** Por assíncrona entenda que cada requisição ao Node.js não bloqueia o processo do mesmo, atendendo a um volume absurdamente grande de requisições ao mesmo tempo mesmo sendo single thread.

Imagine que existe apenas um fluxo de execução (chamado de [Event Loop](https://www.luiztools.com.br/post/entendendo-o-nodejs-event-loop/)). Quando chega uma requisição, ela entra nesse fluxo, a máquina virtual Javascript verifica o que tem de ser feito, delega a atividade ([consultar dados no banco](https://www.luiztools.com.br/post/tutorial-crud-em-node-js-com-driver-nativo-do-mongodb/), por exemplo) e volta a atender novas requisições enquanto este processamento paralelo está acontecendo. Quando a atividade termina (já temos os dados retornados pelo banco), ela volta ao fluxo principal para ser devolvida ao requisitante.

Isso é diferente do funcionamento tradicional da maioria das linguagens de programação, que trabalham com o conceito de multi-threading, onde, para cada requisição recebida, cria-se uma nova thread para atender à mesma. Isso porque a maioria das linguagens tem comportamento bloqueante na thread em que estão, ou seja, se uma thread faz uma consulta pesada no banco de dados, a thread fica travada até essa consulta terminar.

Esse modelo de trabalho tradicional, com uma thread por requisição (multi-thread) é mais fácil de programar, mas mais oneroso para o hardware, consumindo muito mais recursos



### Quais as vantagens do Node.js?

**Node.js usa Javascript.** Javascript tem algumas décadas de existência e milhões de programadores ao redor do mundo. Qualquer pessoa sai programando em JS em minutos (não necessariamente bem) e você contrata programadores facilmente para esta tecnologia. O mesmo não pode ser dito das plataformas concorrentes (veja mais adiante).

**Node.js permite Javascript full-stack.** Uma grande reclamação de muitos programadores web é ter de trabalhar com linguagens diferentes no front-end e no back-end. Node.js resolve isso ao permitir que você trabalhe com JS em ambos e a melhor parte: nunca mais se preocupe em ficar traduzindo dados para fazer o front-end se comunicar com o backend e vice-versa. Você pode usar JSON para tudo.

Claro, isso exige uma [arquitetura clara](https://www.luiztools.com.br/post/arquitetura-de-micro-servicos-em-node-js-mongodb/) e um tempo de adaptação, uma vez que não haverá a troca de contexto habitual entre o client-side e o server-side. Mas quem consegue, diz que vale muito a pena.

**Node.js é muito leve e é multiplataforma.** Isso permite que você consiga rodar seus projetos em servidores abertos e com o SO que quiser, diminuindo bastante seu custo de hardware (principalmente se estava usando Java antes) e software (se pagava licenças de [servidor Windows](https://www.luiztools.com.br/post/como-rodar-nodejs-em-servidor-windows/)). Só a questão de licença de Windows que você economiza em players de datacenter como Amazon chega a 50% de economia, fora a economia de hardware que em alguns projetos meus chegou a 80%.



### Quais as desvantagens do Node.js?

**Node.js usa Javascript.** Para quem gosta de linguagens estritas como Java e C# (como eu, confesso), isso incomoda bastante. Não ter orientação à objetos do jeito que estamos acostumados, a tipagem ser fraca, não ter uma compilação bacana, etc.

**Node.js é recente (2009).** Apesar de já ter muita coisa criada pra ele, isso é uma desvantagem em relação à linguagens mais maduras como a JVM (1995) e Python (1991). Mesmo o .NET (2000) já está bastante calejado e maduro, o que nos garante mais confiança para executar projetos maiores e mais pesados.

**Node.js é assíncrono.** Isso complica bastante dependendo da complexidade de cada uma das suas requisições, uma vez que o uso demasiado de callbacks pode gerar o chamado callback hell com um aninhamento extenso de funções e uma complexidade absurda de depuração, o que é parcialmente resolvido com o uso de Promises (ES6) e Async/Await (ES7).



### Para quê serve Node.js?

Serve para fazer o que você quiser, desde sites à scripts de automação. No entanto, usos ideais de Node.js seriam:

**Node.js [serve para fazer APIs](https://www.luiztools.com.br/post/como-criar-uma-api-com-nodejs-parte-1/).** Esse talvez seja o principal uso da tecnologia, uma vez que por default ela apenas sabe processar requisições. Não apenas por essa limitação, mas também porque seu modelo não bloqueante de tratar as requisições o torna excelente para essa tarefa consumindo pouquíssimo hardware.

**Node.js serve para fazer backend de jogos, IoT e apps de mensagens.** Sim eu sei, isso é praticamente a mesma coisa do item anterior, mas realmente é uma boa ideia usar o Node.js para APIs nestas circunstâncias (backend-as-a-service) devido ao alto volume de requisições que esse tipo de aplicações efetuam.

**Node.js serve para fazer aplicações de tempo real.** Usando algumas extensões de web socket com [Socket.io](https://www.luiztools.com.br/post/criando-um-chat-com-nodejs-e-socketio/), [Comet](https://www.npmjs.com/package/comet.io).io, etc é possível criar aplicações de tempo real facilmente sem onerar demais o seu servidor como acontecia antigamente com Java RMI, Microsoft WCF, etc.