---
layout: post
title: Fetching an API with Node-RED
categories: [Node-RED, NodeJs]
feature-img: "https://cdn.steemitimages.com/0x0/https://res.cloudinary.com/hpiynhbhq/image/upload/v1516346932/qx7cofxe02eucqrh8zdo.png"
---

#### What Will I Learn?
***
In this tutorial you will be learning how to fetch an API and display the data into a Node-Red Dashboard.

#### Requirements
***
To be able to follow this tutorial, you should be familiar with Node-Red and have it installed in your environment. If this is not the case, feel free to follow my previous tutorial:

[Node-Red - Getting Started with Dashboards](https://jayserdny.github.io/node-red/nodejs/node-red-getting-started-with-dashboards/)

My previous tutorial includes steps on how to install NodeJs and Node-Red as well.

#### Difficulty
***
- Basic

#### Tutorial Contents
***
###### Making the flow
***
In the last tutorial we left in this step were we had a Gauge controlled by a Slider:

![Screen Shot 2018-01-19 at 2.12.12 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1516345946/hxyxxa0xpsxv4o7xekvz.png)

But now, we will delete this to start doing a HTTP call to a rest API and parse the data to the dashboard. In this tutorial, we will fetch a fake API used for testing purposes [https://jsonplaceholder.typicode.com](https://jsonplaceholder.typicode.com) and we will use the following endpoint:

```bash
https://jsonplaceholder.typicode.com/posts
```

To get started, let's discuss about the flow. In Node-Red, we can make a HTTP get request by using two components:

```bash
Inject
HTTP Request
```

So, from the left menu, drag those 2 components in the flowchart and connect them:

![Screen Shot 2018-01-19 at 2.16.10 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1516346183/hxoym0v9gbijetupm7el.png)

Basically, the logic in this is easy. The injector tells the http request when to start, the http request fetch the api and return the response. So now, let's tell the http request component which API we will fetch. To do so, double click http request component and reflect the following change:

![Screen Shot 2018-01-19 at 2.17.57 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1516346295/wgbl312xyyxryxkzfevw.png)

What I did was change the url and insert the one we are using in this tutorial:

```bash
https://jsonplaceholder.typicode.com/posts
```

And replace the return type to a parsed JSON Object. Right now, there is not way to know that this works. However, let's insert a debug component which will be connected to the http request to listen the response:

![Screen Shot 2018-01-19 at 2.20.00 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1516346410/twyeeln600lfeclke21e.png)

Before testing, make sure to click the deploy button in the right top corner:

![Screen Shot 2018-01-19 at 2.20.31 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1516346440/b9qegt5hi9t3fynkihtp.png)

After deploying, you are able to test it. But wait! How do I test it? Easy, do you see the button in the left side of the inject component? Click there and then click in the Debug tab in the right side menu:

![Screen Shot 2018-01-19 at 2.21.28 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1516346497/tokaylkkc7n0cyigxypj.png)

![Screen Shot 2018-01-19 at 2.21.41 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1516346512/ae6osazrshctehzjdchi.png)

After clicking the injector button, you will get the following results:

![Screen Shot 2018-01-19 at 2.22.17 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1516346561/z2mazgrotgmxtcdp2gyy.png)

Since this is working, lets create a function to get the length of this array and send the data to a Gauge. The first thing is to drag and drop the function component in the left side menu and connect it to the http request right side:

![Screen Shot 2018-01-19 at 2.23.59 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1516346650/inaq3ynskmncq8lchgpj.png)

Now, let's write our function. The function will basically return an object with a key named payload (which you should always use if you want Node-Red to work correctly) and the value will be the length of the array returned from the http response. Always, the http response can be retrieved using:

```bash
msg.payload
```

![Screen Shot 2018-01-19 at 2.27.10 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1516346841/awfvm6g5av577bz5sdpi.png)

```json
return {
    payload: msg.payload.length
};
```
Click on done and add another debug component to the flow and connect it to the right side of the function:

![Screen Shot 2018-01-19 at 2.28.42 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1516346932/qx7cofxe02eucqrh8zdo.png)

Now, deploy and trigger the injector to see the response in the debugger console. You should expect the following response; The array itself and the length:

![Screen Shot 2018-01-19 at 2.29.31 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1516346985/v3tc0oin7kanujn1yozh.png)

Since it is returning the correct data, let's send this data to a Gauge. To do so, drag and drop a gauge component from the left side menu and connect it to the right side of the function:

![Screen Shot 2018-01-19 at 2.31.19 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1516347088/u19k1gaawfrabuxp4t9k.png)

Then, double click to add the Gauge to the UI that we created in the last tutorial:

![Screen Shot 2018-01-19 at 2.31.51 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1516347126/nsukbzssgqt1o2ya5kca.png)

Click on done and deploy. Then, trigger the injector again and open the following URL:  http://127.0.0.1:1880/ui and you should expect the Gauge to be in the maximum:

![Screen Shot 2018-01-19 at 2.33.17 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1516347207/iqbjd0jhrgkre7ismy95.png)

And that's it! That easy we can make http request to APIs in Node-Red without even need to "code".
