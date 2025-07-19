# FACULTY OF INFORMATION & COMMUNICATION TECHNOLOGY  
## BACHELOR IN COMPUTER SCIENCE  
### (SOFTWARE DEVELOPMENT)  
### BITP 3123 - DISTRIBUTED APPLICATION DEVELOPMENT  

---

# PROJECT:  
## U-CAFE ORDERING SYSTEM

### LECTURER  
**MUHAMMAD FAHEEM BIN MOHD EZANI**

| **MATRIC NO** | **FULL NAME** | **SECTION** |
|---------------|-------------------------------|------------|
| B032420001     | ‘AINNUR IMAN BINTI ADNAN      | S3/1-DE    |
| B032420066     | MUHAMMAD AIMAN AFIQ BIN ZAINAL FITRI | S3/1-DE |
| B032420122     | NUR SITI KHADIJAH BINTI MOHD RIZAL | S3/1-DE |
| B032420135     | NURUL AISYAH NABILAH BINTI HANNES | S3/1-DE |
| B032420145     | ROHANA                        | S3/1-DE    |
| B032420155     | SYAFIQAH IRDINA BINTI SHAMSHUL | S3/1-DE   |

---

# 1.0 Introduction

## 1.1 Project Overview

This report explains the development of the **U-Cafe Ordering System (UOS)** specifically designed for UTeM’s campus cafe. As UTeM students, we witnessed the inefficiencies of traditional paper-based ordering processes. The manual food ordering process leads to order inaccuracies, communication delays between customers and kitchen staff, and extended waiting periods during peak hours.  

To solve these issues, we developed U-Cafe Ordering System to streamline the process. Customers can conveniently place their food and beverage orders through a **self-service kiosk**, and the system relays these orders in **real-time** to the kitchen staff. This improves customer experience and optimizes cafe operations.

**Target Users:** _Customers (ordering), Kitchen Staff (receiving orders)_

## 1.2 Problem Statements

- Paper-based ordering is prone to errors and miscommunication.
- Paper slips increase operational costs and environmental waste.
- Manual systems struggle during peak hours, causing delays.

## 1.3 Objectives

- Streamline the food ordering process efficiently.
- Reduce paper usage and operational costs.
- Provide real-time order tracking for better kitchen management.

## 1.4 Scope

### 1. Users
- **Customers**: UTeM students and staff placing orders.
- **Kitchen Staff**: Cooks and staff managing incoming orders.

### 2. Modules
- **Self-Service Order Placement**: Interface for customers to browse and order.
- **Real-Time Order Display**: Dashboard for kitchen staff to view and manage orders.
- **Order Status Tracking**: Order states (Pending, Preparing, Ready).

## 1.5 Commercial Value / Third-Party Integration

The system modernizes and digitizes cafe operations, reducing waste and improving service speed and customer satisfaction.

### Third-Party Integration
- **Mocean API**: Sends SMS notifications when orders are ready.
- **Justification**: SMS is reliable and ensures customer notification, even if away from the display.

---

# 2.0 System Architecture

## 2.1 High-Level Diagram

### Frontend/UI:
- **Kiosk**: For customers to place orders.
- **Kitchen Dashboard**: For staff to receive and process orders.

### Backend:
- **PHP (REST API)**: Handles kiosk requests.
- **Node.js (TCP Server)**: Real-time order updates via TCP sockets.

### Database:
- **MySQL**: Stores menu, orders, and order items.

### External Service:
- **Mocean API**: Sends SMS notifications using API key authentication.

---

# 3.0 Backend Application

## 3.1 Technology Stack

- **Programming Languages**: JavaScript
- **Frontend**: HTML, CSS, JavaScript, PHP
- **Backend**: Node.js with Express
- **Database**: MySQL (via phpMyAdmin)
- **Server**: XAMPP (Apache, MySQL)
- **Communication**: TCP (Node.js net module), REST API
- **SMS Notification**: Mocean API
- **Tools**: Postman (for API testing)

## 3.2 API Documentation

### 3.2.1 List of Endpoints

