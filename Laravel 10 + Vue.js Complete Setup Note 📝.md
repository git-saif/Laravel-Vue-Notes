## **1. Laravel Project Create**

**Step 1:** Laravel installer দিয়ে নতুন প্রজেক্ট তৈরি করতে হবে। 

```bash
laravel new my-vue-app
cd my-vue-app
```

**Step 2:** `.env` ফাইলে Database সেটিংস পরিবর্তন করতে হবে।

```env
DB_DATABASE=myapp
DB_USERNAME=root
DB_PASSWORD=
```

**Step 3:** Migration Run (Optional)

```bash
php artisan migrate
```

---

## **2. Vue.js Install**


**Step 1:** Vue + Vite plugin ইনস্টল করা।

```bash
npm install vue@latest @vitejs/plugin-vue
```

> Laravel + Vue.js এর জন্য Node.js লাগবে।
---

## **3. Vite Config করা**

`vite.config.js` এ Vue plugin যুক্ত করো:

```js
import { defineConfig } from 'vite';
import laravel from 'laravel-vite-plugin';
import vue from '@vitejs/plugin-vue';

export default defineConfig({
    plugins: [
        laravel({
            input: ['resources/js/app.js'],
            refresh: true,
        }),
        vue(),
    ],
});
```

---

## **4. Vue App তৈরি করা**

`resources/js/app.js` ফাইল:

```js
import { createApp } from 'vue';
import App from './App.vue';

createApp(App).mount('#app');
```

`resources/js/App.vue` ফাইল:

```vue
<template>
  <div>
    <h1>Hello Laravel 10 + Vue.js!</h1>
    <p>{{ message }}</p>
    <button @click="changeMessage">Click Me</button>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const message = ref('Welcome to Vue + Laravel!');
function changeMessage() {
  message.value = 'You clicked the button!';
}
</script>

<style scoped>
h1 { color: #4caf50; }
button {
  padding: 8px 15px;
  background-color: #1976d2;
  color: white;
  border: none;
  cursor: pointer;
}
</style>
```

---

## **5 Blade File Create**

`resources/views/welcome.blade.php`:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Laravel + Vue.js</title>
    @vite('resources/js/app.js')
</head>
<body>
    <div id="app"></div>
</body>
</html>
```

---

## **6. Run Development Server**

```bash
npm run dev
php artisan serve
```

👉 এখন ব্রাউজারে `http://127.0.0.1:8000` এ গেলে Vue Component render হবে।

---

## **7. Component & Page Structure (Recommended)**

```
resources/js/
 ├─ App.vue
 ├─ Pages/
 │   ├─ Home.vue
 │   └─ About.vue
 ├─ Components/
 │   ├─ Navbar.vue
 │   └─ Footer.vue
 └─ app.js
```

---

## **8. Laravel Controller থেকে Data পাঠানো**

`routes/web.php`:

```php
use Inertia\Inertia; // Optional, যদি Inertia use করো
use App\Models\User;

Route::get('/users', function() {
    $users = User::all();
    return view('welcome', compact('users')); // props without Inertia
});
```

Vue component-এ props bind করতে হলে Inertia.js ব্যবহার করা:

```bash
composer require laravel/breeze --dev
php artisan breeze:install vue
npm install
npm run dev
```

---

## **9. Vue.js এর Key Concepts**

- `ref()` → Reactive data
- `reactive()` → Object reactive data
- `computed()` → Computed properties
- `methods` / `setup()` → Functions
- `v-for`, `v-if`, `v-model` → DOM binding
- `props` & `emit` → Parent-child communication

---

## **🔟 Optional Tools**

- TailwindCSS / Bootstrap → Styling
- Vue Router → SPA navigation
- Pinia → State management
- Axios / Fetch → External API calls
- Inertia.js → Laravel ↔ Vue integration without full page reload

---

## ✅ **Summary Workflow**

1. Laravel Project তৈরি
2. Database configure + migrate
3. Vue + Vite plugin install
4. `vite.config.js` setup
5. Vue app (`App.vue` + `app.js`) তৈরি
6. Blade এ `div#app` mount
7. `npm run dev` + `php artisan serve`
8. Component, Routing, State Management add করা

---
