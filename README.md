# Server-side Google Tag Manager (sGTM) Docker Service

Окремий Docker-сервіс для розгортання Server-Side Google Tag Manager (sGTM) паралельно з e-commerce API (OctoberCMS), Frontend (Next.js) та Nginx Proxy Manager (NPM).

## 🚀 Швидкий старт

### 1. Налаштування середовища
Скопіюйте `.env.example` у `.env` та вкажіть ваш `CONTAINER_CONFIG`:

```bash
cp .env.example .env
```

Відкрийте `.env` та вкажіть ключ, скопійований з [Google Tag Manager](https://tagmanager.google.com) (Admin -> Container Settings -> Manually provision your tagging server):

```env
SGTM_CONTAINER_CONFIG=ваша_конфігурація_Base64
```

### 2. Створення Docker мережі (якщо ще не створена)
Переконайтеся, що мережа `proxy-net` існує в Docker (спільна мережа з Nginx Proxy Manager):

```bash
docker network create proxy-net || true
```

### 3. Запуск сервісу

```bash
docker compose up -d
```

---

## 🌐 Налаштування в Nginx Proxy Manager (NPM)

1. У NPM додайте новий **Proxy Host**:
   - **Domain Names**: `sgtm.example.com`
   - **Scheme**: `http`
   - **Forward Hostname**: `sgtm_server`
   - **Forward Port**: `8080`
   - ⚠️ **Websockets Support**: увімкнути (необхідно для Preview mode в GTM).
2. Замовте SSL-сертифікат Let's Encrypt та увімкніть **Force SSL**.

---

## ⚙️ Налаштування подій Meta CAPI та Google Analytics 4

1. Перейдіть на [Google Tag Manager](https://tagmanager.google.com) під вашим акаунтом.
2. Відкрийте створений Server контейнер.
3. У **Admin -> Container Settings** вкажіть **Server container URL**: `https://sgtm.example.com`.
4. У розділі **Tags** додайте тег **Facebook Conversions API** (з Community Template Gallery) та вкажіть ваші:
   - Meta Pixel ID
   - Meta CAPI Access Token
5. Натисніть **Publish**.
