---
layout: post
title: Setting up Ionic 3 with MeteorJs Backend
categories: [Ionic, Meteor]
---

Nowadays, Ionic is one of the easiest way to get done a powerful cross-platform apps. However, there are several users finding information on how to implement a backend into an Ionic app. Most of the users decides to use a serverless solution (such as Firebase) to their apps. Nevertheless, this solution is not efficient to all users (like me). 

This tutorial requires you be familiar with setting up the Ionic framework to get started. If you are not familiar, it is highly recommended that you follow this tutorial [Building Your First Multiplatform Apps Using IONIC](https://steemit.com/utopian-io/@iqbalhood/building-your-first-multiplatform-apps-using-ionic) and then you come back to this one. 

---- 

# Getting Started 

### Creating the Ionic project 
First thing to take in mind is to go the the directory where you want to have your projects. and use the following command to initialize a new project: 

> Ionic start ionicMeteor blank 

![Screen Shot 2017-12-30 at 7.01.16 PM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514678494/wu2a18jfyhr3t3jz7dxm.png) 

Now, navigate to the created folder by doing the following command: 

> cd ./ionicMeteor ![Screen Shot 2017-12-30 at 7.02.09 PM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514678593/etpzodjubnkocpwun4hq.png) 

Then, make sure your project is working fine by running it: 

> Ionic serve --l ![Screen Shot 2017-12-30 at 7.04.34 PM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514678700/wccsbyfrhwouj4f3jt6m.png) 

And you should get a webpage like the next one: 

![Screen Shot 2017-12-30 at 7.05.41 PM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514678775/vx6cjefwkdnc5ewhedn9.png) 

----

# Preparing project for Meteor Js 
Before getting started, we need make some configurations into our Ionic project. So, the first thing is to include Webpack into the project. To do so, open the package.json of your project and reflect it to the following: 

> "config": { "ionic_webpack": "./webpack.config.js" } 

![Screen Shot 2017-12-30 at 7.13.30 PM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514679234/m5u31cafmwa05izd5luo.png) 

However, it will throw an error since this file does not exist. But, Ionic provide us a default config file which is located under "node_modules/@ionic/app-scripts/config/webpack.config.js" and can be copied to our project root by using the following command: 

> cp node_modules/@ionic/app-scripts/config/webpack.config.js . 

![Screen Shot 2017-12-30 at 7.15.42 PM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514679356/fthnulzhjpjezr5kgakz.png)

Now, we will add the following abilities to the project: 

1. Load external TypeScript modules 
2. Have a direct alias to our backend 
3. Be able to use Meteor packages and Cordova plugins. 

Since this step is so large, feel free to copy the content of the following file into your "webpack.config.js" file: 

[webpack.config.js GIST](https://gist.github.com/jayserdny/86308c5d9a801716f541db56a757445b) 

Then, since we are using the module typescript-extends, you need to install it by running the following command: 

> npm install --save-dev typescript-extends 

![Screen Shot 2017-12-30 at 7.28.46 PM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514680138/yzdqzb7ga7hzzp6qishu.png) 

## Typescript configuration 

Now, we will tell TypeScript compiler to include the api folder (this folder will be created when we initialize the Meteor project) during the compilation process. To do so, open the tsconfig.json and reflect the following file: 

![Screen Shot 2017-12-30 at 8.41.54 PM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514684564/mvhdnloaopjn5mhi8cha.png) 

You can get this file in the following gist: [tsconfig.json](https://gist.github.com/jayserdny/1e3e392052d08bd78c191bcb7169482c) 

This configuration requires us to install declaration types for Meteor. This can be done by executing the following command: 

> npm install --save-dev @types/meteor 

![Screen Shot 2017-12-30 at 7.40.33 PM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514680848/kwbl2or18z9rrtvucwrw.png) 

At this point, we are ready to set up Meteor Js. But first, let's run the app once again to confirm that there is not any errors in the configuration: 

> Ionic serve --l 

And if everything is correct, you should get the following screen once again 

![Screen Shot 2017-12-30 at 7.42.40 PM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514680973/fngnezijh0bkwmcyg6ku.png) 

----

# Meteor Js Set up 
Now that our Ionic project is ready for the Meteor Js power, we can start by installing Meteor Js if it is not installed in your machine. To do so, you just need to run the following command: 

> curl https://install.meteor.com/ | sh 

Once it is done, navigate to "src" folder of the app into the command line: 

> cd src 

and create a new Meteor project inside this folder by using the following command: 

> meteor create api 

![Screen Shot 2017-12-30 at 7.49.39 PM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514681396/l11nmbo1cnawboqgsdxg.png) 

Now that our Meteor project is created, we need to remove the following from the api folder: 

1) node_modules folder 
2) client folder 
3) package.json and package-lock.json files 

Why remove these? Because we do not want to have duplicated files in our stack so, we will make a symbolic link to them. 

So, go to the api folder by doing: 

> cd api 

To remove node_modules folder run the following command: 

