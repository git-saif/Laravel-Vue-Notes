Laravel + Vue Project এ Route setup এর জন্য ৬টি স্টেপ ফলো করতে হবে। সেগুলো হলো:

1. Vue Router install করা। 
2. **`resources/js/router/index.js`** তে Route create করা।
3. View Page **(`Home.vue`, `About.vue`)** Create করা। 
4. Vue root component Create করা। 
5. app.js (root) Update করা।
6. Blade File Create করা।

## **1. Vue Router Install:**

**`Terminal:`**
```bash
npm install vue-router@4
```

---

## **2. router Folder Create:**

📁 **`resources/js/router/index.js`**
```js
import { createRouter, createWebHistory } from 'vue-router';
import Home from '../Pages/Home.vue';
import About from '../Pages/About.vue';

const routes = [
  { path: '/', name: 'home', component: Home },
  { path: '/about', name: 'about', component: About },
];

const router = createRouter({
  history: createWebHistory(),
  routes,
});

export default router;
```

---

## **3. Vue Page Create:**

📁 **`resources/js/Pages/Home.vue`**
```vue
<template>
  <div>
    <h2>🏠 Home Page</h2>
    <p>Welcome to Vue Router Demo inside Laravel!</p>
  </div>
</template>
```

📁 **`resources/js/Pages/About.vue`**
```vue
<template>
  <div>
    <h2>ℹ️ About Page</h2>
    <p>This is a sample About page using Vue Router 4.</p>
  </div>
</template>
```

---

## **4. `App.vue` Update:**

📁 **`resources/js/App.vue`**
```vue
<template>
  <div>
    <h1>🌐 Laravel + Vue Router 4 Example</h1>

    <nav>
      <RouterLink to="/">Home</RouterLink> |
      <RouterLink to="/about">About</RouterLink>
    </nav>

    <hr />

    <!-- এখানে পেজগুলো render হবে -->
    <RouterView />
  </div>
</template>

<script setup>
import { RouterLink, RouterView } from 'vue-router';
</script>

<style scoped>
nav {
  margin: 10px 0;
}
a {
  text-decoration: none;
  color: #007bff;
}
a.router-link-exact-active {
  font-weight: bold;
  color: #28a745;
}
</style>
```

> এটি Vue root component / Master Vue file.
---

## **5. `app.js` Update:**

📁 **`resources/js/app.js`**
```js
import { createApp } from 'vue';
import App from './App.vue';
import router from './router';

createApp(App)
  .use(router)
  .mount('#app');
```

---

## **6. Blade File Create:**
📁 **`resources/views/welcome.blade.php`**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Laravel + Vue Router</title>
    @vite('resources/js/app.js')
</head>
<body>
    <div id="app"></div>
</body>
</html>
```

---

## **7. Run Command:**

**`Terminal:`**
```bash
npm run dev
php artisan serve
```

তারপর ব্রাউজারে যেতে হবে:  
🔹 `http://127.0.0.1:8000/` → Home Page  
🔹 `http://127.0.0.1:8000/about` → About Page

> Page reload ছাড়াই Vue Router পেজ পরিবর্তন করবে!

---

## ✅ **Summery:**

| ধাপ | কাজ                                                 |
| --- | --------------------------------------------------- |
| 1️⃣ | `npm install vue-router@4`                          |
| 2️⃣ | `router/index.js` তৈরি                              |
| 3️⃣ | পেজ তৈরি (`Home.vue`, `About.vue`)                  |
| 4️⃣ | `App.vue` এ `<RouterLink>` ও `<RouterView>` ব্যবহার |
| 5️⃣ | `app.js` এ router use                               |
| 6️⃣ | Blade এ `@vite` যোগ                                 |
| 7️⃣ | `npm run dev` + `php artisan serve`                 |

---
