# Chap4 Login Flow Overview

### Overview of Login flow

![Complete Login Flow](.gitbook/assets/2018-08-20-16.15.30.gif)



### Understand simple Token Authentication Flow



### Check SPA Login Flow in this App

1. Use [Google Login](https://developers.google.com/identity/sign-in/web/server-side-flow) 
2. Backend Server return AccessToken to Client
3. Store the AccessToken in Client

### Question you might think

1. Where to store AccessToken ?
2. How to change the client view if user logged in ?



### Where to store AccessToken in Client ?

In react Application, auth0 team shows to store AccessToken in **localStorage**

{% embed url="https://auth0.com/docs/quickstart/spa/react/01-login" %}

**But...** 

Page is Server Side Rendering in Nextjs App, So to store localStorage [is not working well](https://github.com/luisrudge/next.js-auth0/issues/10).

**So...**

Use **Cookie** to store AccessToken

{% embed url="https://github.com/zeit/next.js/issues/153" %}

{% embed url="https://github.com/luisrudge/next.js-auth0" %}



### How to change the client view if user logged in ?

We will learn this in the later chapter !!