| **Endpoint**                             | **Method** | **Description**                        |
|------------------------------------------|------------|----------------------------------------|
| /api_customer/getorder_api.php           | GET        | Get all orders                         |
| /api_customer/menu_api.php               | GET        | Get all menu items                     |
| /api_customer/categories_api.php         | GET        | Get menu by category                   |
| /api_kitchen/order_numbers.php           | GET        | Get order by ID                        |
| /api_kitchen/orders.php                  | GET        | Get all kitchen orders                 |
| /api_kitchen/create_order.php            | POST       | Create a new order                     |
| /api_kitchen/update_status.php           | PUT        | Update order status                    |
| /routes/update_status.js                 | POST       | Send SMS notification (Mocean)         |

### 3.2.2 HTTP Methods

- **GET**: Retrieve data
- **POST**: Create data
- **PUT**: Update data

### 3.2.3 Request Formats

#### POST /update_status  
**URL:** `http://localhost:3000/update_status`  
**Headers:**  
`Content-Type: application/json`  
**Request Body:**

```json
{
  "order_id": 8,
  "new_status": "preparing"
}
```

#### POST /create_order

```json
{
  "customer_name": "Dijah",
  "customer_phone": "60123456789",
  "order_type": "dine_in",
  "items": [
    {"item_name": "Nasi Lemak Ayam", "quantity": 1}
  ]
}
```

### 3.2.4 Responses

#### POST /update_status

- **Success**
```json
{
  "message": "Status updated successfully"
}
```

- **Error**
```json
{
  "error": "Failed to update status"
}
```

#### POST /create_order

- **Success**
```json
{
  "message": "Order created",
  "order_id": 18
}
```

- **Error**
```json
{
  "error": "Failed to create order"
}
```

### 3.2.5 Security

**Mechanism:** API Key Authentication

| **Feature**            | **Description**                                                                 |
|------------------------|---------------------------------------------------------------------------------|
| Authentication         | Requests without correct API key receive 401 Unauthorized                      |
| Endpoint Protection    | Sensitive endpoints protected by middleware                                     |
| Environmental Storage  | Keys stored in `.env` file, not hardcoded in source                            |

**Justification:**

- Simple and suitable for internal use
- No need for login or token storage
- Easy to implement

---

# 4.0 Frontend Application

## 4.1 Self-Service Kiosk Interface (Customer)

- **Purpose**: Interface for ordering, cart, and submitting order.
- **Target User**: Walk-in customers.
- **Tech**: HTML, CSS, JavaScript
- **API Usage**:
  - `GET` from `/api_customer/menu_api.php` and `/categories_api.php`
  - `POST` to `/api_kitchen/create_order.php`
  - Secure with `x-api-key` in headers

## 4.2 Kitchen Dashboard Interface (Kitchen)

- **Purpose**: Displays and updates order status.
- **Target User**: Kitchen staff.
- **Tech**: HTML, CSS, JavaScript, WebSocket, Axios
- **API Usage**:
  - `GET` from `/api_kitchen/orders.php`
  - `PUT` to `/api_kitchen/update_status.php`
  - `POST` to `/routes/update_status.js` (triggers SMS)

---

# 5.0 Database Design

## 5.1 Entity-Relationship Diagram (ERD)
*(Diagram not included in Markdown)*

## 5.2 Schema Justification

- **orders**: Holds customer info and order type.
- **order_items**: Linked via `order_id` (one-to-many with orders).
- **menu_items**: Simplifies filtering and display.

---

# 6.0 Business Logic and Data Validation

## 6.1 Flowcharts
*(Flowcharts not included in Markdown)*

## 6.2 Data Validation

### Frontend:

- **Required**: Name and phone number
- **Phone Format**: Must start with `60` and be 11–12 digits (e.g., `60123456789`)
- **Cart Check**: Cannot submit if no items selected

### Backend:

- **Field Validation**: All fields must be filled
- **Status Constraint**: All new orders are `Pending` by default
- **Status Flow**: Only valid transitions allowed (`Pending → Preparing → Ready`)

---

# 7.0 Conclusion

The **U-Cafe Ordering System** addresses the inefficiencies in the current café ordering process at UTeM. It improves order accuracy, reduces waiting time, and minimizes paper usage.  

A key feature is SMS integration using Mocean API, which notifies customers when their orders are ready — enhancing service efficiency.  

**Future Enhancements:**

- Mobile ordering
- Online payments
- Customer loyalty features

The system establishes a foundation for a smarter and more user-friendly ordering experience at UTeM Café.