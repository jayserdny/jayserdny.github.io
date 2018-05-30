---
layout: post
title: Ionic 3 Meteor - Implementing Authentication Flow
categories: [Ionic, Meteor]
feature-img: "https://cdn.steemitimages.com/0x0/https://res.cloudinary.com/hpiynhbhq/image/upload/v1515397843/oosewfgmvaqajt0c1yu9.png"
---

![Screen Shot 2018-01-08 at 3.45.26 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1515401475/nd14xrpueqefhg6exzsx.png)

Following my last tutorial regarding [making a server method and calling it in ionic](https://jayserdny.github.io/ionic/meteor/ionic-3-meteor-creating-server-methods/), we will continue from this bootstrap and implement an auth system. The auth system will have only 2 functions: Login and Register.

---

The first steep is to clone the [ionic-meteor-starter](https://github.com/jayserdny/Ionic-meteor-starter) boilerplate by doing the following command:

> git clone https://github.com/jayserdny/Ionic-meteor-starter

Then, you will navigate as usual to the created folder by:

> cd ionic-meteor-starter

In order to make the boilerplate works, you need to run the following commands:

> npm install
> npm run meteor-client:bundle

Now, before starting code, let's install the dependencies needed for the auth system. To do so, navigate to the "src/api" folder and run the following commands:

> meteor add accounts-base
> meteor add npm-bcrypt
> meteor add accounts-password

![Screen Shot 2018-01-08 at 2.33.19 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1515396825/dpgnyuayojhif6hk5i9u.png)

Next, you should navigate back to your root folder and tell ionic about the changes made in the api:

> cd ../../
> npm run meteor-client:bundle

![Screen Shot 2018-01-08 at 2.36.01 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1515396981/vyxd4d6n5felwmzfjwmb.png)

At this point, we already have the needed dependencies installed and synced with the client side. Now, we will make a new provider to handle the auth process. So, we will name this provider "authProvider" and will be created with the following command:

> ionic g provider authProvider

![Screen Shot 2018-01-08 at 2.40.02 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1515397219/uvegaywlwm0brednoxd8.png)

Now we will start creating our logic for the auth. The first step is to navigate to the "auth.ts" file which is located at "src/providers/auth/auth.ts". In this file, we will remove the http related modules and you will end up with the following file:

![Screen Shot 2018-01-08 at 2.43.03 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1515397400/pypq5dq7qb8l85az2nlk.png)

Next, we will import the meteor-accounts and meteor module and will create two private methods which returns a promise type void. The two methods are login(..args) and register(..args):

![Screen Shot 2018-01-08 at 2.50.15 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1515397827/bpuxncximpeq8kk5wypi.png)

![Screen Shot 2018-01-08 at 2.50.32 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1515397843/oosewfgmvaqajt0c1yu9.png)

![Screen Shot 2018-01-08 at 2.50.46 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1515397858/mzcj6qlxavdxtpiee06l.png)

```javascript
import { Injectable } from '@angular/core';
import { Accounts } from 'meteor/accounts-base';
import { Meteor } from 'meteor/meteor';

@Injectable()
export class AuthProvider {

  constructor() {
    console.log('Hello AuthProvider Provider');
  }

  login(email: string, password: string): Promise<void> {
  	return new Promise<void>((resolve, reject) => {
                Meteor.loginWithPassword(email, password, (e: Error) => {
                      if (e) return reject(e);
                      resolve();
                });
        });
 }

  register(email: string, password: string, username: string, name: string): Promise<void> {
    let userData = {
      username: username,
      email: email,
      password: password,
      profile: {
        name: name
      }
    };
    return new Promise<void>((resolve, reject) => {
      Accounts.createUser(userData, (e: Error) => {
        if (e) return reject(e);
        resolve();
      });
    });
  }
}
```
You can get this code in the following Gist: [Auth.ts](https://gist.github.com/jayserdny/f13b5f8c15b75550706e76ac453d3431)

Basically, the login method will send the email or username of the use r and the password to server side for validation and will return the response. **Note: Password is sent encrypted before sending it to server side**

The register method will send an email, password, username, and name to the server and will return the response in a promise.

Now, we are going to create 3 pages: Login page, Register page, and Auth Protected Page and a provider for the alerts :

> ionic g page login
> ionic g page register
> ionic g page protected
> ionic g provider alerts

![Screen Shot 2018-01-08 at 2.57.10 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1515398243/nk1ut9xzdvchxuhgm37m.png)

The first thing that we will work on is the provider for the alerts. By having a provider for the alerts, we can avoid rewriting the same code over all the pages that needs alerts. So, navigate to the "alerts.ts" file located in "src/providers/alerts/alerts.ts" and first, we will remove any imports of the http module and import the following:

> import { AlertController, ToastController } from 'ionic-angular';

and replace the constructor with the following:

> constructor(private alertCtrl: AlertController, public toastCtrl: ToastController) {}

Then, create two public methods, showAlert() which receive an argument type any and presentToast() which also receive an argument type any:

![Screen Shot 2018-01-08 at 3.02.58 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1515398598/ysz8dpizccw8afey9b33.png)

```javascript
import { Injectable } from '@angular/core';
import { AlertController, ToastController } from 'ionic-angular';

@Injectable()
export class AlertsProvider {

  constructor(private alertCtrl: AlertController, public toastCtrl: ToastController) {}

  public showAlert(msg): void {
    const alert = this.alertCtrl.create({
      buttons: ['OK'],
      message: msg || msg.message,
      title: 'Oops!'
    });
    alert.present();
  }

  public presentToast(msg: any): void {

    let toast = this.toastCtrl.create({
      message: msg,
      duration: 4000
    });

    toast.present();
  }

}
```

Code for alerts.ts can be found in the following Gist: [Alerts.ts](https://gist.github.com/jayserdny/22e409d29523cdb09ca98275fb9312da)

Now, we will go to our "login.ts" which is located at "src/pages/login/login.ts" and we will implement our auth and alerts provider:

> import { AlertsProvider } from '../../providers/alerts/alerts';
> import { AuthProvider } from '../../providers/auth/auth';

And replace the constructor:

>  constructor(public navCtrl: NavController, public navParams: NavParams, private alertsUtil: AuthProvider, private authUtil: AuthProvider)

And we will delete the ionViewDidLoad() method.  Then, we will create the following variables above the constructor:

> private email: string;
> private password: string;

Then create a private void method called login which takes no arguments. This method will call the login method from the auth provider and will return the promise. We will catch whether or not the login was successfully. If the credentials are corrected, we will show a toast with the name of the user. Otherwise, an alert will be shown with the error. So, to be able to show the name of the user, we will need to import meteor in this page:

> import { Meteor } from 'meteor/meteor';

![Screen Shot 2018-01-08 at 3.34.14 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1515400467/naobeisr7uoe79g3tt3j.png)

```javascript
import { Component } from '@angular/core';
import { IonicPage, NavController, NavParams } from 'ionic-angular';
import { AlertsProvider } from '../../providers/alerts/alerts';
import { AuthProvider } from '../../providers/auth/auth';

@IonicPage()
@Component({
  selector: 'page-login',
  templateUrl: 'login.html',
})
export class LoginPage {

  private email: string;
  private password: string;

  constructor(public navCtrl: NavController, 
              public navParams: NavParams, 
              private alertsUtil: AlertsProvider, 
              private authUtil: AuthProvider) {}

  private login(): void {

    this.authUtil.login(this.email, this.password).then(() => {

      this.alertsUtil.presentToast(Meteor.user().profile.name);

    }, (error) => {

      this.alertsUtil.showAlert(error);
    })
 
  }

}
```

Code for login.ts can be found in the following Gist: [Login.ts](https://gist.github.com/jayserdny/f0275e5799495d8082c007e62b4e71f3)

And in the same folder, open the "login.html" file and we will create a simple UI with two inputs and a login button:

![Screen Shot 2018-01-08 at 3.44.36 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1515401111/ldf2jmdsdwglqw6kjkws.png)

```
<ion-header>

  <ion-navbar>
    <ion-title>login</ion-title>
  </ion-navbar>

</ion-header>


<ion-content padding>

  <ion-grid>
    <ion-row>
      <ion-col col-12>
        <ion-input type="text" 
                   placeholder="Username or email"
                   [(ngModel)]="email"></ion-input>
      </ion-col>

      <ion-col col-12>
        <ion-input type="password" 
                   placeholder="Password"
                   [(ngModel)]="password"></ion-input>
      </ion-col>

      <ion-col col-12>
        <button ion-button block round (tap)="login()">
          Login
        </button>
      </ion-col>
    </ion-row>
  </ion-grid>

</ion-content>
```
Code for login.html can be found in the following Gist: [Login.html](https://gist.github.com/jayserdny/af695587a2e17e3deab12eb42660e60f)

![Screen Shot 2018-01-08 at 3.45.26 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1515401141/z7l6tskd7dl024mdjbzb.png)

Now, we will create the register page. For this one, we will copy the content of the login.html file and we will paste it into the register.html file. This file is located at "src/pages/register/register.html" and we will add two more fields: one for the name and another one for the email. Also, we will change the Login text in the button and the title of the page. In addition, we will add the corresponding ngModels for the inputs and the register() method to the button:

![Screen Shot 2018-01-08 at 3.56.31 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1515401805/zxiy6nxjzo5pxgwnml5x.png)

```
<ion-header>
  
  <ion-navbar>
    <ion-title>Register</ion-title>
  </ion-navbar>
  
</ion-header>

<ion-content padding>
  <ion-grid>
    <ion-row>
      <ion-col col-12>
        <ion-input type="text" 
                   placeholder="Email"
                   [(ngModel)]="email"></ion-input>
      </ion-col>

      <ion-col col-12>
        <ion-input type="text" 
                   placeholder="Username"
                   [(ngModel)]="username"></ion-input>
      </ion-col>

      <ion-col col-12>
        <ion-input type="text" 
                   placeholder="Full Name"
                   [(ngModel)]="name"></ion-input>
      </ion-col>
  
      <ion-col col-12>
        <ion-input type="password" 
                   placeholder="Password"
                   [(ngModel)]="password"></ion-input>
      </ion-col>
  
      <ion-col col-12>
        <button ion-button block round (tap)="register()">
          Register
        </button>
      </ion-col>
      </ion-row>
    </ion-grid>
  
  </ion-content>
```

Code for register.html can be found in the following Gist: [Register.html](https://gist.github.com/jayserdny/7628256ffe24b46616586c3fed9c8952)

![Screen Shot 2018-01-08 at 3.58.28 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1515401932/acjh8uogdj1xuqsyvzzb.png)

Now, inside the register.ts file, we will import the auth and alerts provider:

> import { AlertsProvider } from '../../providers/alerts/alerts';
> import { AuthProvider } from '../../providers/auth/auth';

And we will have the following variables:

> private username: string;
> private email: string;
> private name: string;
> private password: string;

And the following constructor:

> constructor(public navCtrl: NavController, private alertsUtil: AlertsProvider, private authUtil: AuthProvider ) {}

And we will create the register method calling the register method in the auth provider. If there is not error registering the user, an alert will show saying "Success!!" and the screen will close to the main page, Otherwise, it will tell your the error. 

![Screen Shot 2018-01-08 at 4.06.18 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1515402395/olzy14mcuzksgwhogsso.png)

```javascript
import { Component } from '@angular/core';
import { IonicPage, NavController, NavParams } from 'ionic-angular';
import { AlertsProvider } from '../../providers/alerts/alerts';
import { AuthProvider } from '../../providers/auth/auth';

@IonicPage()
@Component({
  selector: 'page-register',
  templateUrl: 'register.html',
})
export class RegisterPage {

  private username: string;
  private email: string;
  private name: string;
  private password: string;

  constructor(public navCtrl: NavController, 
              private alertsUtil: AlertsProvider, 
              private authUtil: AuthProvider ) {}

  /**
   * 
   * Method to register
   * 
   */
  private register(): void {

    this.authUtil.register(this.email, this.password, this.username, this.name).then(() => {
      this.alertsUtil.showAlert("Success!!!!")
      
    }, (error) => {
      this.alertsUtil.showAlert(error);
    })

  }

}
```
The code for register.ts can be found in the following Gist: [Register.ts](https://gist.github.com/jayserdny/69d85dee5136c29ae55df91f4d6fa9e9)

## Opening these two pages from HomePage

Now, we will open these 3 pages (Remember the protected page?) from the home page. So, in the home page we will add three buttons: login, register, protected page.

![Screen Shot 2018-01-08 at 4.13.15 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1515402810/dbg8ka5mqrt56mp0brvf.png)

Then, in the home.ts we will add a method called openPage which takes a string as argument to open the pages:

![Screen Shot 2018-01-08 at 4.14.19 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1515402869/zhea664gtx0nanggiwt6.png)

And let's create a indicator to show the logged in user in the home page so we can know if we are logged in or not. First, import the following module to initialize a meteor tracker function and ngZone to track changes:

> import { Tracker } from 'meteor/tracker';
> import { NgZone } from '@angular/core';

add another reference to the constructor:

> private zone: NgZone

and create a variable to track meteor realtime changes:

> autorunComputation: Tracker.Computation;

![Screen Shot 2018-01-08 at 4.35.37 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1515404155/deexluf9fxvgw2rkqqvt.png)

```javascript
import { Component } from '@angular/core';
import { NgZone } from '@angular/core';
import { NavController } from 'ionic-angular';
import { Tracker } from 'meteor/tracker';

// Import this module
import { Meteor } from 'meteor/meteor';

@Component({
  selector: 'page-home',
  templateUrl: 'home.html'
})
export class HomePage {

  private currentName: string;
  private serverResults: string;
  private currentUser: string;
  autorunComputation: Tracker.Computation;

  constructor(public navCtrl: NavController, private zone: NgZone) {

    this._initAutorun();

  }

  _initAutorun(): void {
    this.autorunComputation = Tracker.autorun(() => {

      this.zone.run(() => {

       
        if (Meteor.user()) { 
          this.currentUser = Meteor.user().profile.name
          
        }
        else {
          this.currentUser = "Not Logged In!"
 
        }
        
      })
    });
  }

  private openPage(s: string) {
    this.navCtrl.push(s);
  }

  private openProtected(s: string) {
    if (Meteor.user()) {
      this.navCtrl.push(s)
    }
  }


  
  callServer(): void {
    // Call Meteor
    Meteor.call('cutenessLevel', this.currentName, (error, results) => {

      if (!error) {
        this.serverResults = results;
      }

      else {
        console.log(error);
      }

    });
  }

}
```
Code for Home.ts can be found in the following Gist: [Home.ts](https://gist.github.com/jayserdny/c8d37723ec81480049d38f2b1bc71dc1)

and bind the currentUser variable to the home page:

![Screen Shot 2018-01-08 at 4.19.06 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1515403156/rcb0bmnlzyeess4qk7xw.png)

```
<ion-header>
  <ion-navbar>
    <ion-title>
      Ionic Blank
    </ion-title>
  </ion-navbar>
</ion-header>

<ion-content padding>
  <ion-grid>
    <ion-row>
      <ion-col col-9>
        <ion-input placeholder="Type your name"
                   [(ngModel)]="currentName"></ion-input>
      </ion-col>
      <ion-col col-3>
        <button ion-button
                (tap)="callServer()">Submit</button>
      </ion-col>
    </ion-row>
    <ion-row>
      <ion-col col-12>
        {{ serverResults }}
      </ion-col>
    </ion-row>
    <ion-row>
      <ion-col col-4>
        <button ion-button (tap)="openPage('LoginPage')">
          Login
        </button>
      </ion-col>
      <ion-col col-4>
        <button ion-button (tap)="openPage('RegisterPage')">
          Register
        </button>
      </ion-col>
      <ion-col col-4>
        <button ion-button (tap)="openPage('ProtectedPage')">
          Protected
        </button>
      </ion-col>
    </ion-row>

    <ion-row>
      <ion-col col-12>
        current user: {{ currentUser }}
      </ion-col>
    </ion-row>

  </ion-grid>

</ion-content>
```
Code for Home.html can be found in the following Gist: [Home.html](https://gist.github.com/jayserdny/751912c51c1e2c4a90e32c62ee536515)

## Testing Login and Register

Now, we will test both functions to ensure its functionality.

![Screen Shot 2018-01-08 at 4.23.07 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1515403403/ra3tg7qssobqxhum0bii.png)

Login works fine! It tells you that there is not user found because we have not yet created an user. Now, let's try registering an user:

![Screen Shot 2018-01-08 at 4.27.17 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1515403655/tu76wlemdshbuzq2jv5p.png)

Works!!! The steemitSteem was successfully registered! Now, I will try logging in with this username:

![Screen Shot 2018-01-08 at 4.28.24 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1515403716/peilcoefbw7miyotsrnc.png)

Worked!! It shows the name of the account in  a toast!

Now, if we check the home page, it should tell us the name of the logged in user:

![Screen Shot 2018-01-08 at 4.37.17 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1515404251/m19rt8cyu6pqhbizu6ao.png)

Works!!! Now, lets create a condition for the protected page. We will create another function to push this page but with a condition to check if the user is logged in or not.

![Screen Shot 2018-01-08 at 4.38.54 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1515404342/l4cxonvjatjd6po8mqpg.png)
 
```javascript
import { Component } from '@angular/core';
import { NgZone } from '@angular/core';
import { NavController } from 'ionic-angular';
import { Tracker } from 'meteor/tracker';

// Import this module
import { Meteor } from 'meteor/meteor';

@Component({
  selector: 'page-home',
  templateUrl: 'home.html'
})
export class HomePage {

  private currentName: string;
  private serverResults: string;
  private currentUser: string;
  autorunComputation: Tracker.Computation;

  constructor(public navCtrl: NavController, private zone: NgZone) {

    this._initAutorun();

  }

  _initAutorun(): void {
    this.autorunComputation = Tracker.autorun(() => {

      this.zone.run(() => {

       
        if (Meteor.user()) { 
          this.currentUser = Meteor.user().profile.name
          
        }
        else {
          this.currentUser = "Not Logged In!"
 
        }
        
      })
    });
  }

  private openPage(s: string) {
    this.navCtrl.push(s);
  }

  private openProtected(s: string) {
    if (Meteor.user()) {
      this.navCtrl.push(s)
    }
  }


  
  callServer(): void {
    // Call Meteor
    Meteor.call('cutenessLevel', this.currentName, (error, results) => {

      if (!error) {
        this.serverResults = results;
      }

      else {
        console.log(error);
      }

    });
  }

}
```
Code for Home.ts can be found in the following Gist: [Home.ts](https://gist.github.com/jayserdny/c8d37723ec81480049d38f2b1bc71dc1)

And change the tap event of the protected page in the home.html:

![Screen Shot 2018-01-08 at 4.39.21 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1515404369/gcowg5wcu1zxtcwujrb8.png)

```
<ion-header>
  <ion-navbar>
    <ion-title>
      Ionic Blank
    </ion-title>
  </ion-navbar>
</ion-header>

<ion-content padding>
  <ion-grid>
    <ion-row>
      <ion-col col-9>
        <ion-input placeholder="Type your name"
                   [(ngModel)]="currentName"></ion-input>
      </ion-col>
      <ion-col col-3>
        <button ion-button
                (tap)="callServer()">Submit</button>
      </ion-col>
    </ion-row>
    <ion-row>
      <ion-col col-12>
        {{ serverResults }}
      </ion-col>
    </ion-row>
    <ion-row>
      <ion-col col-4>
        <button ion-button (tap)="openPage('LoginPage')">
          Login
        </button>
      </ion-col>
      <ion-col col-4>
        <button ion-button (tap)="openPage('RegisterPage')">
          Register
        </button>
      </ion-col>
      <ion-col col-4>
        <button ion-button (tap)="openProtected('ProtectedPage')">
          Protected
        </button>
      </ion-col>
    </ion-row>

    <ion-row>
      <ion-col col-12>
        current user: {{ currentUser }}
      </ion-col>
    </ion-row>

  </ion-grid>

</ion-content>
```

Code for Home.html can be found in the following Gist: [Home.html](https://gist.github.com/jayserdny/751912c51c1e2c4a90e32c62ee536515)

And since we are logged in, we are able to open this page:

![Screen Shot 2018-01-08 at 4.39.54 AM.png](https://res.cloudinary.com/hpiynhbhq/image/upload/v1515404411/e0x4aj9alwdyewq6cthl.png)

**Note: It is better to use auth guards. But for now, we will only use a simple condition to keep things simple**

And that is it! You have a simple auth implementation with Meteor in Ionic 3

***

If you have any question or concerns, just let me know! 💯
