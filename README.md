# LettuceEat

A short-form video platform for food discovery and ordering. Restaurants publish reels of their dishes. Users scroll, find something they want, and order — either through the restaurant's native flow or directly in-app via Razorpay.

---

## The idea

Most food apps make you search. LettuceEat flips that — you don't know what you want until you see it.

The feed is intentionally frictionless. No categories, no filters, no homepage clutter. Just a vertical reel of dishes from restaurants near you. When something catches your eye, one tap takes you to that restaurant's full profile. From there you can browse everything they've posted, place an order, and pay — all without leaving the app.

For restaurants, it's a zero-effort storefront. Upload a 30-second clip of your best dish and you're discoverable to everyone on the platform. No menu builder, no complicated setup. Your content is your listing.

---

## How ordering works

LettuceEat supports two ordering flows depending on what the restaurant has set up:

**Native checkout** — restaurants that onboard with Razorpay get a full in-app ordering experience. Users add items, pay via Razorpay (UPI, cards, netbanking, wallets), and the order lands directly on the partner dashboard. The restaurant confirms, and the user gets a status update in real time.

**Redirect flow** — restaurants that already have their own ordering system (website, aggregator) can link out directly from their profile. LettuceEat acts as the discovery layer; the transaction happens wherever the restaurant prefers.

This makes onboarding easier for existing restaurants while keeping the door open for a fully native experience as they grow with the platform.

---

## Stack

**Frontend** — React 19, Vite, React Router DOM, Axios, CSS custom properties with full light/dark mode

**Backend** — Node.js, Express, MongoDB + Mongoose

**Auth** — Separate JWT flows for users and food partners, HTTP-only cookies, protected routes on both ends

**Payments** — Razorpay (order creation on server, payment verification via signature, webhook support for order status updates)

**Media** — ImageKit for video upload, storage, and optimised delivery

---

## Running locally

Prerequisites: Node 18+, MongoDB, ImageKit account, Razorpay test account

```bash
git clone https://github.com/yourusername/lettuce-eat.git
cd lettuce-eat
```

**Backend**

```bash
cd backend
npm install
```

`backend/.env`:

```
JWT_SECRET=
MONGODB_URI=mongodb://localhost:27017/lettuce-eat
IMAGEKIT_PUBLIC_KEY=
IMAGEKIT_PRIVATE_KEY=
IMAGEKIT_URL_ENDPOINT=
RAZORPAY_KEY_ID=
RAZORPAY_KEY_SECRET=
RAZORPAY_WEBHOOK_SECRET=
```

```bash
node server.js
```

**Frontend**

```bash
cd frontend
npm install
npm run dev
# http://localhost:5173
```

---

## API

### Auth
```
POST   /api/auth/user/register
POST   /api/auth/user/login
POST   /api/auth/food-partner/register
POST   /api/auth/food-partner/login
```

### Feed
```
GET    /api/food                   public feed
POST   /api/food                   create post          food partner
POST   /api/food/like              like / unlike         user
POST   /api/food/save              save / unsave         user
GET    /api/food/save              saved videos          user
```

### Orders
```
POST   /api/orders                 place order           user
GET    /api/orders                 order history         user
GET    /api/orders/incoming        incoming orders       food partner
PATCH  /api/orders/:id/status      update status         food partner
```

### Payments
```
POST   /api/payments/create        create Razorpay order
POST   /api/payments/verify        verify payment signature
POST   /api/payments/webhook       Razorpay webhook handler
```

### Profiles
```
GET    /api/food-partner/:id       restaurant profile + posts
```

---

## Project structure

```
lettuce-eat/
├── backend/
│   ├── src/
│   │   ├── controllers/
│   │   ├── middlewares/
│   │   ├── models/
│   │   ├── routes/
│   │   └── services/
│   │       ├── storage.services.js
│   │       └── payment.services.js
│   └── server.js
└── frontend/
    └── src/
        ├── components/
        ├── pages/
        │   ├── auth/
        │   ├── food-partner/
        │   └── general/
        └── styles/
```

---

## Roadmap

- [ ] Real-time order status via WebSockets
- [ ] Location-based feed filtering
- [ ] Restaurant analytics dashboard
- [ ] Push notifications for order updates

---

## License

MIT
