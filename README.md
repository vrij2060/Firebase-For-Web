# Firebase-For-Web

Be sure to paste the configuration code into your web page as described.
```js
<script src="https://www.gstatic.com/firebasejs/4.12.1/firebase.js"></script>
<script>
  // Initialize Firebase
  var config = {
    apiKey: "xxxxxxxxxxxxxxxxxxxxxxx",
    authDomain: "xxxxxxxxxx.firebaseapp.com",
    databaseURL: "https://xxxxxxxxxxxxxx.firebaseio.com",
    projectId: "xxxxxxxxxxxxxxxxxxxxxxx",
    storageBucket: "xxxxxxxxxxxxxxxxxxxxxxx.appspot.com",
    messagingSenderId: "xxxxxxxxxxxxxxxxxxxxxxx"
  };
  firebase.initializeApp(config);
  
  // your code
  //...
  
  </script>

```

## 1) Firebase Realtime Database 
### 1.1) Write Operation

```js

<script>
  var name="Name";
  
  // Overwite the Data
  firebase.database().ref('data').set({name:name});
  
  // Append the Data and create unique key
  firebase.database().ref('data').push({name:name});
  
</script>

```

### 1.2) Read Operation

```js

<script>
  var name="Name";

  firebase.database().ref('data').one('value',(snap)=>{
    console.log(snap.val());
  });

</script>

```
---

## 2) Firebase Authentication
Firebase Authentication provides backend services, easy-to-use SDKs, and ready-made UI libraries to authenticate users to your app. It supports authentication using passwords, phone numbers, popular federated identity providers like Google, Facebook and Twitter, and more.

Firebase Authentication integrates tightly with other Firebase services, and it leverages industry standards like OAuth 2.0 and OpenID Connect, so it can be easily integrated with your custom backend.


---
### 2.1) Auth with Email and Password
#### 2.1.1) Sign Up New User
Create a form that allows new users to register with your app using their email address and a password. When a user completes the form, validate the email address and password provided by the user, then pass them to the `createUserWithEmailAndPassword` method:
```js


<script>
 
  var email="someone@example.com";
  var password="password";
  
  //Create User with Email and Password
  firebase.auth().createUserWithEmailAndPassword(email, password).catch(function(error) {
    // Handle Errors here.
    var errorCode = error.code;
    var errorMessage = error.message;
    console.log(errorCode);
    console.log(errorMessage);
  });
  
</script>

```


#### 2.1.2) Sign In User
Create a form that allows existing users to sign in using their email address and password. When a user completes the form, call the `signInWithEmailAndPassword` method:


```js
<script>
  
  var email="someone@example.com";
  var password="password";
  
  //Sign In User with Email and Password
  firebase.auth().signInWithEmailAndPassword(email, password).catch(function(error) {
    // Handle Errors here.
    var errorCode = error.code;
    var errorMessage = error.message;
    console.log(errorCode);
    console.log(errorMessage);
});

  
</script>

```

#### 2.1.3) Set an authentication state observer and get user data

For each of your app's pages that need information about the signed-in user, attach an observer to the global authentication object. This observer gets called whenever the user's sign-in state changes.

Attach the observer using the `onAuthStateChanged` method. When a user successfully signs in, you can get information about the user in the observer.


```js

firebase.auth().onAuthStateChanged(function(user) {
  if (user) {
    // User is signed in.
    var displayName = user.displayName;
    var email = user.email;
    var emailVerified = user.emailVerified;
    var photoURL = user.photoURL;
    var isAnonymous = user.isAnonymous;
    var uid = user.uid;
    var providerData = user.providerData;
    // ...
  } else {
    // User is signed out.
    // ...
  }
});

```

#### 2.1.4) Sign Out User

To sign out a user, call `signOut`:

```js
firebase.auth().signOut().then(function() {
  // Sign-out successful.
  console.log('User Logged Out!');
}).catch(function(error) {
  // An error happened.
  console.log(error);
});
```

#### 2.1.5) Get Current Signed-In User Details

You can also get the currently signed-in user by using the `currentUser` property. If a user isn't signed in, `currentUser` is null:

```js
var user = firebase.auth().currentUser;

if (user) {
  // User is signed in.
  if (user != null) {
    name = user.displayName;
    email = user.email;
    photoUrl = user.photoURL;
    emailVerified = user.emailVerified;
    uid = user.uid;  // The user's ID, unique to the Firebase project. Do NOT use
                     // this value to authenticate with your backend server, if
                     // you have one. Use User.getToken() instead.
  }
} else {
  // No user is signed in.
}
```

