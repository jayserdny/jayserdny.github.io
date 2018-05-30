---
layout: post
title: Ionic 3 Meteor - Creating Server Methods
categories: [Ionic, Meteor]
feature-img: "https://res.cloudinary.com/hpiynhbhq/image/upload/v1514860648/alk4q4w3wezv9pxtubiv.png"
---

Following my last tutorial on [Setting up Ionic 3 with MeteorJs Backend](https://jayserdny.github.io/ionic/meteor/setting-up-ionic-3-with-meteorjs-backend/), we will write our first server method which will be called into our Ionic 3 client. If you do not want to follow the last tutorial, feel free to use this quick-starter [Ionic Meteor Starter](https://github.com/jayserdny/Ionic-meteor-starter)

---
# Setting up Meteor Methods

Meteor allows us to create methods on the server that are able to be called in the client side. These methods can return anything you want. You can also pass parameters to them. Today, we will create a simple method that takes our name as a parameter and returns your cuteness level depending on the characters in your name.

The first thing we are going to do is to navigate to our "src/api" folder and create a new folder called methods. **Note: Meteor do not care about the folder structure of your project. However, I prefer to have everything organized to better readability of the project.** Also, I like to create different files for the methods depending on the features. For this tutorial, we are going to create a file named "steemit.ts"

![Screen Shot 2018-01-01 at 8.58.29 PM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514858325/ub2tlpj7mcpynoofkncd.png)

Inside this file, the first thing to do is to write our method.

![Screen Shot 2018-01-01 at 9.02.07 PM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514858550/osbohydzqlzigaw44nyc.png)

There, I just initialize a Meteor.methods instance which takes an object of functions as a parameter.  Then, inside this object, I created a new function called "cutenessLevel" which takes a string as a parameter and return a string.

As we stated before, the cuteness level will be defined by the count of characters in the given string. So, our criteria will be the following:

1) If characters count is less than 4, the method should return: "{name}, you are so ugly!"
2) If characters count is more or equal than 4  but less than 6, the method should return: "{name}, you are okay!"
3) If characters count is more or equal than 6, the method should return: "{name}, DAMN! You're my crush!!"

This can be done with simples if and else if statements.

![Screen Shot 2018-01-01 at 9.12.38 PM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514859171/rlrfdewgnoicmhj4watu.png)

And this simple, we have our first Meteor method created 👌🏼

---

# Calling Meteor methods in Ionic 3 client

In order to call our Meteor method in our Ionic 3 client, we will navigate to our home page "src/pages/home" and we will open our "home.html" file. What we are going to do now is that we are going to create a simple UI: An input and a button to send the string to server side and below, we will bind our variable holding the results from the server:

1) The input should have a "ngModel" which will be created later on in order to capture the current value of the input.
2) The button should be able to listen to a click and execute a function.

![Screen Shot 2018-01-01 at 9.22.46 PM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514859812/g81xqrtqmr9hlep2mx0z.png)

In my case, the input will write its content to a variable called "currentName", the Submit button will call a function called "callServer()", and the variable which will hold the results from the server is called "serverResults".

The next step is to go to our "home.ts" file to write our logic. In this file, we will write the variables and function mentioned before:

![Screen Shot 2018-01-01 at 9.26.02 PM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514859974/dferbgddetr0c4ipv59c.png)

We are almost done! The next step is to import Meteor and write our function to call the server method:

![Screen Shot 2018-01-01 at 9.28.14 PM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514860140/ipmguj4hemvbfabtqf4h.png)

In client side, we call Meteor methods with the following code:

> Meteor.call('name-of-the-method-in-server', *arguments, [callBack])

---

# Testing the results

Now, in the console with the root of the project, we need to run Ionic by using the command "ionic serve --l" and in the console for the api, we need to run Meteor by using the command "meteor run".

![Screen Shot 2018-01-01 at 9.34.44 PM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514860519/l7x4jh0mdnyjcgepnxqe.png)

And you should get a windows like the following:

![Screen Shot 2018-01-01 at 9.35.35 PM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514860546/blprppm4yld8hy4lif5s.png)

So, if I input my name, "jayser" which is 6 characters, it should tell me that I am its crush lol.

![Screen Shot 2018-01-01 at 9.37.03 PM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514860648/alk4q4w3wezv9pxtubiv.png)

But if I type in "jose" which is 4 characters, it will tell that jose is okay:

![Screen Shot 2018-01-01 at 9.38.20 PM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514860714/ifdkcnow0opgyudwky7m.png)

And finally, if I type "ana" which is 3 characters, well, poor ana hahaha, it will tell that she is ugly 😭

![Screen Shot 2018-01-01 at 9.39.20 PM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514860770/lkhntj4u3lua2qdeg6bp.png)

And we are done testing the project 💯

---

Did you see how easy is to create methods in the server and be able to call them back in the client side. If you have any question regarding this tutorial, do not hesitate to ask me in the comments and I will be glad to help you 💯
