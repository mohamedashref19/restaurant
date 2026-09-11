# 🍽️ Restaurant Management & Ordering Platform

A full-stack restaurant web application built with Node.js, Express.js, MongoDB, and Pug.

The platform provides user authentication, menu management, online ordering, table/meal booking, reviews, Stripe Checkout integration, email notifications, and role-based access control.

---

## ✨ Features

### 🔐 Authentication & Authorization

- User registration
- Email OTP verification
- Login / Logout
- JWT authentication
- HTTP-only authentication cookies
- Password hashing with bcrypt
- Password reset via email
- Password change
- Role-based authorization

Supported roles:

- `user`
- `admin`
- `delivery`
- `kitchen`

---

### 🍔 Meals Management

The application provides a complete meal management workflow.

Features include:

- Create meals
- View all meals
- View a single meal
- Update meals
- Delete meals
- Meal filtering
- Sorting
- Field limiting
- Pagination
- Discount pricing
- Meal image uploads
- Multiple meal images
- Image validation
- Image resizing and optimization with Sharp

Meal images are processed before being stored in the public assets directory.

---

### 🛒 Online Ordering

Authenticated users can create and manage orders.

The order system:

- Accepts multiple meal items
- Validates that meals exist
- Calculates the total price on the server
- Supports meal quantities
- Stores the price at the time of ordering
- Associates orders with users
- Provides order history
- Supports order status management

Order statuses include:

```text
pending
cooking
delivering
delivered
cancelled
```

---

### 💳 Stripe Payments

The application integrates Stripe Checkout for online payments.

The payment workflow includes:

```text
Customer
   │
   ▼
Create Checkout Session
   │
   ▼
Stripe Checkout
   │
   ▼
Payment Completed
   │
   ▼
Stripe Webhook
   │
   ▼
Verify Webhook Signature
   │
   ▼
Create Booking Records
   │
   ▼
Send Confirmation Email
```

Stripe webhook signatures are verified using the configured webhook secret before processing successful checkout events.

---

### 📅 Booking System

The application includes a booking workflow connected to meals and authenticated users.

After a successful Stripe Checkout session, the webhook handler retrieves the purchased line items and creates booking records.

Users can also:

- View bookings
- Retrieve individual bookings
- Update bookings
- Delete bookings

Booking confirmation emails are sent after successful checkout processing.

---

### ⭐ Reviews

Users can interact with meal reviews.

Supported operations include:

- Get reviews
- Get reviews for a specific meal
- Create a review
- Update a review
- Delete a review

Reviews are associated with both users and meals.

---

### 📧 Email Notifications

The application integrates email services for important user actions.

Email functionality includes:

- OTP verification emails
- Welcome emails
- Password reset emails
- Booking confirmation emails

The project uses Nodemailer and SendGrid-related packages for email delivery.

---

## 🏗️ Architecture

The backend follows a traditional layered Express.js architecture.

```text
                    ┌─────────────────────┐
                    │    Browser / Client  │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │     Express.js      │
                    │      Application    │
                    └──────────┬──────────┘
                               │
                    ┌──────────┴──────────┐
                    ▼                     ▼
             ┌──────────────┐      ┌──────────────┐
             │    Routes    │      │  Middleware  │
             └──────┬───────┘      └──────────────┘
                    │
                    ▼
             ┌──────────────┐
             │ Controllers  │
             └──────┬───────┘
                    │
                    ▼
             ┌──────────────┐
             │    Models    │
             └──────┬───────┘
                    │
                    ▼
             ┌──────────────┐
             │   MongoDB    │
             └──────────────┘

External Integrations:

        Express
           │
     ┌─────┼─────────┐
     ▼     ▼         ▼
  Stripe  Email    Sharp
```

---

## 🛠️ Tech Stack

### Backend

- Node.js
- Express.js
- JavaScript
- Pug

### Database

- MongoDB
- Mongoose

### Authentication & Security

- JSON Web Tokens (JWT)
- bcryptjs
- HTTP-only cookies
- Helmet
- Express Rate Limit
- Express Mongo Sanitize
- HPP
- XSS protection
- Cookie Parser

### Payments

- Stripe Checkout
- Stripe Webhooks

### Email

- Nodemailer
- SendGrid

### Images

- Multer
- Sharp

### Other

