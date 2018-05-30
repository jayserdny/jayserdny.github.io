---
layout: post
title: Node-RED - Getting Started With Dashboards
categories: [Node-Red, Node.Js]
feature-img: "https://cdn.steemitimages.com/0x0/https://res.cloudinary.com/hpiynhbhq/image/upload/v1516265484/lve85yug4agjtq0hzuzd.png"
---

#### What Will I Learn?
***
In this tutorial, the following will be covered:

- Prerequisites to work with Node-Red
- Node-Red installation
- Integration of UI in Node-Read using dashboard

#### Requirements
***
Before getting started, a user should have installed the following module:

- NodeJs which can be downloaded at [https://nodejs.org/en/download/](https://nodejs.org/en/download/)

**Do not worry if you have not installed Nodejs in your environment, it will be covered in this tutorial.**

#### Difficulty
***
- Basic

#### Tutorial Contents
***
##### Installing Nodejs
***

To get started with this tutorial, you should have Nodejs installed in your environment. So, the first thing is to open the following website: [https://nodejs.org/en/download/](https://nodejs.org/en/download/)

**Windows Users**
Click on the Windows Installer option and the download will start automatically.

**macOS Users**
Click on the macOS Installer option and the download will start automatically.

![Screen Shot 2018-01-18 at 3.22.23 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1516263999/joghrnsyhjdtzdzzj2de.png)

Once the file is already downloaded, open the file and a windows like the following will appear:

![Screen Shot 2018-01-18 at 3.26.11 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1516264010/efd6uggr7l0jsbpxvscj.png)

In this windows, click on the continue button and the following step will appear:

![Screen Shot 2018-01-18 at 3.27.20 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1516264055/tlyx8bopbj0daoknw9le.png)

Take your time to **read** the license agreement and when you are ready, click on Continue and the following windows will appear:

![Screen Shot 2018-01-18 at 3.28.58 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1516264156/tjb7edve4jhavbokbkn7.png)

So, they are making sure you read the license agreement. So, click on Agree if you are ready. When you click in the Agree button, the next step will appear to choose where Nodejs will be installed. By default, it is recommended leave it as it and click on Continue:

![Screen Shot 2018-01-18 at 3.30.15 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1516264228/z51vnyx3cyhrnzbqghbp.png)

After clicking on Continue, the next step appears and you are ready to install. Click on the Install button and the installation will begin. **Note: If it ask for your password, introduce it**.

![Screen Shot 2018-01-18 at 3.32.19 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1516264352/pb2kkv2za9lgu5xhzp0w.png)

Once it is done, it will tell you that it is finished and you will click in the Close button:

![Screen Shot 2018-01-18 at 3.32.51 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1516264387/lvwgpgaaba0sysvutkzz.png)

And to make sure that NodeJs was installed correctly, run a command line and run the following command:

```sh
node -v
```

And the version of the currently version of NodeJs installed should appear:

![Screen Shot 2018-01-18 at 3.35.18 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1516264533/qjq3qcehs4ilz9i9nsth.png)


##### Installing Node-Red
***

Now, we are able to install Node-red. To do so, in a command line run the following command:

**For Windows users:**

```sh
npm install -g --unsafe-perm node-red
```

**For macOS users:**

```sh
sudo npm install -g --unsafe-perm node-red
```

![Screen Shot 2018-01-18 at 3.39.40 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1516264802/sodj2konrgppzfwzey1i.png)

Once this process is done, proceed to run a Node-Red instance by running the following command:

```sh
node-red
```

![Screen Shot 2018-01-18 at 3.41.36 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1516264913/gymzqu5ejp9o7qvs3bav.png)

It start running in http://127.0.0.1:1880/. Open this url and you will expect a windows like the following:

![Screen Shot 2018-01-18 at 3.43.24 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1516265021/fnfe1ssnoamhtvynf4u5.png)

##### Integrating dashboard UI
***

To install the dashboard UI, we need to install the dashboard dependencies. To do so, click in the hamburger menu in the right corner:

![Screen Shot 2018-01-18 at 3.45.19 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1516265132/lp7uxtlw661kjucnurqr.png)

Then click into the "Manage Palette" item.

![Screen Shot 2018-01-18 at 3.46.00 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1516265175/abbbi6caq5vpcfvsp07g.png)

This windows will appear, so click into the Install tab and write dashboard into the searcher:

![Screen Shot 2018-01-18 at 3.46.50 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1516265222/m9ewvqh4ofo0xkeyrlzo.png)

Find the one named: **node-red-dashboard** and click on install:

![Screen Shot 2018-01-18 at 3.47.04 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1516265250/vygcian3nqwvyt25cbj9.png)

Then, close this windows by clicking the red "Close" button in node-red.

Now, if you scroll down in the left side menu, you will see dashboard items:

![Screen Shot 2018-01-18 at 3.48.56 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1516265352/otrqvomdcmw60dzbsht7.png)

In this tutorial, we will create something simple, a Gauge controlled by an slider. So, from this list, drag and drop a Slider and a Gauge to the workflow:

![Screen Shot 2018-01-18 at 3.50.05 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1516265420/ngtwzkjt4ciyi9gcidjw.png)

Then connect both:

![Screen Shot 2018-01-18 at 3.50.59 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1516265484/lve85yug4agjtq0hzuzd.png)

Now, double click the slider item and click where it says: **Default [Unassigned]** then click in **Add new ui_group**:

![Screen Shot 2018-01-18 at 3.52.07 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1516265563/st7ljvkrugzczng6gmks.png)

And click into the edit icon and the following windows will appear:

![Screen Shot 2018-01-18 at 3.53.09 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1516265602/qbv4nidudcsrwio7njug.png)

And you should click on add since the default settings are fine for testing. Then the previous windows will appear and click in the "Done" button.

![Screen Shot 2018-01-18 at 3.54.16 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1516265668/qfmmdyuzd08nepe3mfam.png)

Then, double click into the Gauge item and click where it says: **Default [Unassigned]** then click in **Default [Tab 1]**:

![Screen Shot 2018-01-18 at 3.55.21 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1516265737/enf7nbx52hfw2f1jogbu.png)

Then click on the Done button.

Now, we are ready to deploy the changes to see the dashboard! To do so, click on the Deploy button in the header (it is in the left side of the hamburger menu icon):

![Screen Shot 2018-01-18 at 3.56.17 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1516265796/lubfvzueyi9uonjevlst.png)

To see the dashboard, open the following url **http://127.0.0.1:1880/ui** and you will get the same exact windows:

![Screen Shot 2018-01-18 at 3.57.18 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1516265852/jxknj21wmkh30sxzk2pj.png)

Move the Slider and you will see the changes reflected into the Gauge:

![Screen Shot 2018-01-18 at 3.57.55 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1516265890/lpyqxukoafgbvon26u0z.png)

And this is all! We have our first Node-Red dashboard ready!