#### 2.1.6) Update User Details

You can update a user's basic profile information—the user's display name and profile photo URL—with the `updateProfile` method. For example:



```js
var user = firebase.auth().currentUser;

user.updateProfile({
  displayName: "Updated User's Name",
  photoURL: "https://example.com/user/profile.jpg"
}).then(function() {
  // Update successful.
  console.log('User Profile Updated Successfully');
}).catch(function(error) {
  // An error happened.
});
```


#### 2.1.7) Set a user's email address

You can set a user's email address with the `updateEmail` method. For example:

```js
var user = firebase.auth().currentUser;

user.updateEmail("user@example.com").then(function() {
  // Update successful.
}).catch(function(error) {
  // An error happened.
});
```

#### 2.1.8) Send a user a verification email

You can send an address verification email to a user with the `sendEmailVerification` method. For example:

```js
var user = firebase.auth().currentUser;

user.sendEmailVerification().then(function() {
  // Email sent.
}).catch(function(error) {
  // An error happened.
});
```


#### 2.1.9) Set a user's password

You can set a user's password with the `updatePassword` method. For example:


```js
var user = firebase.auth().currentUser;
var newPassword = getASecureRandomPassword();

user.updatePassword(newPassword).then(function() {
  // Update successful.
}).catch(function(error) {
  // An error happened.
});
```

#### 2.1.10) Send a password reset email

You can send a password reset email to a user with the `sendPasswordResetEmail` method. For example:
```js
var auth = firebase.auth();
var emailAddress = "user@example.com";

auth.sendPasswordResetEmail(emailAddress).then(function() {
  // Email sent.
  console.log('Email Sent');
}).catch(function(error) {
  // An error happened.
});
```


#### 2.1.11) Delete User

You can delete a user account with the `delete` method. For example:

```js
var user = firebase.auth().currentUser;

user.delete().then(function() {
  // User deleted.
  console.log('User Deleted');
}).catch(function(error) {
  // An error happened.
});
```
You can also delete users from the Authentication section of the [Firebase console](https://console.firebase.google.com/?authuser=0), on the Users page.

---


### 2.2) Auth with Using Google Sign-In

If you are building a web app, the easiest way to authenticate your users with Firebase using their Google Accounts is to handle the sign-in flow with the Firebase JavaScript SDK. (If you want to authenticate a user in Node.js or other non-browser environment, you must handle the sign-in flow manually.)

To handle the sign-in flow with the Firebase JavaScript SDK, follow these steps:

1. Create an instance of the Google provider object:
    ```js
      var provider = new firebase.auth.GoogleAuthProvider();
    ```
2. Authenticate with Firebase using the Google provider object. You can prompt your users to sign in with their Google Accounts either by opening a pop-up window or by redirecting to the sign-in page. The redirect method is preferred on mobile devices.
   
   To sign in with a pop-up window, call `signInWithPopup`:
   
   ```js
   firebase.auth().signInWithPopup(provider).then(function(result) {
      // This gives you a Google Access Token. You can use it to access the Google API.
      var token = result.credential.accessToken;
      // The signed-in user info.
      var user = result.user;
      // ...
    }).catch(function(error) {
      // Handle Errors here.
      var errorCode = error.code;
      var errorMessage = error.message;
      // The email of the user's account used.
      var email = error.email;
      // The firebase.auth.AuthCredential type that was used.
      var credential = error.credential;
      // ...
    });
   ```
   
   To sign in by redirecting to the sign-in page, call `signInWithRedirect`:
   
   ```js
    firebase.auth().getRedirectResult().then(function(result) {
      if (result.credential) {
        // This gives you a Google Access Token. You can use it to access the Google API.
        var token = result.credential.accessToken;
        // ...
      }
      // The signed-in user info.
      var user = result.user;
    }).catch(function(error) {
      // Handle Errors here.
      var errorCode = error.code;
      var errorMessage = error.message;
      // The email of the user's account used.
      var email = error.email;
      // The firebase.auth.AuthCredential type that was used.
      var credential = error.credential;
      // ...
    });
   ```
   
   ##### 2.2.2) Sign Out User
   To sign out a user, call `signOut`:
   
   ```js
    firebase.auth().signOut().then(function() {
      // Sign-out successful.
    }).catch(function(error) {
      // An error happened.
    });
   ```

