# JS Migration Guide



### **Suppressing Errors**

TypeScript will immediately start TypeChecking your code and your original JavaScript code *might not be as neat as you thought it was* and hence you get diagnostic errors. Many of these errors you can suppress with using `any` e.g.:



```
var foo = 123;
var bar = 'hey';

bar = foo; // ERROR: cannot assign a number to a string
```

Even though the **error is valid** (and in most cases the inferred information will be better than what the original authors of different portions of the code bases imagined), your focus will probably be writing new code in TypeScript while progressively updating the old code base. Here you can suppress this error with a type assertion as shown below:



```
var foo = 123;
var bar = 'hey';

bar = foo as any; // Okay!
```

In other places you might want to annotate something as `any` e.g.:



```
function foo() {
    return 1;
}
var bar = 'hey';
bar = foo(); // ERROR: cannot assign a number to a string
```

Suppressed:



```
function foo(): any { // Added `any`
    return 1;
}
var bar = 'hey';
bar = foo(); // Okay!
```

> Note: Suppressing errors is dangerous, but it allows you to take notice of errors in your *new* TypeScript code. You might want to leave `// TODO:` comments as you go along.**



### **Third Party JavaScript**

You can change your JavaScript to TypeScript, but you can't change the whole world to use TypeScript. This is where TypeScript's ambient definition support comes in. In the beginning we recommend you create a `vendor.d.ts` (the `.d.ts` extension specifies the fact that this is a *declaration file*) and start adding dirty stuff to it. Alternatively create a file specific for the library e.g. `jquery.d.ts` for jquery.

> Note: Well maintained and strongly typed definitions for nearly the top 90% JavaScript libraries out there exists in an OSS Repository called [DefinitelyTyped](https://github.com/borisyankov/DefinitelyTyped). We recommend looking there before creating your own definitions as we present here. Nevertheless this quick and dirty way is vital knowledge to decrease your initial friction with TypeScript**.

Consider the case of `jquery`, you can create a *trivial* definition for it quite easily:



```
declare var $: any;
```

Sometimes you might want to add an explicit annotation on something (e.g. `JQuery`) and you need something in *type declaration space*. You can do that quite easily using the `type` keyword:



```
declare type JQuery = any;
declare var $: JQuery;
```

This provides you an easier future update path.

Again, a high quality `jquery.d.ts` exists at [DefinitelyTyped](https://github.com/borisyankov/DefinitelyTyped). But you now know how to overcome any JavaScript -> TypeScript friction *quickly* when using third party JavaScript. We will look at ambient declarations in detail next.



### **Third Party NPM modules**

Similar to global variable declaration you can declare a global module quite easily. E.g. for `jquery` if you want to use it as a module (https://www.npmjs.com/package/jquery) you can write the following yourself:

```
declare module "jquery";
```

And then you can import it in your file as needed:

```
import * as $ from "jquery";
```

> Again, a high quality `jquery.d.ts` exists at [DefinitelyTyped](https://github.com/borisyankov/DefinitelyTyped) that provides a much higher quality jquery module declaration. But it might not exist for your library, so now you have a quick low friction way of continuing the migration 🌹



### **External non js resources**

You can even allow import of any file e.g. `.css` files (if you are using something like webpack style loaders or css modules) with a simple `*` style declaration (ideally in a [`globals.d.ts` file](https://jorgedacostaza.gitbook.io/typescript-pt/project/modules/globals)):

```
declare module "*.css";
```

Now people can `import * as foo from "./some/file.css";`

Similarly if you are using html templates (e.g. angular) you can:

```
declare module "*.html";
```