- Axios
- Compression
- Morgan
- Validator
- Slugify
- Parcel

### Deployment

- Docker
- Fly.io
- GitHub Actions

---

## 📁 Project Structure

```text
restaurant/
│
├── controllers/
│   ├── authControllers.js
│   ├── bookingControllers.js
│   ├── errorControllers.js
│   ├── mealsControllers.js
│   ├── orderControllers.js
│   ├── reviewControllers.js
│   ├── userControllers.js
│   └── viewControllers.js
│
├── model/
│   ├── bookingModel.js
│   ├── mealsModel.js
│   ├── orderModel.js
│   ├── reviewModel.js
│   └── usermodel.js
│
├── router/
│   ├── bookingRouter.js
│   ├── mealRouter.js
│   ├── orderRouter.js
│   ├── reviewRouter.js
│   ├── userRouter.js
│   └── viewRouter.js
│
├── utils/
│   ├── apiFeatures.js
│   ├── appError.js
│   ├── catchAsync.js
│   ├── email.js
│   ├── importdata.js
│   └── meal.json
│
├── public/
├── views/
│
├── .github/
│   └── workflows/
│
├── Dockerfile
├── fly.toml
├── app.js
├── server.js
├── package.json
└── package-lock.json
```

The project separates routing, controllers, database models, reusable utilities, and view rendering.

---

## 🔐 Security

Security middleware is configured at the application level.

The project includes:

### HTTP Security Headers

Helmet is used to configure security-related HTTP headers and a Content Security Policy.

### Rate Limiting

API requests are rate limited to reduce excessive requests from a single IP.

### NoSQL Injection Protection

MongoDB query sanitization is enabled through:

```text
express-mongo-sanitize
```

### XSS Protection

User input is processed through XSS protection middleware.

### HTTP Parameter Pollution

HPP protection is enabled with selected whitelisted parameters.

### Authentication Security

- Passwords are hashed using bcrypt.
- JWTs are stored in HTTP-only cookies.
- Production cookies use the `secure` flag.
- Password changes invalidate previously issued JWTs.

---

## 🔑 Authentication Flow

### Registration

```text
User
 │
 ▼
Signup
 │
 ▼
Create User
 │
 ▼
Generate OTP
 │
 ▼
Send OTP Email
 │
 ▼
Verify OTP
 │
 ▼
Activate Account
 │
 ▼
Issue JWT
```

### Login

```text
Email + Password
       │
       ▼
Find User
       │
       ▼
Verify Password
       │
       ▼
Check Email Verification
       │
       ▼
Generate JWT
       │
       ▼
HTTP-only Cookie
```

---

## 🗄️ Database Models

The application uses MongoDB with Mongoose.

### User

Stores:

- Name
- Email
- Password
- Role
- Profile photo
- Account status
- Email verification state
- OTP
- Password reset information
- Password change timestamp

### Meal

Stores restaurant meal information including pricing and images.

### Order

Stores:

- User
- Ordered meals
- Quantity
- Item price
- Total price
- Order status
- Creation date

### Booking

Stores meal/user booking information created through the booking workflow.

### Review

Stores user reviews associated with meals.

---

## 🔌 API Structure

The REST API is organized under:

```text
/api/v1
```

Main resources include:

```text
/api/v1/users
/api/v1/meals
/api/v1/orders
/api/v1/reviews
/api/v1/bookings
```

The application also contains view routes for server-rendered pages.

---

## 🔎 Query Features

Meal listing supports reusable API query features including:

- Filtering
- Sorting
- Field limiting
- Pagination

Example:

```text
/api/v1/meals?sort=price&page=1&limit=10
```

The exact available query parameters depend on the implemented `ApiFeatures` utility.

---

## 🖼️ Image Processing

Meal image uploads use:

```text
Multer → Sharp → JPEG → public/img/meals
```

Supported uploads include:

- One cover image
- Up to three additional meal images

Images are:

1. Validated as image files
2. Loaded into memory
3. Resized to `2000 × 1333`
4. Converted to JPEG
5. Saved with generated filenames

---

## ⚙️ Environment Variables

The application uses environment variables for sensitive configuration.

Typical configuration includes:

