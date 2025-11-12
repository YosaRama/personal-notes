# Web Push

#### 🧠 Problem

Web applications often need to **notify users about important events** — new messages, reminders, or system updates — even when:

* The user has **closed the browser tab**, or
* The app is **not actively running**.

Traditional methods like **WebSocket** or **HTTP polling** can’t deliver notifications when the browser session isn’t open, making it hard to maintain engagement and timely communication.

***

#### 🧭 Approach

The **Web Push API** solves this by enabling **server-initiated notifications** through the browser’s **Push Service**.

Here’s how it works conceptually:

1. The browser (client) **subscribes** to push notifications via a **Service Worker**.
2. The browser issues a **unique endpoint URL** managed by the browser vendor’s push service (e.g., FCM for Chrome, APNs for Safari).
3. Your backend stores that endpoint and can **send encrypted push messages** anytime — even when the web app is closed.
4. The browser’s **Service Worker wakes up**, receives the message, and displays a notification to the user.

This design allows reliable, secure, and asynchronous communication from the server to the user’s device.

***

#### 💡 Solution

Use Web Push when your application needs to **re-engage users** or **deliver background updates** — without requiring them to have your site open.

Common use cases:

* Transaction alerts or reminders
* Marketing campaigns or updates
* Messaging notifications for PWAs

**Example flow (simplified):**

```js
// 1. Client - Subscribe for push
navigator.serviceWorker.register("/sw.js").then(async (reg) => {
  const subscription = await reg.pushManager.subscribe({
    userVisibleOnly: true,
    applicationServerKey: "<Your VAPID Public Key>",
  });
  await fetch("/api/save-subscription", {
    method: "POST",
    body: JSON.stringify(subscription),
  });
});
```

```js
// 2. Server - Send push notification
import webpush from "web-push";

webpush.setVapidDetails(
  "mailto:you@example.com",
  process.env.VAPID_PUBLIC_KEY,
  process.env.VAPID_PRIVATE_KEY
);

await webpush.sendNotification(subscription, JSON.stringify({
  title: "Hello 👋",
  body: "You have a new message!",
}));
```

```js
// 3. Service Worker (sw.js)
self.addEventListener("push", (event) => {
  const data = event.data.json();
  self.registration.showNotification(data.title, {
    body: data.body,
    icon: "/icon.png",
  });
});
```

**Key benefits:**

* Works even when app is closed or offline
* Uses browser’s built-in push infrastructure
* Fully encrypted and secure (VAPID keys)

**Limitations:**

* Requires HTTPS and explicit user permission
* Limited payload size (\~4 KB)
* Browser-dependent delivery via vendor push services
