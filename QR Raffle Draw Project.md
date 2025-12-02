একদম শুরু থেকে শেষ পর্যন্ত একটা পরিষ্কার “চিটশিট” আকারে দিচ্ছি — **Laravel + Vue 3 + Vue Router 4 + Tailwind v4.1 + Vite** সেটআপ করার পুরো প্রসেস।  
(ধরা হচ্ছে তুমি already `composer create-project` করে Laravel প্রজেক্ট বানিয়ে ফেলেছ।)

---

## 0️⃣ বেসিক রিকয়ারমেন্ট

- PHP 8.x + Composer
    
- Node.js (latest LTS)
    
- Laravel project ready (ধরি প্রজেক্টের নাম `qr-raffle`)
    

```bash
cd qr-raffle
```

---

## 1️⃣ NPM প্যাকেজ ইন্সটল

### 1.1. Tailwind v4 + Vite plugin ইন্সটল

Official Tailwind v4 গাইড অনুযায়ী: ([Tailwind CSS](https://tailwindcss.com/docs/guides/laravel?utm_source=chatgpt.com "Install Tailwind CSS with Laravel"))

```bash
npm install -D tailwindcss @tailwindcss/vite
```

### 1.2. Vue 3 + Vue Router 4 + Vite plugin ইন্সটল

```bash
npm install vue vue-router
npm install -D @vitejs/plugin-vue
```

(Laravel প্রজেক্টে `laravel-vite-plugin` আগে থেকেই থাকে, তাই আলাদা করে ইন্সটল লাগবে না সাধারণত।)

---

## 2️⃣ Vite কনফিগারেশন (Laravel + Vue + Tailwind v4)

`vite.config.js` খুলে এইরকম করে দাও (multi SPA উদাহরণও ঢুকিয়ে দিলাম):

```js
import { defineConfig } from 'vite'
import laravel from 'laravel-vite-plugin'
import vue from '@vitejs/plugin-vue'
import tailwindcss from '@tailwindcss/vite'

export default defineConfig({
  plugins: [
    laravel({
      // এখানে তোমার সব CSS/JS entry ফাইল
      input: [
        'resources/css/app.css',
        'resources/js/user-app.js',
        'resources/js/admin-app.js',
        'resources/js/screen-app.js',
      ],
      refresh: true,
    }),
    vue(),
    tailwindcss(),   // Tailwind v4 Vite plugin
  ],
})
```

> চাইলে পরে `input` এন্ট্রিগুলো কম/বেশি করতে পারো, কিন্তু Tailwind থাকবে `tailwindcss()`।

---

## 3️⃣ Tailwind CSS এন্ট্রি ফাইল

`resources/css/app.css` তৈরি/এডিট করো:

```css
@import "tailwindcss";

/* Tailwind v4 এ content এর জায়গায় @source ব্যবহার হয়  :contentReference[oaicite:1]{index=1} */

/* Laravel pagination views */
@source "../../vendor/laravel/framework/src/Illuminate/Pagination/resources/views/*.blade.php";

/* Laravel cached views */
@source "../../storage/framework/views/*.php";

/* তোমার নিজের Blade + Vue + JS ফাইলগুলো */
@source "../**/*.blade.php";
@source "../**/*.{js,jsx,ts,tsx,vue}";
```

> এভাবে দিলে Tailwind তোমার Blade, Vue component, JS সবখানে থাকা ক্লাস detect করতে পারবে।

👉 এই সেটআপে **কোনো `tailwind.config.js` লাগবে না** — Tailwind v4 এর default কনফিগই কাজ করবে।

---

## 4️⃣ Vue SPA এন্ট্রি ফাইল তৈরি

ধরি তুমি তিনটা SPA ব্যবহার করছ: `user`, `admin`, `screen`.

### 4.1. `resources/js/user-app.js`

```js
import { createApp } from 'vue'
import UserApp from './Apps/UserApp.vue'
import userRouter from './router/user-router.js'

createApp(UserApp)
  .use(userRouter)
  .mount('#user-app')
```

### 4.2. `resources/js/admin-app.js`

```js
import { createApp } from 'vue'
import AdminsApp from './Apps/AdminsApp.vue'
import adminRouter from './router/admin-router.js'

createApp(AdminsApp)
  .use(adminRouter)
  .mount('#admin-app')
```

### 4.3. `resources/js/screen-app.js`

```js
import { createApp } from 'vue'
import ScreenApp from './Apps/ScreenApp.vue'
import screenRouter from './router/screen-router.js'

createApp(ScreenApp)
  .use(screenRouter)
  .mount('#screen-app')
```

---

## 5️⃣ Vue Router 4 সেটআপ

### 5.1. `resources/js/router/user-router.js`

```js
import { createRouter, createWebHistory } from 'vue-router'
import RegForm from '../Pages/User/Reg-Form.vue'
import WinnerScreen from '../Pages/User/Winner-Screen.vue'
import LoserScreen from '../Pages/User/Loser-Screen.vue'

const routes = [
  { path: '/', name: 'user.reg', component: RegForm },
  { path: '/winner', name: 'user.winner', component: WinnerScreen },
  { path: '/loser', name: 'user.loser', component: LoserScreen },
]

const router = createRouter({
  history: createWebHistory('/user'), // base URL: /user
  routes,
})

export default router
```

### 5.2. `resources/js/router/admin-router.js`

```js
import { createRouter, createWebHistory } from 'vue-router'
import Admin from '../Pages/Admin/Admin.vue'
import Participent from '../Pages/Admin/Participent.vue'
import CountDown from '../Pages/Admin/CountDown.vue'

const routes = [
  { path: '/', name: 'admin.home', component: Admin },
  { path: '/participants', name: 'admin.participants', component: Participent },
  { path: '/countdown', name: 'admin.countdown', component: CountDown },
]

const router = createRouter({
  history: createWebHistory('/admin'),
  routes,
})

export default router
```

### 5.3. `resources/js/router/screen-router.js`

```js
import { createRouter, createWebHistory } from 'vue-router'
import QrCode from '../Pages/Screen/QrCode.vue'
import CountDown from '../Pages/Screen/CountDown.vue'
import WinnerList from '../Pages/Screen/WinnerList.vue'

const routes = [
  { path: '/', name: 'screen.qr', component: QrCode },
  { path: '/countdown', name: 'screen.countdown', component: CountDown },
  { path: '/winners', name: 'screen.winners', component: WinnerList },
]

const router = createRouter({
  history: createWebHistory('/screen'),
  routes,
})

export default router
```

---

## 6️⃣ Root Vue Components (Tailwind ক্লাস সহ)

### 6.1. `resources/js/Apps/UserApp.vue`

```vue
<template>
  <div class="min-h-screen bg-slate-900 text-slate-50">
    <header class="border-b border-slate-800 px-4 py-3 flex items-center justify-between">
      <h1 class="text-lg font-semibold">User Panel</h1>
      <nav class="flex gap-3 text-sm">
        <RouterLink to="/" class="hover:underline">Registration</RouterLink>
        <RouterLink to="/winner" class="hover:underline">Winner</RouterLink>
        <RouterLink to="/loser" class="hover:underline">Loser</RouterLink>
      </nav>
    </header>

    <main class="p-4">
      <RouterView />
    </main>
  </div>
</template>

<script setup>
import { RouterLink, RouterView } from 'vue-router'
</script>
```

অ্যাডমিন/স্ক্রিনের জন্য একই স্টাইলে Tailwind class ব্যবহার করবে (তোমার আগের কোড already ঠিক আছে)।

---

## 7️⃣ Blade ফাইলগুলোতে Vite assets লোড

### 7.1. `resources/views/user.blade.php`

```blade
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>User App</title>
  @vite(['resources/css/app.css', 'resources/js/user-app.js'])
</head>
<body class="antialiased">
  <div id="user-app"></div>
</body>
</html>
```

### 7.2. `resources/views/admin.blade.php`

```blade
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Admin App</title>
  @vite(['resources/css/app.css', 'resources/js/admin-app.js'])
</head>
<body class="antialiased">
  <div id="admin-app"></div>
</body>
</html>
```

### 7.3. `resources/views/screen.blade.php`

```blade
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Screen SPA</title>
  @vite(['resources/css/app.css', 'resources/js/screen-app.js'])
</head>
<body class="antialiased">
  <div id="screen-app"></div>
</body>
</html>
```

> খেয়াল রাখবে: এখানে **কোথাও `mix()` ব্যবহার করবে না**, শুধু `@vite(...)`।

---

## 8️⃣ Laravel routes দিয়ে SPA serve করা

`routes/web.php`:

```php
use Illuminate\Support\Facades\Route;

// User SPA
Route::view('/user/{any?}', 'user')
    ->where('any', '.*');

// Admin SPA
Route::view('/admin/{any?}', 'admin')
    ->where('any', '.*');

// Screen SPA
Route::view('/screen/{any?}', 'screen')
    ->where('any', '.*');

// Optional: root redirect
Route::redirect('/', '/user');
```

এভাবে `/user`, `/admin`, `/screen` — তিনটা আলাদা Vue SPA Tailwind সহ চলবে।

---

## 9️⃣ Dev server ও build

### ডেভেলপমেন্ট

একটা টার্মিনালে:

```bash
php artisan serve
```

আরেকটা টার্মিনালে:

```bash
npm run dev
```

তারপর ব্রাউজারে:

- `http://127.0.0.1:8000/user`
    
- `http://127.0.0.1:8000/admin`
    
- `http://127.0.0.1:8000/screen`
    

### প্রোডাকশন build

```bash
npm run build
```

Laravel Vite নিজে থেকেই versioned assets বানিয়ে নেবে।

---

এটা follow করলে তুমি এক শটে পেয়ে যাচ্ছ:

- **Tailwind v4.1 (official Vite plugin সহ)**
    
- **Laravel + Vite + laravel-vite-plugin**
    
- **Vue 3 + Vue Router 4**
    
- **Multi-SPA structure (User/Admin/Screen)**
    

এখন চাইলে আমি শুধু Tailwind installation অংশটা আলাদা করে ছোট নোট আকারে বানিয়ে দিতে পারি, future প্রজেক্টে কপি-পেস্ট করার জন্য।