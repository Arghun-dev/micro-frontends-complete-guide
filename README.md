# micro-frontends-complete-guide

## What are micro-frontends?

1. Divide a monolithic app into multiple, smaller apps.
2. Each smaller app is responsible for a distinct feature of a product.

## Why micro-frontends?

1. Multiple engineering teams can work in isolation.
2. Each smaller app is easier to understand and make changes to.


## Example scenario

Imagine we have `product list` and `cart list` components and we have decided to make each module as a single MFE of a micro-frontend application, like image below: But as you can see from image below, for haveing different MFEs in one monorepo we need to have one additional MFE called `container` or `shell` to combine and integrate different MFEs.
 
<img width="1720" alt="Screenshot 2024-08-17 at 14 11 08" src="https://github.com/user-attachments/assets/8306a027-b5a6-4bfb-a4de-aec71bd56a58">


This process of combining MFEs is called `integration`. 


## What is Integration?

How and when does the container get access to the source code in MFEs.

Integration types:

1. Build-Time integration
2. Run-Time integration
3. Server integration

<img width="1720" alt="Screenshot 2024-08-17 at 16 34 22" src="https://github.com/user-attachments/assets/d0eedc8a-116e-457a-81ae-5795bc827c60">

<img width="1720" alt="Screenshot 2024-08-17 at 16 37 00" src="https://github.com/user-attachments/assets/86dbf01b-8980-4401-a6d3-2478cd898442">


## Build time integration

<img width="1720" alt="Screenshot 2024-08-17 at 16 47 21" src="https://github.com/user-attachments/assets/b814a6ce-32d8-4e6c-aa1a-3e04fd776a4d">

<img width="1720" alt="Screenshot 2024-08-17 at 16 53 54" src="https://github.com/user-attachments/assets/f438d7b8-8433-45f5-b438-818873500bf9">


## Run-Time Integration

1. Engineering team develops ProductsList
2. Time to deploy!
3. ProducsList code deployed at https://my-app.com/productslist.js
4. User navigates to my-app.com, Container app is loaded
5. Container app fetches productslist.js and executes it

**Pros**
1. ProductsList can be deployed independently at any time
2. Different versions of ProductsList can be deployed and Container can decide which one to use

**Cons**
1. Tooling + setup is far more complicated


In this course we will focus on `Run-Time` integration using `Webpack Module Federation`



## Project Structure

Container -> src | public | package.json | webpack.config.js
Cart -> src | public | package.json | webpack.config.js
Products -> src | public | package.json | webpack.config.js


1. Products (MFE) will be build as a JS app with no framework
2. We have to be able to run it in isolation (on its own)
3. We have to be able to run it through the 'container' app


## Dependencies we use

1.  `webpack` this package combines all different js files and generates one single `bundle.js` or `main.js` file.
2.  `webpack-dev-server` => we now that have our main.js file created, but with just creating it doesn't really do us a whole lot. We now want to somehow take this file and execute it inside the browser, and make sure that eventually the code inside of here generate some HTML that gets displayed on the screen. So we need to load up main.js into the browser. `webpack-dev-server` makes output easily available to the browser.


webpack.config.js

```js
module.exports = {
  mode: "development",
  devServer: {
    port: 8081, // this will run the app in port 8081 - after you build the app you can get access the app in localhost:8081/main.js
  },
};
```

package.json

```js
{
  "name": "products",
  "version": "1.0.0",
  "main": "index.js",
  "scripts": {
    "start": "webpack serve" // this command will create main.js and serve it into the browser
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "description": "",
  "dependencies": {
    "faker": "5.1.0",
    "html-webpack-plugin": "5.5.0",
    "webpack": "5.88.0",
    "webpack-cli": "4.10.0",
    "webpack-dev-server": "4.7.4"
  }
}
```

with the above config you can get accesss main.js file in localhost:8081/main.js

But, we actually want to load an html inside the browser that's gonna have script tag that tries to load up main.js file.

like => <script src="main.js"></script> => we could definitelly add that manually.

But very quickly you will notice that the files coming out of webpack do not have predictable names and webpack can return you couple of js files with dynamic names like: `1k4j43.bundle.js`, `46j3ss.bundle.js` and ... that are absolutely unpredictable.

So, we're going to make use of a little plugin called `html-webpack-plugin` => this plugin is gonna take a look at different files that are coming out of webpack, it's gonna take a look at those file names and then automatically update that HTML document with adding those js files as script tags.

so we will update the webpack.config.js as below:

```js
const HtmlWebpackPlugin = require("html-webpack-plugin");

module.exports = {
  mode: "development",
  devServer: {
    port: 8081,
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: "./public/index.html",
    }),
  ],
};
```

and then run `npm start` => it will run `webpack serve` and then if you open up the app in the browswer and check the console, you will see the products has been logged into the console.

