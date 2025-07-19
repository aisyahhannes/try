# **FACULTY OF INFORMATION & COMMUNICATION TECHNOLOGY**
## **BACHELOR IN COMPUTER SCIENCE (SOFTWARE DEVELOPMENT)**
## **BITP 3123 - DISTRIBUTED APPLICATION DEVELOPMENT**

**PROJECT: U-CAFE ORDERING SYSTEM**

**LECTURER: MUHAMMAD FAHEEM BIN MOHD EZANI**

| **MATRIC NO** | **FULL NAME** | **SECTION** |
| --- | --- | --- |
| B032420001 | ‘AINNUR IMAN BINTI ADNAN | S3/1-DE |
| B032420066 | MUHAMMAD AIMAN AFIQ BIN ZAINAL FITRI | S3/1-DE |
| B032420122 | NUR SITI KHADIJAH BINTI MOHD RIZAL | S3/1-DE |
| B032420135 | NURUL AISYAH NABILAH BINTI HANNES | S3/1-DE |
| B032420145 | ROHANA | S3/1-DE |
| B032420155 | SYAFIQAH IRDINA BINTI SHAMSHUL | S3/1-DE |

### **1.0 Introduction**

**1.1 Project Overview**

This report explains the development of the **U-Cafe Ordering System (UOS)** specifically designed for UTeM’s campus cafe. As a UTeM’s student, we had witnessed the inefficiencies of traditional paper-based ordering processes at the cafe. The current food ordering process is manual which leads to order inaccuracies, communication delays between customers and kitchen staff, and time-consuming order processing that leads to extended waiting periods during peak hours. In response to this, we have developed U-Cafe Ordering System to streamline the process where the customer can conveniently place their food and beverage orders through a self-service kiosk while the system will relay these orders in real-time to the kitchen staff. This initiative aims to enhance the overall customer experience and optimize operational workflows for cafe in UTeM campus.

**Target Users:** _Customers (sender-ordering), Kitchen Staff (receiver-receiving orders)._

**1.2 Problem Statements**

- The current paper-based ordering system is prone to errors and miscommunication between customers and kitchen staff.
- Waste of resources as the consumption of paper for order slips leads to increased operational costs and environmental impact.
- Difficult to manage high customer volume during peak hours and time-consuming.

**1.3 Objectives**

- To improve the efficiency and streamline the ordering process.
- To minimize the usage of paper and operational costs by digitalizing the system.
- To provide real-time incoming orders to facilitate efficient tracking and management of order statuses.

**1.4 Scope**

Scope that involved in U-Cafe Ordering System is divided into two parts, which are involvement of user and types of modules. The scope is described as below:

1. Target Users

The customers and kitchen staff are the main users for U-Cafe Ordering System. The system targets the UTeM community including students and staff who frequent the campus cafe. Kitchen staff includes the chefs, cooks, and other kitchen personnel responsible for managing food orders at UTeM Cafe. These users will primarily interact with the U-Cafe Ordering System (UOS).

2. Module
- 1. Self-Service Order Placement

Development of kiosk interface for customers to browse menus, select items and submit them.

- 2. Real-time Order Display

Development of a display system for the kitchen to receive, view, and manage incoming orders digitally.

- 3. Order Status Tracking

Functionality to update the status of orders (Pending, Preparing, Ready)

**1.5 Commercial Value / Third-Party Integration**

This system demonstrates commercial value by digitalizing and streamlining food service operations. By implementing it at UTeM, it could significantly reduce paper waste, improve turnaround time, and increase customer satisfaction through faster service and clearer communication.

**Third-Party Integration:**

- **Mocean API:** Used to send SMS notifications to customers when their orders are ready. The API requires an API key and secret key obtained from the Mocean platform.
- **Justification:** SMS is a reliable and direct communication method to ensure customers are informed especially when they are away from the queue display screen or not present in the cafeteria.

### **2.0 System Architecture**

**2.1 High-Level Diagram**

<img src="md pic/architecture.jpg">

**Frontend/UI:**
- Kiosk for customer to place orders by selecting the menu on the screen
- Kitchen dashboard for staff to handle the incoming orders and prepare the food
**Backend**:
- PHP (REST API) – Handle requests from the kiosk
- Node.js (TCP Server) – Listens for new orders and broadcasts them to the kitchen dashboard in real-time via TCP sockets.
**Database:** MySQL – Stores all data of orders, order items, and menu.
**External Service:** Mocean API – Sends SMS notifications to the customers when the order is ready, using API key authentication.

**3.0 Backend Application**

**3.1 Technology Stack:**

Programming Languages: JavaScript
Frontend: HTML, CSS, JavaScript, PHP
Backend Framework: Node.js with Express
Database: MySQL (via phpMyAdmin)
Server Environment: XAMPP (Apache, MySQL)
Communication: TCP (Node.js net module), RESTful api
SMS Notification: Mocean API
Tools: Postman for API testing

**3.2 API Documentation**

**3.2.1 List of Endpoints**

| **Endpoint**                         | **Method** | **Description**                                       |
|-------------------------------------|------------|-------------------------------------------------------|
| /api_customer/getorder_api.php      | GET        | Get all orders                                        |
| /api_customer/menu_api.php          | GET        | Get all menu items                                    |
| /api_customer/categories_api.php    | GET        | Get menu by category                                  |
| /api_kitchen/order_numbers.php      | GET        | Get order by ID                                       |
| /api_kitchen/orders.php             | GET        | Get all orders (for kitchen)                          |
| /api_kitchen/create_order.php       | POST       | Create a new order                                    |
| /api_kitchen/update_status.php      | PUT        | Update order status                                   |
| /routes/update_status.js            | POST       | Send SMS notification to customer (via Mocean)        |


