Create a new folder `pwa`.

`index.html`
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Live Cricket Score</title>
    <link href="css/style.css" rel="stylesheet" />
  </head>
  <body>
    <header>
      Live Cricket
    </header>
    <div class="container">
      <div>Score:</div>
      <div id="score">0/0</div>
    </div>
  </body>
</html>

```

Initial  `css/style.css`:
```css
html,
body {
  margin: 0;
  padding: 0;
  font-family: Arial, Helvetica, sans-serif;
  font-size: 16px;
  background: #eee;
}

header {
  height: 60px;
  line-height: 60px;
  text-align: center;
  background: #4056d6;
  font-size: 24px;
  font-weight: bold;
  color: #fff;
}

.container {
  text-align: center;
  margin: 100px auto;
  font-size: 18px;
}

#score {
  font-size: 60px;
  font-weight: bold;
}


```

create `store.json`
```js
{
  "score": "243/2"
}
```

add jQuery reference
```html
<script
      src="https://code.jquery.com/jquery-3.3.1.min.js"
      integrity="sha256-FgpCb/KJQlLNfOu91ta32o/NMZxltwRo8QtmkMRdAu8="
      crossorigin="anonymous"
    ></script>
```

`js/app.js`
```js
$(document).ready(function() {
  setInterval(function() {
    $.ajax({
      url: "score.json",
      success: function(data) {
        $("#score").text(data.score);
      }
    });
  }, 5000);
});
```
Add reference to `js/app.js`


## App Shell
Add to `js/app.js`
```js
if ("serviceWorker" in navigator) {
  navigator.serviceWorker.register("./service-worker.js").then(function() {
    console.log("Service Worker is registered!");
  });
}
```

Create `service-worker.js`
```js
var cacheName = "cricket";
var filesToCache = ["index.html", "css/style.css"];

self.addEventListener("install", function(e) {
  console.log("Inside service worker install event");
  e.waitUntil(
    caches.open(cacheName).then(function(cache) {
      console.log("Service worker caching app shell");
      return cache.addAll(filesToCache);
    })
  );
});

self.addEventListener("fetch", function(e) {
  e.respondWith(
    caches.match(e.request).then(function(response) {
      return response || fetch(e.request);
    })
  );
});
```

## Push Notification
Update check in app.js to 
```js
if ("serviceWorker" in navigator && "PushManager" in window) {}
```
Inside app.js service worker register callback
```js
// checking for subscription
    sw.pushManager.getSubscription().then(function(subscription) {
      isSubscribed = !(subscription === null);

      if (isSubscribed) {
        console.log("User IS subscribed.");
      } else {
        console.log("User is NOT subscribed.");
        // Subscribe user
        const applicationServerPublicKey =
          "BL9-ndSdb8V56v_tGZzN5PD7b7YCvNNck-sy1peHC204qx78S_CMWQLhjA7p5L3dR_L8loyMvGVyVFFijlTUrxE";
        const applicationServerKey = urlB64ToUint8Array(
          applicationServerPublicKey
        );
        sw.pushManager
          .subscribe({
            userVisibleOnly: true,
            applicationServerKey: applicationServerKey
          })
          .then(function(subscription) {
            console.log("User is subscribed.");
          })
          .catch(function(err) {
            console.log("Failed to subscribe the user: ", err);
          });
      }
    });
```

```js
function urlB64ToUint8Array(base64String) {
  const padding = "=".repeat((4 - (base64String.length % 4)) % 4);
  const base64 = (base64String + padding)
    .replace(/\-/g, "+")
    .replace(/_/g, "/");

  const rawData = window.atob(base64);
  const outputArray = new Uint8Array(rawData.length);

  for (let i = 0; i < rawData.length; ++i) {
    outputArray[i] = rawData.charCodeAt(i);
  }
  return outputArray;
}
```

Inside service worker file
```js
self.addEventListener("push", function(event) {
  console.log("inside push");
  const title = "272/6 Virat Kohli 98*";
  const options = {
    body: "42 runs to win from 34 balls",
    icon: "images/icons/icon-72x72.png"
  };

  event.waitUntil(self.registration.showNotification(title, options));
});
```
