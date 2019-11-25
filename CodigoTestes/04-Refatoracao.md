## O que é Refatoração?

Em uma tradução livre do livro:

> “*Uma mudança feita na estrutura interna do software para deixá-lo simples de entender e alterar, sem modificar o comportamento observado.”*

Na prática refactoring é um processo de **pequenas mudanças no código**, visando a melhora do design do software, **sem mudar o comportamento** observado ao inicio do processo. A grande vantagem é que ao final de várias iterações, sejam elas uma em seguida da outra ou ao longo de semanas, você possui um **grande impacto na qualidade de código** do seu produto.

Existem três etapas básicas em uma refatoração: **modificar, testar, commitar**. Cada iteração gera uma base de código funcional que se comporta como antes (comprovadamente por testes), porém com a vantagem de possuir um design melhor do que antes. Caso você quebre algum teste, é só voltar para o último commit, e ta lá! Sua base de código está funcional novamente.

## Mas por que eu preciso refatorar?

Sejamos sinceros, **raramente código nasce com o melhor design possível**. Duvido que você nunca olhou para um código que fez há um mês atrás e viu várias melhorias quanto ao design (sendo que no dia que fez achou ele lindão), e isto é natural.

Além disso, existe uma **forte tendência de a qualidade de código decair ao longo da vida do produto**. Na correria do dia a dia código é duplicado, stack overflow é copiado, boas práticas são negligenciadas. Existe uma desculpa muito comum para descuidar do design do software: *não dá tempo*. A verdade é que tempo você não vai ter quando tiver que lidar com a enorme dívida técnica deixada para trás. Uma hora hoje pode salvar uma semana amanhã.

![img](https://miro.medium.com/max/30/0*y846xK1UdJHTiQTc?q=20)

![img](https://miro.medium.com/max/622/0*y846xK1UdJHTiQTc)

*Adicionar funcionalidade e dar manutenção em um código de bom design é mais fácil e mais rápido.*

Alguns de vocês estão pensando agora “Pfff, eu não preciso disso, sou um arquiteto de software fodástico. Sempre faço o melhor design do mundo e sempre prevejo tudo que poderá acontecer no meu projeto. Comigo não tem essa de design ruim”.

**Don’t do that.** [**You aren’t gonna need it (YAGNI)**](https://martinfowler.com/bliki/Yagni.html)

Prever código é deixar código parado. Você até pode dizer que ele está em produção, mas, de fato não está produzindo nada. Foque em um código simples, coeso, pouco acoplado, que represente o seu domínio — mas **somente o necessário**. Quando você precisar daquele parâmetro, você adiciona.

O refactoring está diretamente ligado ao conceito de YAGNI. Software incremental precisa de etapas de refatoração para continuar com o seu design bem cuidado.

## Beleza, e quando refatorar?

O refactoring faz parte da adição de uma nova funcionalidade ou da correção de um problema. É intrínseco ao processo de desenvolvimento, não algo para ser orçado a parte. Refactoring planejado, aquele que você possui um card no kanban, deve ser evitado.

Passou por um código de seu interesse que está ruim de ler? **Refatore**. Achou uma oportunidade de melhorar o nome de alguma variável, método, atributo? **Refatore**. Observou um acoplamento desnecessário? **Refatore**. Precisou duplicar algum código? **Refatore**. Realizou code review e o código não ficou claro para o revisor? **Refatore**.

Enfim, ao observar oportunidades de melhorar o design durante o desenvolvimento, **refatore**.

Aliás, aquela desculpa de “*Não tenho tempo*” é quebrada facilmente. Refactoring não muda comportamento, logo você não passa tempo debugando. Na técnica pregada por Martin Fowler o código passa pouco tempo quebrado, podendo parar a qualquer momento para que outras iterações sejam feitas posteriormente.

## Então refactoring é solução para todos os problemas?

**Refatorar não é fácil**. Exige maturidade de desenvolvimento, capacidade analítica para melhorar sempre o design do código. A cada iteração você aprende um pouco mais, além de poder absorver muito conhecimento em code review. É necessário praticar e ser auto crítico.

Em times maiores, onde refactoring é cultural, a integração de código pode se tornar mais complexa. Uma vez que sua base de código está em constante mudança manter diferentes branches se torna um desafio. Uma saída para diminuir essa complexidade é usar [**Integração Contínua (Continuos Integration — CI).**](https://martinfowler.com/articles/continuousIntegration.html)

Sem testes automatizados, não há refactoring. Apesar de parecer radical, essa afirmação é um fato. O conceito de refatoração diz que ao final de uma iteração você tem que comprovar que o comportamento observado foi mantido. Como fazer isso sem testes? Você pode até melhorar o design do seu código sem testes, mas esse não é um processo de refactoring. Uma boa maneira de sempre manter código bem testado é utilizando [**Test Driven Development (TDD)**](https://martinfowler.com/bliki/TestDrivenDevelopment.html), além de inúmeras outras vantagens que esta técnica trás.

A curva inicial para refatorar código legado geralmente é alta. Isso acontece pois esses códigos são bem acoplados, o que acaba deixando complicado de alterar algo apenas em um lugar. Mas o lado bom é que a alta curva inicial se paga no futuro. **O primeiro passo para pagar uma divida é deixar de aumentá-la**, e isto sempre será difícil no começo. E existe sempre aquela situação também: as vezes reescrever um código é mais produtivo do que refatorar.

Existem sim refactorings mais complicados dos que outros. **Refatorar base de dados é difícil**, exige cuidado. Uma alteração básica do nome de uma coluna envolve mais de um deploy. É um assunto complexo, inclusive com livro dedicado para o tema — [Refactoring Databases](https://martinfowler.com/books/refactoringDatabases.html).

## Então eu desisto?

Não desanime. Refactoring é um processo incremental que traz inúmeras vantagens. **Ter um código bem cuidado** vale a pena. **Entregar valor cedo e construir software incremental** vale a pena. As técnicas citadas para evitar problemas com a refatoração são muito valiosas também. Quanto mais você pesquisa sobre desenvolvimento e qualidade de software mais você percebe como técnicas são interligadas e necessárias para o crescimento saudável de um projeto.

Como uma última dica, dê uma olhada no [Extreme Programming (XP)](http://www.extremeprogramming.org/). Essa metodologia ágil, criada por [Kent Beck](https://twitter.com/kentbeck) (coautor do livro Refactoring), prega práticas e técnicas muito interessantes para uma entrega de valor rápida e com qualidade. Spoiler alert: quase tudo que foi dito aqui é englobado pelo XP.



## Exercício

Com o código fonte do repositório [Exemplo-Angular](https://github.com/tutagomes/exemplo-angular), refatore o código dos serviços e componentes, tendo em mente:

- Padrão de nomenclatura:
  - camelCase
  - snake_case
  - kebak-case
  - PascalCase
  - UPPER_CASE

- Padrão de nomenclatura de função:
  - Utilizar verbos para denominar ações
-  Código duplicado;
- Métodos e classes extensas;
-  Lista de parâmetros longa;
-  Má indentação;
-  Uso adequado de Strings;
-  Obsessão por tipos primitivos (*Primitive Obsession*);
-  Substituição de constantes por Enums;
-  Utilização de literais em tipos numéricos.
  - Trocar as URLS de requisição para utilizar as variáveis de ambiente

```typescript
import { environment } from '../../environments/environment';
.
.
.
environment.apiUrl
```

