# 🍕 Food Delivery Backend

A production-ready REST API backend for a food delivery platform (similar to Swiggy/Zomato), built with Java Spring Boot.

---

## 🛠️ Tech Stack

| Technology | Purpose |
|------------|---------|
| Java 17+ | Core language |
| Spring Boot  | Backend framework |
| Spring Security | Authentication & Authorization |
| JWT (JJWT 0.13) | Stateless token-based auth |
| Spring Data JPA | Database ORM |
| PostgreSQL | Relational database |
| Lombok | Boilerplate reduction |
| Maven | Dependency management |

---

## 🏗️ Architecture

The project follows a **modular layered architecture**:
```
food-delivery-backend/
│
├── auth/           → Register, Login, JWT
├── restaurant/     → Restaurant management
├── menu/           → Categories & Menu items
├── cart/           → Cart management
├── order/          → Order lifecycle
└── config/         → Security, JWT, Exception handling
```

Each module contains:
- `entity/`     → JPA entities (database tables)
- `dto/`        → Request/Response objects
- `repository/` → Database queries
- `service/`    → Business logic
- `controller/` → REST API endpoints

---

## 👥 User Roles

| Role | Permissions |
|------|-------------|
| `CUSTOMER` | Browse restaurants, manage cart, place & cancel orders |
| `RESTAURANT_OWNER` | Manage restaurants, menu items, update order status |
| `DELIVERY_AGENT` | (Future scope) Accept & track deliveries |
| `ADMIN` | (Future scope) Platform management |

---

## 🔐 Security

- **JWT Authentication** — Stateless, token-based
- **Role-based Authorization** — `@PreAuthorize` on every sensitive endpoint
- **BCrypt Password Encoding** — Passwords never stored in plain text
- **Input Validation** — `@Valid` + `@NotBlank` on all request DTOs

---

## 📌 API Endpoints

### Auth
| Method | Endpoint | Access | Description |
|--------|----------|--------|-------------|
| POST | `/api/auth/register` | Public | Register new user |
| POST | `/api/auth/login` | Public | Login & get JWT token |
| GET | `/api/auth/me` | Authenticated | Get current user info |

### Restaurants
| Method | Endpoint | Access | Description |
|--------|----------|--------|-------------|
| POST | `/api/restaurants` | RESTAURANT_OWNER | Add restaurant |
| GET | `/api/restaurants` | Public | Get all open restaurants |
| GET | `/api/restaurants/my` | RESTAURANT_OWNER | Get my restaurants |
| PUT | `/api/restaurants/{id}/toggle` | RESTAURANT_OWNER | Toggle open/close |

### Menu
| Method | Endpoint | Access | Description |
|--------|----------|--------|-------------|
| POST | `/api/menu/category` | RESTAURANT_OWNER | Add category |
| GET | `/api/menu/category/restaurant/{id}` | Public | Get categories |
| POST | `/api/menu/item` | RESTAURANT_OWNER | Add menu item |
| GET | `/api/menu/restaurant/{id}` | Public | Get full menu |
| GET | `/api/menu/restaurant/{id}/available` | Public | Get available items |
| PUT | `/api/menu/item/{id}/toggle` | RESTAURANT_OWNER | Toggle availability |

### Cart
| Method | Endpoint | Access | Description |
|--------|----------|--------|-------------|
| POST | `/api/cart/add` | Authenticated | Add item to cart |
| GET | `/api/cart` | Authenticated | View cart |
| DELETE | `/api/cart/item/{id}` | Authenticated | Remove item |
| DELETE | `/api/cart/clear` | Authenticated | Clear cart |

### Orders
| Method | Endpoint | Access | Description |
|--------|----------|--------|-------------|
| POST | `/api/orders/place` | CUSTOMER | Place order from cart |
| GET | `/api/orders/my` | CUSTOMER | Get my orders |
| GET | `/api/orders/restaurant/{id}` | RESTAURANT_OWNER | Get restaurant orders |
| PUT | `/api/orders/{id}/status` | RESTAURANT_OWNER | Update order status |
| PUT | `/api/orders/{id}/cancel` | CUSTOMER | Cancel order |

---

## ⚙️ Setup & Run

### Prerequisites
- Java 17+
- PostgreSQL
- Maven

### Steps

1. **Clone the repository**
```bash
git clone https://github.com/yourusername/food-delivery-backend.git
cd food-delivery-backend
```

2. **Create PostgreSQL database**
```sql
CREATE DATABASE food_delivery_db;
```

3. **Configure application.properties**
```properties
spring.datasource.url=jdbc:postgresql://localhost:5432/food_delivery_db
spring.datasource.username=postgres
spring.datasource.password=your_password
spring.jpa.hibernate.ddl-auto=update
```

4. **Run the application**
```bash
mvn spring-boot:run
```

5. **API is running at**
```
http://localhost:8080
```

---

## 🔄 Order Lifecycle
```
PENDING → CONFIRMED → PREPARING → OUT_FOR_DELIVERY → DELIVERED
                                                    
         CANCELLED (only from PENDING state)
```

---

## 📐 Database Schema
```
users
restaurants  (owner_id → users)
categories   (restaurant_id → restaurants)
menu_items   (category_id → categories, restaurant_id → restaurants)
carts        (user_id → users, restaurant_id → restaurants)
cart_items   (cart_id → carts, menu_item_id → menu_items)
orders       (customer_id → users, restaurant_id → restaurants)
order_items  (order_id → orders, menu_item_id → menu_items)
```

---

## 🚀 Future Scope

- Payment integration (Razorpay)
- Real-time delivery tracking
- Reviews & Ratings
- Push notifications
- Microservices migration

---

## 👨‍💻 Author

**Ritik Hedau**  
Java Backend Developer  
[GitHub](https://github.com/ritik-hedau18) | [LinkedIn](https://www.linkedin.com/in/ritik-hedau-4b51b2203/)
