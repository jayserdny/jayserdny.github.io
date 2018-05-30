---
layout: post
title: Ionic 3 - Native like chat page
categories: [Ionic]
---

Are you tired of dealing with this in your Ionic app?

![25555054_343777162758469_3780792468660813824_n.gif](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514713538/sgef8uuo2sy3dhaxqv88.gif)

Well! Me too! By default, in a chat page the keyboard should stay open even when you click on the send button. But guess what? We will resolve this issue in this tutorial :)

For this tutorial, I have created a simple (and ugly) chat page (you can see it in the GIF). I believe it is not necessary to include how did I do it. It was simple, I just added an ion-footer component with a grid to divide the area of the input with the area of the send button. For the messages, I just created an array and a function to push the text wrote into the input. Easy right? Yep! 🙃

In order to resolve this problem, we need to create a new function into our chat page. This function will receive an event (if you are not familiar with events, I highly recommend you to read this [Events - Mozilla](https://developer.mozilla.org/en-US/docs/Web/Events))

The function will look like this:

> preventFocusChange(e) {
          e.preventDefault();
  }

![Screen Shot 2017-12-31 at 4.53.46 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514714043/ooq7srrzido9wzx2pl8z.png)

And in the send button, you should add the mousedown event and pass the created function with $event as parameter:

> (mousedown)="preventFocusChange($event)"

![Screen Shot 2017-12-31 at 4.56.12 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514714182/bxulkd0l6hiis1eh9pgj.png)

And we are done!! We now have a native like chat page. To confirm it, take a look at the GIF below 🙃

![25429192_343782026091316_4524749068538216448_n.gif](https://res.cloudinary.com/hpiynhbhq/image/upload/v1514714479/sgr5igommm5smpr5qxci.gif)

If you have any question, just let me know 💯