```env
NODE_ENV=development
PORT=3000

DATABASE=your_mongodb_connection_string

DATABASE_USERNAME=
DATABASE_PASSWORD=

JWT_SECRET=
JWT_EXPIRES_IN=
JWT_COOKIE_EXPIRES_IN=

STRIPE_SECRET_KEY=
STRIPE_WEBHOOK_SECRET=

EMAIL_USERNAME=
EMAIL_PASSWORD=
EMAIL_HOST=
EMAIL_PORT=
```

> The exact environment variable names should be checked against the current deployment configuration before running the application.

> Never commit `.env` files or production secrets to GitHub.

---

## 🚀 Getting Started

### Prerequisites

Install:

- Node.js
- npm
- MongoDB / MongoDB Atlas
- Git

For Stripe payment development:

- Stripe account
- Stripe CLI (optional for local webhook testing)

---

### 1. Clone the repository

```bash
git clone https://github.com/mohamedashref19/restaurant.git

cd restaurant
```

---

### 2. Install dependencies

```bash
npm install
```

---

### 3. Configure environment variables

Create a `.env` file in the project root.

```env
NODE_ENV=development
PORT=3000
```

Then add the required database, JWT, email, and Stripe configuration.

---

### 4. Run in development

```bash
npm run dev
```

This starts the application using Nodemon.

---

### 5. Run in production

```bash
npm start
```

Which runs:

```bash
node server.js
```

---

## 📦 Available Scripts

### Development

```bash
npm run dev
```

Runs the server with Nodemon.

### Production

```bash
npm start
```

Starts the Node.js server.

### Frontend JavaScript Watch

```bash
npm run watch:js
```

Runs Parcel in watch mode.

### Frontend JavaScript Build

```bash
npm run build:js
```

Builds the frontend JavaScript bundle.

---

## 🐳 Docker

The project includes a Dockerfile for containerized deployment.

Build the image:

```bash
docker build -t restaurant .
```

Run the container:

```bash
docker run -p 3000:3000 restaurant
```

---

## ☁️ Deployment

The project includes Fly.io deployment configuration.

Current Fly.io configuration specifies:

```text
Application: restaurant-muddy-shadow-3798
Internal Port: 3000
HTTPS: Enabled
Region: cdg
Memory: 1 GB
CPU: 1
```

The configuration also enables automatic machine start/stop behavior.

---

## 🌐 Live Application

The repository is configured with a deployed Fly.io application:

```text
https://restaurant-muddy-shadow-3798.fly.dev/
```

---

## 📧 Payment & Booking Workflow

The complete checkout workflow is:

```text
1. User selects meals
        ↓
2. Backend creates Stripe Checkout Session
        ↓
3. User completes payment
        ↓
4. Stripe sends webhook event
        ↓
5. Backend validates Stripe signature
        ↓
6. Backend reads purchased line items
        ↓
7. Booking records are created
        ↓
8. Confirmation email is sent
```

The webhook endpoint is:

```text
POST /webhook-checkout
```

The endpoint receives the raw Stripe request body so that the Stripe signature can be verified correctly.

---

## 📌 Project Highlights

This project demonstrates practical backend development concepts including:

- REST API design
- MVC-style architecture
- MongoDB data modeling
- Authentication
- Authorization
- JWT
- Secure cookies
- Password hashing
- Email verification
- Password reset
- CRUD operations
- Query filtering
- Pagination
- Image upload processing
- Stripe Checkout
- Stripe Webhooks
- Order management
- Booking workflows
- Email notifications
- Security middleware
- Docker
- Cloud deployment

---

## 🔮 Possible Future Improvements

Potential improvements for future versions include:

- Automated integration and unit tests
- OpenAPI / Swagger documentation
- More granular authorization for order/booking operations
- Centralized validation schemas
- Improved webhook processing reliability
- Background job processing for email notifications
- Better observability and structured logging
- CI/CD test gates

---

## 👨‍💻 Author

### Mohamed Ashraf

Backend Engineer focused on:

- Node.js
- Express.js
- MongoDB
- REST APIs
- Authentication & Security
- Payment Integrations
- Backend Architecture

GitHub: [https://github.com/mohamedashref19](https://github.com/mohamedashref19)

Portfolio: [https://mohamed-ashraf-backend.vercel.app/](https://mohamed-ashraf-backend.vercel.app/)

---

## 📄 License

This project is licensed under the ISC License.
