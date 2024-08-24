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

