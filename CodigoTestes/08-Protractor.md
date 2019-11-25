## Protractor

Testes E2E servem para automatizar a interação com aplicações web tal como um usuário faria, e o framework [Protractor](http://www.protractortest.org/#/) foi desenvolvido para viabilizar isso.

Como inicialmente foi projetado para aplicações com o falecido [AngularJS](https://angularjs.org/) — que deus o tenha — , ele também é executado em [Node](https://nodejs.org/en/), utilizando o [WebDriverJS](http://www.webdriverjs.com/) para manipulação dos elementos da página web.

Ainda que seu desenvolvimento tenha os alicerces no [Angular](https://angular.io/guide/quickstart), no qual se baseia este artigo, o Protractor pode também ser utilizado em conjunto a outros tipos de aplicações, uma vez que fornece além dos seletores Angular (*binding*, *model*, *repeat* e afins) os populares de atributos HTML como *id* e *class*.

De maneira enxuta e superficial, o fluxo ocorre da seguinte maneira (mais adiante falarei mais a respeito de cada termo):

1. Início da execução;
2. O protractor instância um servidor [Selenium](https://www.seleniumhq.org/) para os testes;
3. O arquivo de configuração (*configuration file)* é lido*;*
4. É realizado a busca dos arquivos de teste (*spec’s)* para execução;
5. Ao final, é apresentado os resultados dos testes.

#### Selenium, é de comer? 🤤

Não. *Keep calm young*… Em resumo, a Selenium nada mais é do que uma ferramenta que o Protractor utiliza para emular/reproduzir um browser, abstendo o desenvolvedor de ficar reinventando a roda para executar a automatização de “coisas”, como os testes e2e, por exemplo.

#### O que é o tal do configuration file? 🙄

Devido ao Protractor estar vinculado ao Angular, na criação de um novo projeto é gerado, por *default,* um arquivo chamado **protractor.conf.js** (figura 1) na raiz do projeto — bem como a pasta /e2e contendo alguns testes simplórios — , que como o próprio nome indica, contém as configurações que serão utilizadas para execução dos testes.

![img](https://miro.medium.com/max/60/1*oJnGkqN-DB06ZwVKRcWW1A.png?q=20)

![img](https://miro.medium.com/max/969/1*oJnGkqN-DB06ZwVKRcWW1A.png)

Figura 1 — arquivo de configuração do Protractor em um projeto [Angular](https://angular.io/guide/quickstart).

#### E o spec file? 🤔

Conforme comentado anteriormente, existe um diretório gerado na criação de um novo projeto (/e2e). Nele, encontra-se o arquivo que executa de fato os casos de testes. Em uma nova aplicação, o arquivo será o **app.e2e-spec.ts** (figura 2), com o seguinte conteúdo:

![img](https://miro.medium.com/max/60/0*wHFhW76Y-6V75mu9?q=20)

![img](https://miro.medium.com/max/495/0*wHFhW76Y-6V75mu9)

Figura 2 — arquivo app.e2e-spec.ts.

#### Certo, mas e a page object, para que serve mesmo? 🤨

Por fim, mas não menos importante, a *PO* (*Page Object* — figura 3). Ela possui os recursos necessários para capturar os elementos que serão utilizados nos casos de testes.

Recomenda-se sua utilização para que, caso ocorra a mudança do nome de classes, *id’s* e afins no template (HTML) haja mais facilidade na manutenção. Outro benefício dessa **boa prática** é a reutilização dos métodos de acesso do elemento, que poderão diminuir drasticamente a quantidade de código, se bem aplicados.

![img](https://miro.medium.com/max/60/0*O2tqDyqGBJW6obUf?q=20)

![img](https://miro.medium.com/max/423/0*O2tqDyqGBJW6obUf)

Figura 3 — arquivo app.po.ts.

------



#### E falando em boas práticas… 🤓

É certo que o uso de boas práticas na codificação traz diversos benefícios, tanto que não ficarei abordando aqui sobre esse assunto e irei direto ao ponto! A seguir, deixo uma lista de práticas recomendadas por diversos autores contemporâneos do mundo *e2e* e que sempre que tenho alguma dúvida se estou seguindo o caminho certo, releio-os. Abaixo estão os quais levo como meus Dez Mandamentos E2E:

> I. Não criarás testes E2E para funcionalidades que contemplam testes unitários;
>
> II. Utilizarás apenas um arquivo de configuração;
>
> III. Estruturarás e agruparás os testes de maneira que faça sentido ao projeto;
>
> IV. Darás preferência aos localizadores de elementos do [Protractor](http://www.protractortest.org/#/api?view=ElementArrayFinder.prototype.count);
>
> V. Evitarás a utilização de textos estáticos e Xpath’s para capturar elementos;
>
> VI. Sempre farás o uso de PO’s — sempre mesmo, blz?;
>
> VII. Declararás funções para operações que realizam mais de um passo. Por mais que seja um dos conceitos básicos de clean code, é válido ressaltar;
>
> VIII. Criarás Wrappers quando houver elementos utilizados por diversos testes, que nada mais são do que um sinônimo de PO, porém, de maneira que esteja num escopo global;
>
> IX. Restabelecerás o estado original da página que testada a cada it (caso de teste);
>
> X. Criarás casos de testes totalmente independentes uns dos outros, caso contrário, muito em breve seus testes começarão a quebrar e você nem saberá o por quê!



## Testando tela de login

Primeiro, vamos ver as principais formas de selecionar elementos na tela e enviar comandos:



###### Cheatsheet for accessing elements:

```
element(by.id('firstName'))

element(by.css('.signout'))

element(by.model('address.city')) 

element(by.binding('address.city')); 

element(by.input('firstName'));

element(by.input('firstName')).clear();

element(by.buttonText('Close'));

element(by.partialButtonText('Save'));

element(by.linkText('Save'));

element(by.partialLinkText('Save'));

element(by.css('img[src*='assets/img/profile.png']')); element(by.css('.pet .cat'));element(by.cssContainingText('.pet', 'Dog'));

allColors = element.all(by.options('c c in colors'));
```



###### Cheatsheet for typing (sendKeys):

```
element(by.id('firstName').sendKeys("John");

sendKeys(Key.ENTER);

sendKeys(Key.TAB);sendKeys(Key.BACK_SPACE)element(by.id('user_name')).clear()
```



###### Cheatsheet for collection:

```
var list = element.all(by.css('.items));

var list2 = element.all(by.repeater('personhome.results'));

expect(list.count()).toBe(3);

expect(list.get(0).getText()).toBe('First’)

expect(list.get(1).getText()).toBe('Second’)

expect(list.first().getText()).toBe('First’)

expect(list.last().getText()).toBe('Last’)
```



### Exercício

Vamos ver um teste simples de tela com login/logout

```typescript
import { AppPage } from './app.po';
import { browser, element, by } from 'protractor';

describe('workspace-project App', () => {
  let page: AppPage;

  beforeEach(() => {
    page = new AppPage();
  });

  it('Should display login if not authenticated', () => {
    page.navigateTo();
    expect(browser.getCurrentUrl()).toContain('login');
  });

  it('Should redirect if successfully logged', () => {
    page.navigateTo();
    expect(browser.getCurrentUrl()).toContain('login');
    
    let userInput = element(by.css('[ng-reflect-name=username]'));
    userInput.sendKeys('usuario');
    expect(userInput.getAttribute('value')).toBe('usuario');
    
    let passwordInput = element(by.css('[ng-reflect-name=password]'));
    passwordInput.sendKeys('password');
    expect(passwordInput.getAttribute('value')).toBe('password');

    let loginButton = element(by.css('#loginButton'));
    loginButton.click();
    browser.waitForAngular();
    expect(browser.getCurrentUrl()).not.toContain('login');
    expect(element(by.css('#tarefa_titulo')).isPresent()).toBe(true);

    element(by.css('#logoutButton')).click();
    browser.waitForAngular();
    expect(browser.getCurrentUrl()).toContain('login');
  });

  it('Should show Error if cannot login', () => {
    page.navigateTo();
    expect(browser.getCurrentUrl()).toContain('login');
    
    let userInput = element(by.css('[ng-reflect-name=username]'));
    userInput.sendKeys('12123');
    expect(userInput.getAttribute('value')).toBe('12123');
    
    let passwordInput = element(by.css('[ng-reflect-name=password]'));
    passwordInput.sendKeys('password');
    expect(passwordInput.getAttribute('value')).toBe('password');

    let loginButton = element(by.css('#loginButton'));
    loginButton.click();
    browser.waitForAngular();
    expect(browser.getCurrentUrl()).toContain('login');
    expect(element(by.css('#tarefa_titulo')).isPresent()).toBe(false);
  });

  afterEach(async () => {
    // Assert that there are no errors emitted from the browser
    /* const logs = await browser.manage().logs().get(logging.Type.BROWSER);
    expect(logs).not.toContain(jasmine.objectContaining({
      level: logging.Level.SEVERE,
    } as logging.Entry)); */
  });
});

```

Agora, tente criar casos de teste para verificar o funcionamento da tela de tarefas, verificando se os componentes são mostrados corretamente e também se tarefas são adicionadas/removidas de forma adequada.

Você pode navegar até a tela de tarefas após o login utilizando o comando:

```typescript
browser.get('/tarefas');
```

Link de referência ao Protractor https://www.protractortest.org/#/api?view=ProtractorBrowser