![WhatsApp Image 2024-08-24 at 15 23 48](https://github.com/user-attachments/assets/5cf47b78-5df6-4e77-8fec-aea0ac6bcba1)
![WhatsApp Image 2024-08-24 at 15 30 09](https://github.com/user-attachments/assets/dd1802a6-2020-4479-b4b1-9a99448c9055)
![WhatsApp Image 2024-08-24 at 15 38 13](https://github.com/user-attachments/assets/578011e9-f834-4958-b7bd-88b6bab3d895)

![WhatsApp Image 2024-08-24 at 17 14 26](https![WhatsApp Image 2024-08-24 at 17 52 45](https://github.com/user-attachments/assets/703bd3aa-9f18-44dd-aaa3-9fd781c8768c)
://github.com/user-attachments/assets/c07edf6a-8a1a-4ad3-a704-74a368ade0bb)
![Uploading WhatsApp Image 2024-08-24 at 17.52.45.jpeg…]()


Team 1 and 2 can develop their entire application in isolation, in other words team number 1 can develop products by itself and they don't have to care about what any other team is doing. They can have their own dependencies, their own tooling, only requirement is that they make use of Webpack and they make use of the Module Federation plugin, that's pretty much it. only container team should make sure all sub projects and MFEs are running so they have to run npm start for all MFEs, so they need to run npm start inside of cart, products and container apps.

![WhatsApp Image 2024-08-24 at 18 02 18](https://github.com/user-attachments/assets/e48ac3a1-6d72-4e60-af50-6dc4dbe82e1f)


## Sharing dependencies

as you see both cart and products MFEs are using faker.js module, in the current solution the container will load faker module twice since it's being installed in two MFEs. which is not good at all
![WhatsApp Image 2024-08-24 at 18 08 24](https://github.com/user-attachments/assets/474d1fb0-09f1-44b9-bc97-58915c012107)


To solve this problem we need to add remote MFEs not Host MFE the shared config to Module Federation.

cart webpack:

```js
const HtmlWebpackPlugin = require("html-webpack-plugin");
const ModuleFederationPlugin = require("webpack/lib/container/ModuleFederationPlugin");

module.exports = {
  mode: "development",
  devServer: {
    port: 8082,
  },
  plugins: [
    new ModuleFederationPlugin({
      name: "cart",
      filename: "remoteEntry.js",
      exposes: {
        "./CartIndex": "./src/index.js",
      },
      shared: ['faker'] // you need to add this line
    }),
    new HtmlWebpackPlugin({
      template: "./public/index.html",
    }),
  ],
};

```

products webpack:

```js
const HtmlWebpackPlugin = require("html-webpack-plugin");
const ModuleFederationPlugin = require("webpack/lib/container/ModuleFederationPlugin");

module.exports = {
  mode: "development",
  devServer: {
    port: 8081,
  },
  plugins: [
    new ModuleFederationPlugin({
      name: "products",
      filename: "remoteEntry.js",
      exposes: {
        "./ProductsIndex": "./src/index",
      },
      shared: ['faker'] // you need to add this line
    }),
    new HtmlWebpackPlugin({
      template: "./public/index.html",
    }),
  ],
};
```

with adding `shared` config to your webpack module federation you will solve loading duplicate packages.

but when we add shared package we hit an issue, if you run for example products MFE standalone, you will get this error:

`Uncaught Error: Shared module is not available for eager consumption`

That's not an issue if you run the app through container, but if you run the products app standalone you will get this error.

The reason: when you load up products in isolation, the first file that really gets executed is our src/index.js file and inside of it we have code that says
`import faker from 'faker';` => get access to faker right away, Like we instantaneously want Faker available inside this file because we're gonna use it.

Unfortunately when we mark faker as a shared module that causes it to be loaded up by default `asynchronously` So when we load up our index.js file we do not yet have faker available. And this is not an issue when we load up products through container, we are first loading up the remote entry file for products and a remote entry file has some code and configuration inside of it that says Hey we need to get access to index.js and to run that file we need Faker. So when we load up our application therough remote entry we don't run into any issue, because webpack can very easily have the time and say "Hey, we need to get both index.js and Faker modile" but when we load up products by itself we're getting index.js right away and end up with the error.

When we use `import` as a function form like below:

```js
import('./bootstrap');
```

it's gonna load bootstrap file async. So, we're gonna do same kind of thing inside of products app as well. By doing that we're going to give webpack the opportunity to take a look at what files this code requires.

so in we create bootstrap.js in products and we paste all code to bootstrap.js and inside index.js we will do this:

index.js

```js
import('./bootstrap');
```

And this will solve the issue.


## Shared Module Versioning

Remember different MFEs are going to be developed by different engineering teams and they could decide to use different versions of the shared dependencies.

so what will happen if they use different versions?

in that case whe we run the app again we will load two separate versions of faker dependency which basically we have the same issue of loading duplicate dependencies again. This is actually desireable, this is what we want to have happen.

So, Module Federation plugin is gonna take a look at the versions of these different modules that you specify inside of your package.json file.
