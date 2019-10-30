

### 1. Não dependa só das cores!

Cores são ferramentas poderosas que normalmente utilizamos para expressar marcas, mensagens, dar ênfase e até comunicar com o usuário. Mas claro, não podemos depender somente delas.

Por exemplo, é de conhecimento geral de que "verde" significa correto e "vermelho" significa errado. Mas o que acontece quando utilizamos somente essas informações como métodos de retorno para o usuário?

Na imagem abaixo, podemos ver o que uma pessoa com daltonismo veria! É perceptível que apenas as cores não trazem informações relevantes para pessoas que não conseguem distinguí-las.



![img](https://aerolab.cdn.prismic.io/aerolab/3b666159ba679809aebdb2c5715005d1120d7677_01-web-accessibility-color-blindness.png)



Se utilizarmos somente as cores para dar feedback aos usuários, estaremos deixando aproximadamente 4,5% da população sem feedback!

É então uma boa prática adicionar outros elementos visuais além das cores para informar o estado da aplicação para o usuário, como no caso abaixo, é utilizado também ícones que transmitem a ideia de correto/incorreto:

![img](https://aerolab.cdn.prismic.io/aerolab/3a6b205b2f8597f25f634a0ad55ebcb1774f64c6_02-web-accessibility-color-blindness.png)



Uma solução interessante foi implementada pelo site [CanIUse](https://caniuse.com/#), que permite o usuário alterar entre cores de acessibilidade e cores default.



![img](https://aerolab.cdn.prismic.io/aerolab%2F704e8b36-af3e-4695-9bd1-98ad8eb38fd5_03-web-accessibility-caniuse.gif)



Para verificar se a paleta de cores está acessível também para pessoas com daltonismo, é possível utilizar a extensão:

[Color Blinding](https://chrome.google.com/webstore/detail/colorblinding/dgbgleaofjainknadoffbjkclicbbgaa?authuser=0)





### 2. Não desabilite o Zoom

Na era dos sites responsivos, é comum os desenvolvedores cometerem alguns erros na hora de implementar as páginas (principalmente em dispositivos móveis).

Um destes erros é utilizar a propriedade `maximum-scale=1.0` na metaTag do `viewport`, que desabilita a funcionalidade de zoom em dispositivos móveis.

```html
<meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1">
```

A funcionalidade de zoom não é somente uma guideline do WCAG, mas também ajuda a simplificar a vida de pessoas que só querem ler as letras pequenas em certos sites.



### 3. Utilize a propriedade `alt` 

Não importa há quanto tempo você está desenvolvendo websites, você pode se surpreender ao saber essas poucas dicas sobre o famoso, mas misterioso, atributo alt.

1. O atributo **alt** é obrigatório para cada **tag** **img** mas um atributo **alt** vazio é completamente válido. Se uma imagem é decorativa ou não é necessária para entender o conteúdo da página você pode simplesmente usar alt="".
2. Os leitores de tela dizem ao usuário que uma tag *<*img> é uma imagem, então não há necessidade de ser redundante e começar seu **alt** com "Imagem de"; basta ir direto ao ponto.
3. A função de uma imagem é tão importante quanto o seu significado: se o seu logotipo é um link para  página inicial do seu site, então o seu texto alt deve ser algo como "Home Page" em vez de "Logotipo".
4. O texto alternativo não é apenas uma questão de acessibilidade. Às vezes, usuários com conexões de dados lentas desabilitam imagens para obter uma experiência de navegação mais rápida. Tenha esses usuários em mente sempre que você escrever seus atributos alt também!

Mas nem todas as imagens no seu site são img tags, certo? Você pode ter um SVG ou dois por aí... ou um sistema completo de ícones SVG.

Como tornamos o SVG acessível a todos? Felizmente para nós, o padrão **Scalable Vector Graphics** já tem isso implementado! Para descrever nossos vetores, temos as tags <title> e <desc> para descrições curtas e longas.

```html
<symbol id="langIcon">
    <title>Language Icon</title>
    <desc>Longer description</desc>
    <path d="M0 2C6.47 2 2 6.48 2 12s4.47 10 9.99 0h24v24H0z" />
</symbol>
```





### 4. Adicione subtítulos e legendas aos seus vídeos



Esta poderá ser uma das orientações mais difíceis de alcançar da WCAG, não devido a uma dificuldade técnica, mas porque pode ser bebem demorada. Mas existem algumas formas de implementar:

1. Vejamos o YouTube, por exemplo. Depois de carregar um vídeo na plataforma, você pode ativar legendas de forma automática. Elas são geradas automaticamente e podem ser imprecisas em algumas circunstâncias, dependendo do idioma, do ruído de fundo ou do sotaque do locutor. No entanto, estes são muito fáceis de implementar e podem funcionar bem na maioria dos vídeos que falam inglês.
2. Se estamos à procura de legendas 100% exatas, é difícil confiar no YouTube, por isso temos de escrever as legendas ou contratar um terceiro para o fazer. O YouTube suporta os formatos de legendas mais comuns (.srt, .sub e .sbv) e nos permite escrever as legendas na própria plataforma, o que pode ser muito conveniente se não tivermos nenhum software de legendas ou se quisermos pedir à nossa comunidade para nos ajudar a traduzir o conteúdo sem dar acesso à nossa conta.
3. Mas talvez você não queira usar o YouTube como sua plataforma de hospedagem. Talvez você queira usar um vídeo HTML5 hospedado no seu próprio servidor. O HTML5 inclui a <track> tag, que você pode usar para anexar facilmente quantos arquivos de legendas quiser usando o formato WebVTT (Translations FTW!).

```html
<video controls>
  <source src="movie.mp4" type="video/mp4">
  <track label="English Captions" kind="captions" srclang="eN" src="captions.vtt" default>
  <track label="Subtitulos en español" kind="captions" srclang="es" src="subs.vtt">
</video>
```





### 5. Semântica = acessibilidade

A semântica não nasceu com HTML5. Elas existem desde [a primeira página HTML](http://info.cern.ch/hypertext/WWW/TheProject.html) e melhoraram muito desde então. Com o padrão HTML5, novas tags semânticas são introduzidas para nosso uso diário.



![img](https://aerolab.cdn.prismic.io/aerolab/18ffcc9f9f0a3cb97dfef8817f11bc70956d5d74_05-web-accessibility-first-website.png)



Ok, mas a semântica não é só para SEO?

Não necessariamente. Quando você escolhe conscientemente uma tag <h1> sobre uma <p> ou uma <span>, você está mudando deliberadamente o significado de um elemento, fornecendo hierarquia e construindo uma estrutura em árvore das informações da sua página.

Os leitores de tela não se esquecem disso. Na verdade, a semântica é uma de suas armas mais úteis.

Tenha em mente que **com grande poder vem grande responsabilidade**, então certifique-se de usar a tag semântica apropriada para cada elemento, desde h1 até a nova tag <main>.





#### 6. Button vs. *<*a> tag



>  *Get the popcorn, this is a good one.* When should we use each?

Vejamos:

<a> tags são destinadas a vincular um arquivo a outro ou abrir links em uma nova aba ou na atual. No entanto, esta tag não é ideal quando desejamos acionar ações como menus de hambúrguer ou galerias de imagens. O elemento botão é a escolha certa para estas situações e é geralmente alcançável com JavaScript.

Além disso, a tag <button> pode ser facilmente confundida com a <input type="button">, mas a diferença reside no fato de a primeira ser capaz de ter mais conteúdo (texto, imagem + texto ou apenas imagens).

Há duas coisas a considerar ao usar a tag <button>:

Primeiro, se o conteúdo de um botão não for suficientemente explícito (por exemplo, um "X" num botão fechado), devemos adicionar um atributo aria-label para ajudar a explicar a função.

```html
<button aria-label="Close">X</button>
```

Em segundo lugar, se adicionar um atributo *href* faz sentido, então podemos também usar a tag <a> e substituir o comportamento do link com JavaScript.

> Uma galeria de imagens que usa uma tag <a> com *href* irá atrapalhar graciosamente seu site se o JavaScript não estiver habilitado.



### 7. Utilize `roles`  quando necessário



Para dizer aos leitores de tela que nosso link dispara uma ação e não é, de fato, uma tag comum <a>, precisamos adicionar a propriedade `role` com o valor "button".

Mas cuidado!

Ao escrever seu JavaScript, você precisa chamar suas funções não apenas com um clique, mas também quando o usuário pressiona a barra de espaço. **Isso é necessário porque o comportamento usado para botões é diferente daquele usado para links e o usuário deve ser capaz de acionar a ação em qualquer um desses comandos.**

```html
<a href="img/kitten.jpg" role="button" onclick="handleBtnClick(event)" onKeyPress="handleBtnKeyPress(event)">
  Button
</a>


function handleBtnClick(event) {    
  // Do something
}
 
function handleBtnKeyPress(event) {
  // Check to see if space or enter were pressed
  if (event.keyCode === 32 || event.keyCode === 13) {
    // Prevent the default action to stop scrolling when space is pressed
    event.preventDefault();
    
    // Do something
 
  }
}
```

> Read more about this on [MDN](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/ARIA_Techniques/Using_the_button_role).

Tenha em mente utilizar a propriedade `role` não é necessário a menos que você quebre as regras, como no exemplo acima. Elementos semânticos HTML têm uma função padrão já aplicada: "navigation" para a tag <nav>, "link" para a tag <a>, e assim por diante. Isso significa que um atributo de `role` só é necessário quando desejamos alterar esses valores padrões.



### 8. Sobre elementos escondidos

Existem alguns métodos disponíveis para esconder coisas com HTML e CSS. 

Se você quiser esconder elementos da vista mas ainda assim informar os leitores de tela sobre eles, então a primeira opção é a melhor.

Se você deseja remover a visibilidade do conteúdo como um todo, incluindo para os leitores de tela, você deve optar pela segunda opção.

```css
.visually-hidden { 
    position: absolute !important;
    clip: rect(1px 1px 1px 1px); /* IE6, IE7 */
    clip: rect(1px, 1px, 1px, 1px);
    padding:0 !important;
    border:0 !important;
    height: 1px !important;
    width: 1px !important;
    overflow: hidden;
}
body:hover .visually-hidden a, body:hover .visually-hidden input, body:hover .visually-hidden button { display: none !important; }
```





### 9. Siga os padrões de acessibilidade web

Desenvolver páginas com alto nível de acessibilidade é difícil e os padrões estão aqui para ajudar.

- [W3C standards](https://www.w3.org/TR/html51/)

- [WCAG guidelines](https://www.w3.org/TR/WCAG20/)

  

Você sabia que o Brasil também possui um guia de acessibilidade que deve ser utilizado pelos sites do governo?

http://emag.governoeletronico.gov.br/

Embora pareçam muitas regras, são no fundo ou uma reimplementação ou uma tradução em relação ao guia internacional.



### 10. Auditoria e revisão 



Depois de aplicar todo este conhecimento, está na hora de testar. Aqui está uma lista de ferramentas para auditar a acessibilidade de um site:

- [ChromeVox:](http://www.chromevox.com/) Disponível para Mac e Windows, esta extensão para o Chrome é um leitor de tela que pode utilizar para testar o seu website.
- [Accessibility Developer Tools for Chrome:](https://chrome.google.com/webstore/detail/accessibility-developer-t/fpkknkljclfencbdbgkenhalefipecmb?hl=en) Outra grande extensão para o navegador que adiciona uma opção de auditoria de acessibilidade em suas ferramentas de desenvolvimento.
- [Color Filter:](https://www.toptal.com/designers/colorfilter) Teste seu site para diferentes tipos de daltonismo.
- [ColorBlinding](https://chrome.google.com/webstore/detail/colorblinding/dgbgleaofjainknadoffbjkclicbbgaa): uma extensão para Chrome que permite ao usuário ver a página em diferentes tipos de daltonismo.
- [W3C Validator:](http://validator.w3.org/) Esta ferramenta oficial do W3C irá informá-lo se a suas tags HTML seguem as regras de acessibilidade web!
- [A11Y Compliance Platform:](http://www.boia.org/?wc3) O Bureau of Internet Accessibility (BOIA) oferece um relatório completo que fornece uma visão geral de como seu site se sai quando testado nos pontos de verificação WCAG A/AA..
- [WAVE:](http://wave.webaim.org/) Uma ferramenta de avaliação de acessibilidade web feita pela WebAIM.
- [AXE](https://chrome.google.com/webstore/detail/axe-web-accessibility-tes/lhdoppojpmngadmnindnejefpokejbdd): Uma versão um pouco melhor do WAVE





### Para sites externos:

https://www.powermapper.com/products/sortsite/checks/accessibility-checks/

https://www.toptal.com/designers/colorfilter

>https://aerolab.co/blog/web-accessibility



### Resumindo...

Aqui vão algumas dicas:

1. Faça descrição alternativa das imagens, pois assim os leitores de tela poderão identificá-las e descrevê-las para os usuários cegos.
2. Não use apenas cores para destacar uma informação. Isso ajudará pessoas com daltonismo, por exemplo, que não conseguiriam diferenciar os itens destacados apenas por cores.
3. Simplifique seu texto. Ao fazer isso, você facilita a leitura de pessoas com dislexia - Ou utilize a fonte [OpenDyslexic](https://opendyslexic.org/).
4. Crie áreas de clique maiores nos botões, isso ajudará usuários que não têm precisão nos cliques a acessar conteúdos específicos.

A [W3C](https://www.w3.org/) (World Wide Web Consortium) é uma organização mundial que desenvolve especificações técnicas e orientações para web. Ou seja, **ela quem cria e mantém os padrões para os sites na internet, incluindo os de acessibilidade**. O [WCAG](https://www.w3.org/Translations/WCAG20-pt-PT/) (*Web Content Accessibility* Guidelines) é o seu documento que traz as diretrizes de acessibilidade para a web, explicando como tornar o conteúdo acessível para pessoas com deficiências. De acordo com elas, um site acessível segue os seguintes princípios:

- **Perceptível**: As informações e interface são apresentadas de uma forma que possa ser percebida;
- **Operável**: A Interface e a navegação devem ser operáveis para todos os usuários;
- **Compreensível**: A informação deve ser apresentada de forma simples e compreensível;
- **Robusto**: O conteúdo deve ser robusto de uma forma que possa maximizar sua compatibilidade com diferentes tipos de pessoas e tecnologias assistivas.



http://emag.governoeletronico.gov.br/cursoconteudista/exercicios/lista-exercicios.html

https://www.w3.org/

http://romeo.elsevier.com/accessibility_checklist/

https://www.w3c.br/pub/Materiais/PublicacoesW3C/cartilha-w3cbr-acessibilidade-web-fasciculo-I.html



### Exercício

Utilizando as extensões acima, visite um site que costuma entrar em base diária e execute um diagnóstico para verificar se está ou não cumprindo as normas internacionais de acessibilidade.



E mais um...

Execute as extensões vistas acima na nossa página da API de tarefas - Verifique os  problemas encontrados, principalmente os que afetam diretamente a usabilidade de um usuário segundo as regras internacionais.
https://www.essentialaccessibility.com/blog/web-accessibility-issues/





> 
>
> Dicas sobre CSS para sites Acessíveis
>
> https://webdesign.tutsplus.com/articles/keyboard-accessibility-tips-using-html-and-css--cms-31966
>
> https://contrast-ratio.com/
>
> 
>
> Mais informações
> https://www.w3.org/WAI/teach-advocate/accessibility-training/workshop-outline/
>
> http://emag.governoeletronico.gov.br/cursoconteudista/exercicios/lista-exercicios-corrigidos.html
>
> 