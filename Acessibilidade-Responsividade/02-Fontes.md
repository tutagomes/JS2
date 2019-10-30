#### Font-size responsivo

A propriedade `font-size` define o tamanho (altura) da fonte do nosso site.
Nós não conseguimos controlar o tamanho da fonte tão facilmente como controlamos outros o tamanho de outros elementos.

Exitem 2 tipos de fontes que podemos utilizar: fontes com valores fixos e valores relativos.

Valores absolutos:

* px (pixels): O valor definido em px vai ser sempre o mesmo independente da resolução/tamanho da tela.

Valores relativos:

* em: A unidade em é uma unidade relativa com base no valor computado do tamanho da fonte do elemento pai. Isso significa que os elementos filhos são sempre dependentes de seus pais para definir seu tamanho de fonte.
* rem: A unidade rem é relativa ao elemento root, ou seja, o valor da fonte definida no html ou body.

![font responsive vw](https://css-tricks.com/wp-content/uploads/2014/05/vw.gif)

vw/vh: Valores relativos ao tamanho do viewport da tela.
1vw = 1% of viewport width
1vh = 1% of viewport height



***

#### Imagens responsivas

![image responsive](http://i.imgur.com/NjGkGn6.gif)

Imagens responsivas respondem ao tamanho da tela para escalar proporcionalmente, sem ficar pixeladas ou desproporcionais.

Uma técnica para conseguirmos ter imagens responsivas é a seguinte:

```css
.img-responsive {
  width: 100%;
  max-width: 100%;
  height: auto;
  display: block;
}
```

Criamos uma classe que podemos aplicar a todas as imagens que estão no html que queremos que fiquem responsivas. As imagens que tiverem essa classe vão ter 100% de largura com altura sempre proporcinal a altura.
O atributo `max-width: 100%;` vai assegurar que essa imagem não estique mais do que o tamanho original dela permite.



***

#### Formatos populares de arquivo de imagens para internet

* JPG ou JPEG: É um dos formatos de imagem mais populares que existem. Ele permite salvar fotos com qualidade leves porque permite compressão do arquivo. Deve ser usado para imagens do site que não precisem ser aumentadas ou que tenham grande quantidade de detalhes ou definição.

* PNG: É um formato que permite imagens com transparência e que tem a melhor qualidade. Na hora de salvar o .png também faz compressão da imagem, porém sem perder tanta qualidade quanto o .jpg. A desvantagem é que ele é um dos formatos de arquivo mais pesado.

* GIF: O .gif é um dos formatos mais populares hoje em dia. Permite salvar animações, porém com qualidade de imagem bem ruim.

* SVG: O .svg é um vetor e não uma imagem. Ele é um arquivo com instruções xml que criam um desenho vetorizado na tela. O .sg não perde qualidade e pode ser ampliado quase infinitamente. É recomendado para ícones e logotipos.

Para ajudar a salvar imagens leves e com qualidade: https://tinypng.com/



---

- Exercício - Agora tente utilizar a fonte responsiva para automaticamente redimensionar os textos da página.

***

