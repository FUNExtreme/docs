# Lock: Types of applications

You can use Auth0 Lock with both Single Page Apps and with Regular WebApps. In this section we'll learn how to use it with each of them.

## Single Page App

When you're using a Single Page App, you need that after a successful login through Auth0, you get the `id_token` ([JWT](/jwt)) with which you can then call your API. When you want to use [this workflow](/sequence-diagrams), the `responseType` is then a `token`.

Lock can work in [2 Authentication modes: Redirect and Popup](/libraries/lock/authentication-modes).

### Popup Mode

````js
var lock = new Auth0Lock('dsa7d77dsa7d7', 'mine.auth0.com');

lock.show(function(err, profile, id_token) {
  if (err) {
    console.log("There was ane error :/", err);
    return;
  }

  console.log("Hey dude", profile);
})
````
### Redirect mode

In this first example you'll see that a callbackURL isn't set. That's because by default the callbackURL is set to `location.href` which means current URL.

Optionally, [you can set the callbackURL to whatever you need](/libraries/lock/customization#callbackurl-string). Please bear in mind that if you do, you'll also need to specify `responseType: token` as part of the options.

````js
var lock = new Auth0Lock('dsa7d77dsa7d7', 'mine.auth0.com');

var hash = lock.parseHash();

if (hash) {
  if (hash.error) {
    console.log("There was an error logging in", hash.error);
  } else {
    lock.getProfile(hash.id_token, function(err, profile) {
      if (err) {
        console.log('Cannot get user :(', err);
        return;
      }

      console.log("Hey dude", profile);
    });
  }
}

lock.show();
````

To learn more about `parseHash` please [read this](https://github.com/auth0/auth0.js#redirect-mode).

## Regular Webapp

When you're doing a Regular web app, you need that after a successful login through Auth0, your app is redirected to a callback endpoint that you've created in your server. That callback endpoint will receive the `code` from Auth0 which must then [be exchanged for an `access_token` to get the user information](/protocols#4).

This means that in this case, only [redirect mode](/libraries/lock/authentication-modes#redirect-mode) makes sense.

### Redirect mode

```js
var lock = new Auth0Lock('dsa7d77dsa7d7', 'mine.auth0.com');

lock.show({
  callbackURL: 'http://myUrl.com/auth/callback'
});
```