> rm -rf node_modules 

To remove client folder: 

> rm -rf client 

To remove package.json and package-lock.json files: 

> rm package.json package-lock.json 

And now, we will create a symbolic link to the files needed: 

> ln -s ../../package.json 
> ln -s ../../package-lock.json 
> ln -s ../../node_modules 

![Screen Shot 2017-12-30 at 7.55.56 PM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514681782/swlgtjqlqmxutoo7qhch.png) 

Since we are using Typescript and no Javascript, we need to make our Meteor project to be compatible with Typescript. Inside the "api" folder, run the following command to make Meteor compatible with Typescript: 

> meteor add barbatus:typescript 

![Screen Shot 2017-12-30 at 7.58.08 PM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514681903/mmcosygdynyyrql7y4tc.png) 

The next thing is to create a "tsconfig.json" into the "api" root folder: 

> touch tsconfig.json And reflect the following changes to this new file: 

![Screen Shot 2017-12-30 at 8.01.43 PM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514682132/blsicwxtntgtmhmlqiq6.png) 

If you do not want to follow this image, feel free to copy the file content from this GIST [tsconfig.json API](https://gist.github.com/jayserdny/935870bcc0bec58525d95aba18b214e9) 

Now, since Meteor generate .js files by default, let's change the extension of "server/main.js" to "server/main.ts": 

> mv server/main.js server/main.ts 

![Screen Shot 2017-12-30 at 8.04.16 PM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514682267/hfygbkezdftqvb4tdojp.png) 

The next thing is to go back to the root of the project folder: 

> cd ../../ 

and install the following dependencies so, our backend can run properly with our Ionic Angular front-end: 

> npm install --save babel-runtime 
> npm install --save meteor-node-stubs 

![Screen Shot 2017-12-30 at 8.06.41 PM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514682416/ju1yubx2qwnuydkl6zob.png) 

----
# Hooking up Ionic 3 With Meteor 
Now that our project is ready to connect to Meteor backend, we need to install [meteor-client-bundler](https://github.com/Urigo/meteor-client-bundler) in order to generate all Meteor client script into a single injectable module for our Ionic client. To do so, we need to install globally the following dependence: 

> sudo npm install -g meteor-client-bundler 

And in order to use this dependence, we need to also install the "tmp" module to generate temp files: 

> npm install --save-dev tmp ![Screen Shot 2017-12-30 at 8.11.50 PM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514682723/kekpqhfx0tywe0rkdbvl.png) 

The next thing is to include a custom script for generating our client bundle. To do so, we need to open the package.json into our project's root folder and add the following into the scripts section: 

> "meteor-client:bundle": "meteor-client bundle -s src/api" 

![Screen Shot 2017-12-30 at 8.14.13 PM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514682871/m7uvtvpl13gguevewlay.png) 

Now, you can run the following command to generate our bundle: 

> npm run meteor-client:bundle 

![Screen Shot 2017-12-30 at 8.19.18 PM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514683171/c8akjuth0ofdppkh7lho.png) 

By doing this, we are able to import our meteor client by using the following import: 

> import 'meteor-client'; 

So, open the following file "src/app/main.ts" and at the top of this file, include the following import: 

> import 'meteor-client'; 

![Screen Shot 2017-12-30 at 8.20.47 PM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514683257/veigrtksbmrszagvmdsi.png) 

By default, it will listen to Meteor on port 3000 (which is the default port using in Meteor) but you can specify your own port by using --url flag in the npm script that we included before. 

----
# Running the app 
To run the app, I prefer to have 4 command lines (YES FOUR!). Why that much? Because we will use two to run Ionic and Meteor, and two more to add packages to both :). So, Open 4 command lines and in two of them, go to the root folder of your app and in the other two, go to the api folder of the app. If you use my setup, you will end up with something like this: 

![Screen Shot 2017-12-30 at 8.25.36 PM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514683562/p1qfax1ugt5ieeek10hy.png) 

So, in the first command line, run the following command: 

> ionic serve --l 

and in the command line next to this one, run the api: 

> meteor run 

Once both are running, you can expect the following results: 

![Screen Shot 2017-12-30 at 8.47.03 PM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514684832/xohbaklj22gw3phvg7of.png) 

BUT WAIT! There is still the same! Yeah, actually the UI is still the same. However, when you open your browser console, you can confirm that Ionic is connecting to the port 3000 using sockets :D 

![Screen Shot 2017-12-30 at 8.48.09 PM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514684900/olhfxxyt3z2ptotm8vzk.png) 

So, yeah! Meteor is successfully connected to Ionic at this point! 

----
# Quick Starter 
For those who wants to start using the integration without following the tutorial, feel free to fork/clone my ionic-meteor-starter repo: [ionic-meteor-starter repository](https://github.com/jayserdny/Ionic-meteor-starter) 

---
If you have any question regarding this tutorial or if you are facing technical problems, do not hesitate to leave a comment in this post and I will certainly help you 👌🏼 
