# 🛒 E-Commerce API — ASP.NET Core (3-Layer)

A scalable, 3-layer Web API (DAL → BLL → PL) with a generic repository, JWT authentication, cart and checkout, and product image uploads.

![.NET](https://img.shields.io/badge/.NET-9.0-blue)

<hr>
<br>

## 📌 Table of Contents
- [🚀 Overview](#-overview)
- [📐 Architecture](#-architecture)
- [🧩 Key Features](#-key-features)
- [🚀 Tech Stack](#-tech-stack)
- [📁 Project Structure](#-project-structure)
- [🔑 Authentication Flow](#-authentication-flow)
- [📦 API Modules](#-api-modules)
- [❌ Error Handling](#-error-handling)
- [⚙️ Getting Started](#setup)
- [🔐 Environment Variables](#-environment-variables)
- [📘 API Documentation](#-api-documentation)
- [📞 Contact](#-contact)

<br>
<hr>
<br>

## 🚀 Overview
The **E-Commerce API** is a backend for products, cart, checkout, reviews, and admin tasks.

1. Built a REST API with 37 endpoints across 12 controllers using ASP.NET Core 9 and EF Core (Code First, SQL Server), in a 3-layer layout (DAL/BLL/PL) with dependency injection, a generic repository, and Mapster DTO mapping. OpenAPI is exposed in Development through Scalar. There is no global exception handler.

2. Implemented JWT authentication with role checks for `customer`, `admin`, and `superAdmin`, plus email confirmation, forgot/reset password, and admin user management (block, unblock, and role change).

3. Admin product list with pagination and create with a main image plus extra images; customer cart add and cart summary; checkout with Cash and Visa (Stripe); customer review submit with a rating. Admin CRUD for brands and categories, order list by status and status updates (Pending, Cancelled, Approved, Shipped, Delivered). One admin PDF lists product id and name.

<br>

## 🧩 Key Features
* 📈 **Scalable architecture:** The 3-layer separation (DAL / BLL / PL), dependency injection, and a generic repository make it easy to add new features and grow the system.
* 🔐 **Identity:** JWT auth with roles `customer`, `admin`, and `superAdmin`. Email confirmation is required. Forgot-password and reset-password endpoints exist.
* 🛍️ **Catalog:** Categories and brands support create, read, update, delete, and status toggle. Products support an admin paginated list and create only.
* 🖼️ **Media:** Product create accepts one main image and a list of extra images.
* 🛒 **Cart and checkout:** A customer can add a cart item and get a cart summary. Checkout supports Cash and Visa (Stripe Checkout), plus a payment-success endpoint.
* 📦 **Orders and report:** Admins can list orders by status and change status. The report endpoint returns a PDF of product id and name.
* 🗂️ **Data access:** Generic repository for some entities, and Mapster mapping in several services.
* 📝 **Reviews:** A customer can submit a comment and a rating. There is no list, update, or delete review endpoint.

<br>

## 🚀 Tech Stack
* **Framework:** ASP.NET Core 9 (Web API)
* **ORM:** Entity Framework Core (Code First)
* **Database:** SQL Server
* **Security:** JWT Bearer Authentication
* **Documentation:** OpenAPI and Scalar in Development. There is no Swagger UI.
* **Architecture:** 3-layer (DAL / BLL / PL). The PL project references DAL.
* **Mapping:** Mapster
* **Dependency Injection**

<br>

## 📐 Architecture
This project follows a **3-layer architecture**:
```
PL  → Controllers / API
BLL → Business Logic & Services
DAL → Data Access (EF Core + Repositories)
```
Controllers call BLL service interfaces. PL also references DAL for DTOs and models. `ReportsController` injects the `ReportService` class directly.

<br>

## 📁 Project Structure
```plaintext
Ecommerce_App
│
├── DAL
│   ├── Data_Base
│   │   ├── Migrations
│   │   └── ApplicationDbContext.cs
│   ├── DTO
│   ├── Models
│   ├── Utils
│   └── Repositories
│       ├── Interfaces
│       └── Classes
│
├── BLL
│   └── Services
│       ├── Interfaces
│       └── Classes
│
└── PL
    ├── Areas (Controllers)
    │   ├── Admin
    │   ├── Identity
    │   └── Customer
    ├── Utils
    ├── appsettings.json
    └── Program.cs
```

<br>
<hr>
<br>

## 🔑 Authentication Flow

Authentication is implemented using **JWT Bearer Tokens**.

```
Authorization: Bearer <token>
```

Register can send a confirmation email. Login returns a JWT access token after the email is confirmed. Token validation is handled by JWT Bearer middleware. Issuer and audience checks are turned off.

<br>

## 📦 API Modules

* **Authentication:** Register, login, confirm email, forgot password, and reset password.
* **Admin - Products:** Paginated list and create (main image and extra images). No update or delete product endpoint.
* **Admin - Categories:** Create, read, update, delete, and status toggle. Categories are a flat list.
* **Admin - Brands:** Create, read, update, delete, and status toggle.
* **Admin - Orders:** List orders by status and change status (`Pending`, `Cancelled`, `Approved`, `Shipped`, `Delivered`). No carrier or tracking endpoint.
* **Admin - Reports:** PDF of product id and name. Not a sales or user-activity report.
* **Customer - Browse and cart:** List and get brands and categories. Add a cart item and get the cart summary. There is no customer product-catalog endpoint.
* **Customer - Checkout:** Cash, or Visa through Stripe Checkout, plus a success endpoint.
* **Customer - Reviews:** Submit a product rating and comment.
* **User management:** List users, get one user, block, unblock, check block status, and change role.

<br>

## ❌ Error Handling

There is no global exception middleware. Several services throw `Exception`. Many actions return `200 OK`. Checkout can return `200` with `Success: false` when the cart is empty.

<br>
<hr>
<br>

<a name="setup"></a>
## ⚙️ Getting Started

### Prerequisites
- .NET SDK 9.0
- SQL Server

### Installation
From the repository folder:

```
dotnet restore Ecommerce_App.sln
```

### Database Setup
Pending EF Core migrations run on startup. You can also update the database from the repository folder:

```
dotnet ef database update --project DAL --startup-project PL
```

### Run Application
```
dotnet run --project PL
```

### API Access
In Development the Scalar UI is at:

```
https://localhost:7050/scalar
```

OpenAPI is mapped in Development. There is no `/swagger` page.

<br>

## 🔐 Environment Variables

Set these in `appsettings.json` or as environment variables. Do not commit real secrets.

| Key                                  | Description                                      |
|--------------------------------------|--------------------------------------------------|
| ConnectionStrings:DefaultConnection  | SQL Server connection string                     |
| jwtOptions:SecretKey                 | JWT signing secret key                           |
| Stripe:SecretKey                     | Stripe secret key used for Visa checkout         |

SMTP settings for confirmation and password emails are hardcoded in `PL/Utils/EmailSending.cs`. They are not read from configuration.

<br>
<hr>
<br>

## 📘 API Documentation
[To see the api document of this project click here](./Docs/Api_Document.md)

<br>

## 📞 Contact

- 📧 **Email**: [mahmoudjawad02025@gmail.com](mailto:mahmoudjawad02025@gmail.com)
- 💻 **GitHub Profile**: [@mahmoudjawad-2025](https://github.com/mahmoudjawad-2025/)
- 💼 **LinkedIn:** [linkedin.com/in/mahmoud-abu-alsebaa](https://linkedin.com/in/mahmoud-abu-alsebaa)
