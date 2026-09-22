# E commerce Application

A full-stack e-commerce application with a React single-page frontend and a Spring Boot REST API. It supports product discovery, account authentication, cart and address management, checkout navigation, product administration, and seller/admin workflows.

## Project structure

```text
.
├── ecom-frontend/          # React, Vite, Redux Toolkit, Material UI
├── sb-ecom/                # Spring Boot, Spring Security, JPA, PostgreSQL
└── ecommerce-er-diagram.pdf
```

## Features

- Browse, search, sort, paginate, and filter products by category.
- Register and sign in users with BCrypt password hashing and JWT cookie authentication.
- Support user, seller, and administrator roles.
- Manage carts, item quantities, delivery addresses, products, categories, and product images.
- Use a multi-step checkout interface for address, payment method, order summary, and payment.
- Provide protected administrator routes for dashboard, products, sellers, orders, and categories.

## Technology stack

| Layer | Technologies |
| --- | --- |
| Frontend | React 19, Vite, React Router, Redux Toolkit, Axios, Material UI, Tailwind CSS utilities |
| Backend | Java 17, Spring Boot 3.4, Spring Web, Spring Security, Spring Data JPA, Bean Validation |
| Data | PostgreSQL, Hibernate |
| Payments | Stripe Elements on the frontend |

## Prerequisites

- Node.js 20 or later
- Java 17
- PostgreSQL

## Run locally

### 1. Configure PostgreSQL

Create a local database named `ecommerce` and update the datasource settings in `sb-ecom/src/main/resources/application.properties` to match your local PostgreSQL user and password.

Do not commit real credentials or JWT secrets. Before publishing this repository, replace the current local values with environment-variable based configuration or an ignored local configuration file.

### 2. Start the backend

```bash
cd sb-ecom
./mvnw spring-boot:run
```

The backend starts on Spring Boot's default port, `8080`, unless you configure another port.

### 3. Configure and start the frontend

Create `ecom-frontend/.env.local`:

```env
VITE_BACK_END_URL=http://localhost:8080
VITE_FRONTEND_URL=http://localhost:5173
VITE_STRIPE_PUBLISHABLE_KEY=your_stripe_publishable_key
```

Then run:

```bash
cd ecom-frontend
npm install
npm run dev
```

Open the URL printed by Vite, usually `http://localhost:5173`.

## API overview

| Area | Example endpoints |
| --- | --- |
| Authentication | `POST /api/auth/signup`, `POST /api/auth/signin`, `POST /api/auth/signout` |
| Categories | `GET /api/public/categories`, `POST /api/public/categories` |
| Products | `GET /api/public/products`, `GET /api/public/products/keyword/{keyword}` |
| Cart | `POST /api/carts/products/{productId}/quantity/{quantity}`, `GET /api/carts/users/cart` |
| Addresses | `POST /api/addresses`, `GET /api/users/addresses` |

Most API routes require an authenticated session. Public routes and authentication routes are configured in `WebSecurityConfig`.

## Data model

The main domain entities are `User`, `Role`, `Category`, `Product`, `Cart`, `CartItem`, `Address`, `Order`, `OrderItem`, and `Payment`. The ER diagram is available in [ecommerce-er-diagram.pdf](ecommerce-er-diagram.pdf).

## Available checks

```bash
# Frontend
cd ecom-frontend
npm run lint
npm run build

# Backend
cd ../sb-ecom
./mvnw test
```

## Current limitations

- The frontend contains Stripe payment integration, but corresponding order/payment controller code is not present in the supplied backend source.
- Production deployment configuration, monitoring, automated end-to-end tests, and secure secret management are still required.
- The payment workflow must be verified server-side before treating an order as paid.

## Security note

This project contains local development configuration. Before making the repository public, remove committed credentials and signing secrets, configure secure cookies and HTTPS, restrict CORS, and apply server-side authorization checks to every sensitive seller and administrator action.