**3.2.2 HTTP Methods**

- GET: Retrieve data
- POST: Create new records
- PUT: Update existing records

**3.2.3 Request Formats**

<u>POST /update\_status</u>

http://localhost:3000/update_status

Headers: Content-Type: application/json

Request Body:
```js
{

    "order_id": 8,

    "new_status": "preparing"

}
```

<u>POST /create\_order</u>

Request Body:
```js
{

  "customer_name": "Dijah",

  "customer_phone": "60123456789",

  "order_type": "dine_in",

  "items": \[

    {"item_name": "Nasi Lemak Ayam", "quantity": 1}

  ]

}
```

**3.2.4 Responses**

<u>**POST /update\_status**</u>

- **Success**
**Status: 200 OK**
```js
{
    "message": "Status updated successfully"
}
```

- **Error: Update Status Failed**
**Status: 500 Internal Server Error**
```js
{
“error”: “Failed to update status”
}
```

<u>**POST /create\_order**</u>

- **Success**

**Status: 201 Created**
```js
{
    "message": "Order created",
    "order_id": 18
}
```

- **Error: Create Order Failed**
**Status: 400 Bad Request**
```js
{
    “error”: “Failed to create order”
}
```

**3.2.5 Security**

**Mechanism Used: API Key Authentication**

The Kiosk Ordering System implements API Key-based security to protect backend API endpoints from unauthorized access. This mechanism ensures that only authorized clients (customer kiosk or kitchen dashboard) can interact with the server.

**Justification for the Chosen Mechanism**

- Simplicity: Easy to implement and manage for a system without user accounts or login.
- Suitable for Internal Use: Ideal for kiosk systems on a local network or controlled environment.
- Low Overhead: No need for token generation, storage, or expiration handling like JWT

| **Feature** | **Description** |
| --- | --- |
| Authentication | Only requests with the correct API key can access the API. Others receive a 401 Unauthorized response. |
| Endpoint Protection | All sensitive endpoints are protected using this middleware. |
| Environmental Storage | The API key is stored in the .env file, preventing hardcoding in source files and reducing security risks. |

**4.0 Frontend Application**

**2.3.1 Self-Service Kiosk Interface (Customer)**

- Purpose: A touch-based interface where customers can place food orders, select dining options (e.g., dine-in, takeaway), input contact info, view the cart, and place order.
- Target User: Walk-in café customers using a self-service kiosk.
- Technology Stack: HTML, CSS, JavaScript
- API Integration:

\- Sends POST requests to `/api\_kitchen/create\_order.php` with order data.

\- Uses GET requests to `/api\_customer/menu\_api.php` and `/api\_customer/categories\_api.php` to dynamically load menu items by category.

\- API Key is added in headers (x-api-key) for secure communication.

**2.3.2 Kitchen Dashboard Interface (Kitchen)**

- Purpose: Displays real-time incoming orders and allows kitchen staff to update the status of each order (e.g., pending → preparing → ready).
- Target User: Kitchen staff and servers managing order fulfillment.
- Technology Stack: HTML, CSS, JavaScript, WebSocket, Axios
- API Integration:

\- Uses GET requests to `/api\_kitchen/orders.php` to load all active orders.

\- Sends PUT requests to `/api\_kitchen/update\_status.php` to update order status.

\- Triggers SMS to customer by POST request to `/routes/update\_status.js` after status is updated to "ready".

**5.0 Database Design**

**5.1 Entity-Relationship Diagram (ERD)**
<img src="md pic/erd.jpg">

**5.2 Scheme Justification**

- The database consists of three main tables which are orders, menu\_items, and order\_items.
- Table orders holds the overall order info including customer and type (dine-in/takeaway).
- Table order\_items allows multiple items per order (**one-to-many** relationship with orders).
- Table menu\_items keeps menu organized and simplifies UI filtering.

**6.0 Business Logic and Data Validation**

**6.1 Flowcharts**
<img src="md pic/flowchart cust.png">
<img src="md pic/flowchart kitchen.png">

**6.2 Data Validation**

**Frontend:**

- Required Fields: Customer name and phone number must be filled in.
- Phone Format: Phone number must start with 60 (Malaysia calling code) and contain only digits, with a total length of 11–12 digits (e.g., 60123456789).
- Cart Check: Order cannot be submitted if no items are selected.

**Backend:**

- Input Validation: All required fields (name, phone, items) must be presented and non-empty.
- Status Constraint: Every new order is inserted into the database with a default status of "Pending".
- Status Flow Validation: Order status can only change in the sequence Pending → Preparing → Ready. An order cannot be marked as "Ready" unless it was first marked as "Pending".

**7.0 Conclusion**

The **U-Cafe Ordering System** was developed to address inefficiencies in UTeM’s existing café ordering process. By digitalizing the workflow, the system enhances order accuracy, reduces customer waiting time, minimizes paper usage, and significantly improves the overall user experience for both customers and kitchen staff.

Key feature is the integration of SMS notifications using the Mocean API, which improves communication by notifying customers when their orders are ready for pickup. This reduces the need for manual updates and enhances service efficiency.

Looking ahead, the system offers strong potential for future enhancement, including:

- Mobile ordering via smartphones
- Online payment integration
- Customer loyalty programs

Overall, the U-Cafe Ordering System lays the foundation for a smarter, more efficient, and user-friendly ordering experience on campus.

