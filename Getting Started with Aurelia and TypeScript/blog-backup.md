## [draft !!]

Today, we will see how easy it is to get started developing on the Aurelia platform and how to leverage the power of TypeScript, a free and open source programming language developed and maintained by Microsoft.  We will focus on the main benefits and how to efficiently get going.

As a note before we get started, one of the truly great things about the Aurelia platform is its ability to support a wide variety of development scenarios.  Choosing the TypeScript language for Aurelia development is one of several great development language choices.

## TypeScript Benefits

In case you are wondering why you would want to look at using TypeScript, lets explore some of the features it provides.

### JavaScript IS TypeScript

It is super easy to get started with TypeScript because JavaScript IS TypeScript.  This means we can just rename our `.js` files to `.ts` files and we are off and running.

We will then immediately see TypeScript benefits before we've done anything to our code.

### The Integrated Development Environment (or IDE)

TypeScript benefits show up at design time, when we are actually writing and compiling our code.  We gain access to these benefits through `tooling` exposed by the IDE of your choice.  We have many excellent choices for our IDE.  Here is a short list of options:

1. <a href="https://atom.io/" target="_blank">Atom</a> IDE with the [Atom-TypeScript](https://github.com/TypeStrong/atom-typescript) extension
1. [WebStorm](https://www.jetbrains.com/webstorm/) IDE
1. [Sublime Text](http://www.sublimetext.com/) IDE with the [Sublime-TypeScript](https://github.com/Microsoft/TypeScript-Sublime-Plugin) extension
1. [Eclipse](https://www.eclipse.org/downloads/) IDE with the [Eclipse-TypeScript](https://github.com/palantir/eclipse-typescript) extension
1. [Visual Studio 2013](https://msdn.microsoft.com/en-us/library/dd831853.aspx) IDE
1. [Visual Studio 2015](https://www.visualstudio.com/en-us/downloads/visual-studio-2015-ctp-vs.aspx) IDE
1. [Visual Studio Code](https://code.visualstudio.com//) IDE

### Type Inference

The TypeScript language service (which is leveraged by the IDE), can use [type inference](https://github.com/Microsoft/TypeScript/wiki/Type-Inference) to make intelligent observations about our code and its expected types, before we ever add any type information ourselves.

Some of the benefits are possible because there is additional information that the IDE tooling has about the code.

Type inference might be relatively simple, like picking up the type of a constant that is assigned to a variable at initialization, and flowing that type through a function return.

It may also be quite sophisticated like picking up a function signature from assignment to a known callback type, defined in a type definition file.

### IntelliSense

When the IDE knows type information, it can offer statement completion, reducing typos and also making platforms like Aurelia MUCH easier to learn and use (we'll see examples of this when we start developing our application).  

The statement completion pop-up choices also includes any inline doc comments like descriptions, parameters and parameter descriptions and return types.

It makes it much easier for developers to explore, experiment with, and discover the features of an API.

### Intelligent Refactoring

As an application grows in size, renaming and other refactoring operations become necessary.  Without type information, error-prone search and replace options must be used, but with TypeScript, the language service knows where the renaming should occur and where it is a different identifier that happens to have the same name.

### Code Generation

TypeScript type annotations `evaporate` when the code is compiled.  The generated JavaScript is canonical JavaScript, well formatted, and what an expert would write to implement the corresponding patterns.

### Type Definition Files

One very important capability of TypeScript is the ability to wrap the API of an existing JavaScript library with defined types.  This is done by creating a file with a `.d.ts` extension that declares the types a developer can expect from the API.  Many, many JavaScript libraries have been wrapped by `.d.ts` files, created by the open source community and housed at [DefinitelyTyped](https://github.com/borisyankov/DefinitelyTyped).

Each Aurelia repo has a corresponding `.d.ts` file, as seen [here](https://github.com/cmichaelgraham/aurelia-typescript-atom/tree/900655787fc7775d87e79e061eb1539415b3b856/skel-nav-ts/typings/aurelia).

## Aurelia TypeScript Application

Great!  You made it this far, and hopefully by now you're convinced that TypeScript might be able to help you.

If you want to follow along, building and running as we go, you can read the [steps outlined here](https://github.com/cmichaelgraham/aurelia-typescript-atom).  If not, no worries; you can just look at the code and screen shots below to get a more concrete feel for the benefits obtained by using TypeScript to build Aurelia applications.

If you have trouble adapting the sample to your particular development environment, feel free to engage with the Aurelia community's vibrant [gitter channel](https://gitter.im/Aurelia/Discuss) or post questions on [Stack Overflow](http://stackoverflow.com/questions/tagged/aurelia), using the `aurelia` tag.

There is an excellent [getting started](https://player.vimeo.com/video/117778145) video that shows the basics of getting an Aurelia application up an running using ES6 development.

Lets dive in and look at some code.

### main.ts

The role of the `main.ts` file is to initialize Aurelia and then start up with the `app` view and view model.

The first thing to notice is that the [TypeScript version](https://github.com/cmichaelgraham/aurelia-typescript-atom/tree/900655787fc7775d87e79e061eb1539415b3b856/skel-nav-ts/views/main.ts) of this code and the [JavaScript version](http://aurelia.io/docs.html#startup-and-configuration) are identical.

```javascript
export function configure(aurelia) {
  aurelia.use
    .standardConfiguration()
    .developmentLogging();

  aurelia.start().then(a => a.setRoot());
}
```

The reason this works is because JavaScript IS TypeScript.

But now lets pretend that we didn't have the code to paste in from the Aurelia documentation.  This is where TypeScript begins to shine.

Notice that the configure function accepts an `Aurelia` object as its parameter.  At this point no type has been specified, so the IDE tooling can't help us figure out how to use the object we've been passed.

To obtain the `Aurelia` type, we need to import it from the `aurelia-framework` repo.  As we type the import statement, the IDE tooling suggests possible imports we might want:

![main 01](/content/images/2015/04/main-01.jpg)

Now we can fill in the actual type we want to pull in from the `aurelia-framework` repo.  Again the IDE tooling leads the way, showing us the repo's exports, and helping us pick the one we are seeking:

![main 02](/content/images/2015/04/main-02.jpg)

The fact that we're picking the name from a list and not typing it out manually reduces typos.  But lets say we're feeling pretty good about our typing, and ignore the IDE tooling's helpful suggestion.  If we do make a typo, a friendly, unobtrusive red squiggly underline is presented to us.

![main 02B](/content/images/2015/04/Main-02B.jpg)

So at this point we have successfully imported the `Aurelia` type, and can now use it to annotate the parameter type in the `configure` function as shown below.  Type annotation allows the IDE tooling to help us discover and chose valid members of the object.

![main 03](/content/images/2015/04/main-03.jpg)

At this point you may be wondering how the IDE actually knew about the types in the `aurelia-framework` import.  Let's talk about that next.

### TypeScript Type Definition (`.d.ts`) Files

Each Aurelia repo has a corresponding `.d.ts` file that declares the type information exposed by that repo's public API.  Our project's Aurelia `.d.ts` files can be found [here](https://github.com/cmichaelgraham/aurelia-typescript/tree/master/master/typings/aurelia).

You can inspect the [`aurelia-framework.d.ts` file](https://github.com/cmichaelgraham/aurelia-typescript-atom/tree/900655787fc7775d87e79e061eb1539415b3b856/typings/aurelia/aurelia-framework.d.ts#L37) if you'd like a glimpse at the syntax that defined the `Aurelia` type we used in our example above. 

But how does the IDE know which `.d.ts` files to include in our project?  The next section explains that very mechanism.

### The `tsconfig.json` Project File

The `tsconfig.json` file is a universal project format for TypeScript.  Our project's `tsconfig.json` file is located [here](https://github.com/cmichaelgraham/aurelia-typescript-atom/tree/900655787fc7775d87e79e061eb1539415b3b856/skel-nav-ts/tsconfig.json).

This is the tsconfig.json file in our project (with the files property stripped down to save space).

```javascript
{
    "version": "1.5.1",
    "compilerOptions": {
        "target": "es5",
        "module": "amd",
        "declaration": false,
        "noImplicitAny": false,
        "removeComments": false,
        "noLib": true,
        "emitDecoratorMetadata": true
    },
    "filesGlob": [
        "./**/*.ts",
        "!./node_modules/**/*.ts"
    ],
    "files": [
        // ...
    ]
}
```

The `files` property is filled in automatically by Atom-TypeScript using the `filesGlob` property's patterns.  This occurs when saving `tsconfig.json`.

For a description of the `compilerOptions`, please refer to the [TypeScript wiki Compiler-Options page](https://github.com/Microsoft/TypeScript/wiki/Compiler-Options).  `emitDecoratorMetadata` is an experimental compiler option that allows the @autoinject decorator to do its job (described in the next section).

Atom-TypeScript has an excellent description of the tsconfig.json details [here](https://github.com/TypeStrong/atom-typescript/blob/master/docs/tsconfig.md)

So the answer to the question about how Atom-TypeScript knows which files to include comes from (in our case) the expression: `./**/*.ts`, which says start in the root folder of the project and pull in any files ending in `.ts` (which includes files that end in `.d.ts`), so all of our `.d.ts` files in the typings folder will be included.

The final concept we'll explore before we wrap up (and provide you with a list related resources), is the topic of `decorators`

### Decorators

From the excellent [decorator description](https://github.com/wycats/javascript-decorators) by Mr. [Yehuda Katz](http://yehudakatz.com/): 

> Decorators make it possible to annotate and modify classes and properties at design time.
>
> While ES5 object literals support arbitrary expressions in the value position, ES6 classes only support literal functions as values. Decorators restore the ability to run code at design time, while maintaining a declarative syntax.
>
> A decorator is:

> * an expression
> * that evaluates to a function
> * that takes the target, name, and property descriptor as arguments and
> * optionally returns a property descriptor to install on the target object

Lets make that abstract concept more concrete by looking at some specific ways that Aurelia uses decorators.

First off, decorators are actually part of the ES7 (a.k.a ES2016) spec, but are available for your use today through the the TypeScript 1.5 Compiler (here's a TypeScript [roadmap](https://github.com/Microsoft/TypeScript/wiki/Roadmap#15)).

Now lets look at some code that shows how easy it is for you to use decorators.

#### dependency injection

We are going to look at a simple example of dependency injection. You may be very familiar with the concept of dependency injection, but if you're not, think about the flickr view and view model in our sample application.  The view model needs to retrieve a collection of images from the Flickr web services.

![flickr ui](/content/images/2015/05/flickr-ui.jpg)

We are going to "separate our concerns" into separate responsibilities - our flickr view model will worry only about how to display the results that come back from the web service, and the Aurelia `HttpClient` will be responsible for managing the HTTP retrieval of data from the web service.

We really don't want our flickr view model to even worry about how to obtain the Aurelia `HttpClient`.  We just want to specify in our constructor that we need it and we'll let the framework take care of the rest.

All we have to do to get the Aurelia framework to "inject" the Aurelia Http Client into our constructor is to first import and then place the `@autoinject` decorator on the class.

IntelliSense will help us discover and correctly type the text as before.

![autoinject intellisense](/content/images/2015/05/autoinject-decorator.jpg)

So you can see, we just ask for the `HttpClient` and then we use its public API.  In fact, our flickr view model will be happy with any object that provides the same (polymorphic) API - that is one of the very powerful aspects of dependency injection.

![autoinject 2](/content/images/2015/05/autoinject-decorator-2-1.jpg)

Now lets dive a little deeper.  Don't worry if you don't understand everything going on in the next section as we talk about the decorator called `bindable`.

#### bindable

Before we talk about the `bindable` decorator, we'll need a little bit of background.

First, a `custom element` is a reusable view / view model pair that can be embedded in another view.

In our sample project, we have a `custom element` called `nav-bar`:

![nav-bar](/content/images/2015/05/nav-bar.jpg)

The `nav-bar` custom element gives us a great way to "encapsulate" the layout and functionality of our navigation menu in a single unit that can be included with a clean, understandable syntax:
  
![nav-bar binding 2](/content/images/2015/05/nav-bar-binding-2.jpg)

Notice in the code above that our `custom element` (the `nav-bar`) has a router property that we are able to set (`bind`) to the router in our main application.  This works because the Aurelia framework supports bindable properties in `custom elements`.  But how does the Aurelia framework know which properties are to be exposed as such?  (hopefully you are thinking DECORATORS !!).

Within the `nav-bar` view model, the `@bindable` decorator is used to alert the Aurelia framework that we want the `router` property to be bindable.

![bindable decorator](/content/images/2015/05/bindable-decorator.jpg)

So in the outer code that embeds the `nav-bar`, we bound it to the router, and now router is accessible to the `nav-bar`'s view.  Notice how clean and expressive the `nav-bar`'s view template is, when referencing the router object's properties:

![nav-bar binding](/content/images/2015/05/nav-bar-binding-1.jpg)

I'm sure at this point you're ready to start building great applications in TypeScript and Aurelia.  Here are a few resources you can utilize as you start down the path.  Thanks for your interest and we look forward to seeing you online !!

### Resources

* [TypeScript Website](http://www.typescriptlang.org/)
* [TypeScript on GitHub](https://github.com/microsoft/TypeScript)
* [TypeScript Roadmap](https://github.com/Microsoft/TypeScript/wiki/Roadmap#15)
* [Aurelia TypeScript Samples](https://github.com/cmichaelgraham/aurelia-typescript#aurelia-typescript)
* [Aurelia Gitter Channel](https://gitter.im/Aurelia/Discuss)
* [Aurelia Website](http://aurelia.io/)
* [Aurelia Docs](http://aurelia.io/docs.html)
* [Aurelia Blog](http://blog.durandal.io/)