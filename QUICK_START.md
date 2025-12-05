# 🚀 Полная инструкция: Laravel 12.0 + Filament 3.3 + Vite + Tailwind CSS 4.0

## 📋 Требования

- PHP 8.2+
- Composer
- Node.js 18+ и npm
- MySQL/MariaDB (или другая БД)

---

## Шаг 1: Создать Laravel проект

```bash
composer create-project laravel/laravel:^12.0 resto-mesto
cd resto-mesto
```

---

## Шаг 2: Установить Filament 3.3

```bash
composer require filament/filament:"^3.3"
php artisan filament:install --panels
```

---

## Шаг 3: Настроить Vite 7.0.4 и Tailwind CSS 4.0

### 3.1 Установить зависимости

```bash
npm install vite@^7.0.4 laravel-vite-plugin tailwindcss@^4.0.0 postcss autoprefixer axios
```

### 3.2 Создать конфигурацию Tailwind

```bash
npx tailwindcss init -p
```

### 3.3 Настроить `tailwind.config.js`

```js
import defaultTheme from 'tailwindcss/defaultTheme';

/** @type {import('tailwindcss').Config} */
export default {
    content: [
        './vendor/laravel/framework/src/Illuminate/Pagination/resources/views/*.blade.php',
        './vendor/filament/**/*.blade.php',
        './storage/framework/views/*.php',
        './resources/**/*.blade.php',
        './resources/**/*.js',
        './app/**/*.php',
    ],
    theme: {
        extend: {
            fontFamily: {
                sans: ['Figtree', ...defaultTheme.fontFamily.sans],
            },
        },
    },
    plugins: [],
};
```

### 3.4 Настроить `vite.config.js`

```js
import { defineConfig } from 'vite';
import laravel from 'laravel-vite-plugin';

export default defineConfig({
    plugins: [
        laravel({
            input: [
                'resources/css/app.css',
                'resources/js/app.js',
            ],
            refresh: true,
        }),
    ],
});
```

### 3.5 Настроить `postcss.config.js`

```js
export default {
    plugins: {
        tailwindcss: {},
        autoprefixer: {},
    },
};
```

### 3.6 Обновить `resources/css/app.css`

```css
@import 'tailwindcss';
```

### 3.7 Обновить `resources/js/app.js`

```js
import './bootstrap';
import axios from 'axios';

window.axios = axios;
window.axios.defaults.headers.common['X-Requested-With'] = 'XMLHttpRequest';
```

### 3.8 Обновить `resources/js/bootstrap.js`

```js
import axios from 'axios';
window.axios = axios;
```

---

## Шаг 4: Настроить базу данных

### 4.1 Создать `.env` файл (если его нет)

```bash
cp .env.example .env
php artisan key:generate
```

### 4.2 Настроить подключение к БД в `.env`

```env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=resto_mesto
DB_USERNAME=root
DB_PASSWORD=your_password
```

### 4.3 Запустить миграции

```bash
php artisan migrate
```

---

## Шаг 5: Создать пользователя администратора

```bash
php artisan make:filament-user
```

---

## Шаг 6: Запустить проект

### Терминал 1: Vite dev server (для фронтенда)

```bash
npm run dev
```

### Терминал 2: Laravel server

```bash
php artisan serve
```

---

## ✅ Готово!

Откройте в браузере: **http://localhost:8000/admin**

Войдите с учетными данными, созданными на шаге 5.

---

## 📝 Следующие шаги

1. Создать Filament ресурсы для моделей:
   ```bash
   php artisan make:filament-resource ModelName
   ```

2. Настроить права доступа и политики

3. Добавить бизнес-логику

---

## 🔧 Полезные команды

```bash
# Очистить кеш
php artisan cache:clear
php artisan config:clear

# Пересобрать фронтенд
npm run build

# Запустить миграции заново
php artisan migrate:fresh
```
