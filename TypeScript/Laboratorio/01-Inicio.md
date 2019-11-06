# TypeScript - Quick Start

O TypeScript tem suporte de *primeira classe* para o Node.js. Vamos começar configurando um projeto para isso:

> Nota: muitas dessas etapas são na verdade apenas práticas comuns de configuração do Node.js

1. Configure um projeto do Node.js `package.json `  criando uma pasta e executando `npm init -y`
2. Adicione o TypeScript (`npm install typescript --save-dev`)
3. Adicione o `node.d.ts` (`npm install @types/node --save-dev`) - A definição do TypeScript.
4. Inicie um `tsconfig.json` para opções do TypeScript com algumas opções-chave em seu tsconfig.json (`npx tsc --init --rootDir src --outDir dist --esModuleInterop --resolveJsonModule --lib es6,dom --module commonjs`)

É isso aí! Abra seu IDE (por exemplo, `code .`) e divirta-se. Agora você pode usar todos os módulos de Node embutidos (por exemplo, `import * as fs from 'fs';`) com toda a segurança e ergonomia do desenvolvedor do TypeScript!

E por último, criaremos uma pasta `src` para guardar todo o nosso código.

Ao final desta parte, deveremos ter uma pasta de projeto com a seguinte estrutura:



Todo o seu código TypeScript entra em `src` e o JavaScript gerado vai em `dist`.



### Utilizando o `nodemon`

- Instale o pacote `ts-node`,  que serve para possibilitar a compilação em tempo real do TS (`npm install ts-node --save-dev`)
- Instale o `nodemon` que irá executar um comando `ts-node` sempre que um arquivo for alterado (`npm install nodemon --save-dev`

Agora, basta apenas adicionar um novo comando na seção de scripts do `package.json` para que o seu aplicativo consiga executar a cada mudança de código:

```json
  "scripts": {
    "start": "npm run build:live",
    "build": "tsc -p .",
    "build:live": "nodemon --watch 'src/**/*.ts' --exec 'ts-node' src/index.ts"
  },
```

Então você pode agora executar o `npm start` e editar o`index.ts`:

- nodemon executa novamente seu comando (ts-node)
- ts-node transpila automaticamente pegando o tsconfig.json e a versão do TypeScript instalada,
- O ts-node executa a saída JavaScript através do Node.js.

E quando você estiver pronto para implantar seu aplicativo JavaScript, execute `npm run build`.





### Executando o projeto



Usar módulos escritos em TypeScript é super divertido, pois você obtém segurança em tempo de compilação e também funções de autocomplete (essencialmente, documentação executável).

Criar um módulo TypeScript é simples. Vamos assumir a seguinte estrutura de pastas para a aplicação:



```
Projeto
├─ package.json
├─ tsconfig.json
├─ src
│  ├─ Todos os seus arquivos de origem
│  ├─ index.ts
│  ├─ foo.ts
│  └─ ...
└─ dist
  ├─ Todos os seus arquivos de saída
  ├─ index.d.ts
  ├─ index.js
  ├─ foo.d.ts
  ├─ foo.js
  └─ ...
```

- Em seu arquivo `tsconfig.json`
  - contém `compilerOptions`: `"outDir": "dist"` e `"declaration": true` - Isso indica que os resultados da compilação do código devem ser salvos na pasta lib.
  - contém `include: ["./src/**/*"]` - Incluindo todos os arquivos da pasta `src`
- Em seu arquivo `package.json` contem
  - `"main": "dist/index"` - Informa ao Node.js para carregar `dist/index.js`
  - `"types": "dist/index"` < Informa ao TypeScript para carregar `dist/index.d.ts`





### Utilizando TypeDoc



E claro, como não podemos esquecer, agora vamos também documentar sempre o nosso código utilizando a biblioteca TypeDoc - uma versão do JSDoc para TypeScript!

Para instalar, basta executar o comando:

```bash
npm install --save-dev typedoc
```



E claro, para gerar a documentação, vamos adicionar na seção script do nosso `package.json`:

```json
"docs": "typedoc ./src --out docs"
```



### Criando o primeiro Arquivo

Para iniciar então nosso projeto, vamos começar criando uma pasta `src` e dentro dela um arquivo `index.ts` com o conteúdo: 

```typescript
/**
 * Solves the Hanoi Towers puzzle, for any number of disks.
 *
 * @param {number} disks - How many disks to move
 * @param {number} from - The starting pole's position
 * @param {number} to - The destination pole's position
 * @param {number} extra - The other pole's position
 */
const hanoi = (disks:number, from:number, to:number, extra:number) => {
    if (disks === 1) {
        console.log(`Move disk 1 from post ${from} to post ${to}`);
    } else {
        hanoi(disks - 1, from, extra, to);
        console.log(`Move disk ${disks} from post ${from} to post ${to}`);
        hanoi(disks - 1, extra, to, from);
    }
};

console.log('teste')
```

Dessa forma, ao executarmos o comando `npm run build:live`, vamos ver o resultado no console.

E se executarmos `npm run docs`, será criada uma pasta `Docs` contendo um HTML da documentação que pode ser servido ou acessado localmente.