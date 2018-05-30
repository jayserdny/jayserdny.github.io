---
layout: post
title: Ionic 3 - Importing third party JavaScript libraries using custom script
categories: [Ionic]
feature-img: "https://cdn.steemitimages.com/DQmRWeK27PNoFZDT7N2kG9Hp9VK8pN1Nur1eMPqKAfXpDeR/Screen%20Shot%202018-05-28%20at%2012.09.53%20AM.png"
---

#### Repository
https://github.com/ionic-team/ionic

#### What Will I Learn?

In this tutorial you will learn how to import JS and CSS files from 3rd party libraries using a custom script to always have the latest version.

#### Requirements
In order to follow this tutorial, you need to have installed the following:

- [Node.js](https://nodejs.org/en)
- [Ionic Framework](https://ionicframework.com/getting-started)
- An Ionic project already working.

#### Difficulty

- Intermediate

#### Tutorial Contents

##### Introduction
---
I bet you have found an amazing JavaScript library that you want to use in your Ionic Angular project. But when you try to install and import it, you notice that even it is installed, it appears like it is not installed and thus, can't find this library. It is a common mistake to think that any NPM library will work in any project, but sadly, this is not true.

Those libraries exposed a global variable that can be accessed in any part of the code.

A workaround to this problem is to manually download the distribution files of the library and import them into the index.html file of the project. However, you will need to repeat this process for each update of the library which is really tedious.

The goal of this tutorial is to take advantage of the execution of the script of the Ionic compilation process to automate this process.

**Please note that this tutorial will not cover how to initialize an Ionic project.**

##### Finding a NPM Library
---
To demonstrate the usability of this tutorial, I will select a JS library which does not have any type definitions yet.

In order to install the library, just the following command in the root folder of your Ionic project:

> npm install --save material-refresh


##### Trying to import the JS library into one of the pages
---

Now, we will import this library in one of our pages. The import will look like the following:

> import * as mRefresh from 'material-refresh';

If you notice, this will show an error saying that this module cannot be found (even though it was installed):

![Screen Shot 2018-05-27 at 11.29.15 PM.png](https://cdn.steemitimages.com/DQmRM6dmL6GjuG2RbUuNAmoMBzihnk2ZG5CETcscpVkzEx8/Screen%20Shot%202018-05-27%20at%2011.29.15%20PM.png)

In this case, we will redistribute the assets of this library to our project. So, feel free to delete this line to avoid compilations error.

##### Creating a custom script to grab assets from the library
---
In this step, we will create a custom script to grab all the assets of this library and redistribute them into our app's assets.

First, we will create a folder in the root folder of our project called config.

Inside this folder, we will create a file named copy.config.js and we will include the following content:

```
const existingConfig = require('../node_modules/@ionic/app-scripts/config/copy.config');

module.exports = Object.assign(existingConfig, {
    
});
```

In brief, this code is breakdown in the following:

```
const existingConfig = require('../node_modules/@ionic/app-scripts/config/copy.config');
```

This line will grab the default copy.config from our Ionic project and will save it into a constant variable.

```
module.exports = Object.assign(existingConfig, {
    
});
```

And this line will extend this constant variable with the new configurations to prevent overriding the whole configuration.

Now, we will copy the CSS file and the JS file from this library. In order to do so,  replace the module.exports block with the following:

```
module.exports = Object.assign(existingConfig, {
    copyMaterialRefreshJs: {
        src: './node_modules/material-refresh/material-refresh.min.js',
        dest: '{{BUILD}}'
    },
    copyMaterialRefreshCss: {
        src: './node_modules/material-refresh/material-refresh.min.css',
        dest: '{{BUILD}}'
    }
});
```

Basically, the first key "copyMaterialRefreshJs" will find the .js file of the library in its folder and will copy it to the build folder in the compilation process. The same thing for the copyMaterialRefreshCss key, it will find the specified file and will copy it to the build folder. 

By doing this process, it ensures that your assets will be par to the library updates. So, if you update the library, the app will grab the assets again in the compilation process.

##### Importing the library assets from the build folder
---
In this step, we will inject the copied assets into our index.html file. So, open the index.html file of the project and add the following lines:

In the Head section:

```
<link rel='stylesheet' href='build/material-refresh.min.css'/>
```

And in the script section after "<ion-app></ion-app>"


```
<script src='build/material-refresh.min.js'></script>
```

##### Telling Ionic to use our custom copy script
---
On the steps above, we created a custom copy.config file to copy the assets to our build folder. In this step, we will tell Ionic to use this script instead of the default one.

To do so, open the package.json file and add the following object inside the main object:

```
"config": {
    "ionic_copy": "./config/copy.config.js"
},
```

##### Using the library in the code
---

Now, in order to use this library, we need to declare a variable with the name exposed by the library. In order to find out this name, you need to check the repository of the library. In this case, https://github.com/lightningtgc/material-refresh

The exposed variable in this library is mRefresh.

So, to use this library, we need to do the following in our page file:

> declare var mRefresh: any;

declare the variable mRefresh as type any.

and if we do a console.log(mRefresh) inside the constructor of the page, you will notice that the library is being loaded correctly:

![Screen Shot 2018-05-28 at 12.09.53 AM.png](https://cdn.steemitimages.com/DQmRWeK27PNoFZDT7N2kG9Hp9VK8pN1Nur1eMPqKAfXpDeR/Screen%20Shot%202018-05-28%20at%2012.09.53%20AM.png)

##### Things to take in mind
---
Some libraries require other libraries to be installed. So, it means that you will need to do the same for the required libraries in order to make this work. For instance, in order to use the one used in the example, you need to include jQuery or Zepto.
