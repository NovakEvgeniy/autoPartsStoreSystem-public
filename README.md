# autoPartsStoreSystem

**autoPartsStoreSystem is a web-based e-commerce application designed to manage an online store specializing in automotive spare parts and related products. It provides functionality for managing product categories and brands, product filtering, price lists, product cards, promotions, customer orders, delivery, and other essential e-commerce operations.**

**Keywords:** Java, Spring Boot, Spring MVC, Spring Data JPA, Hibernate, MySQL, Maven, HTML5, CSS3, JavaScript, AJAX, REST API, Multi-Page Application (MPA), Server-Side Rendering (SSR), Feature-Based Architecture, Modular Monolith, UI-First Development, Automotive spare parts, Automotive related products, Internet Store, E-Commerce, Shopping Cart, Online Checkout, Order Management, Payment Processing, Customer Reviews, Product Reviews, Promotions, FAQ, Site Content.

## 1.  About the Project

### 1.1. Project Overview

- autoPartsStoreSystem is an independent information system that can be used as a complementary solution to an enterprise accounting system.
- Product data is transferred to the online store based on an internal accounting document containing a list of products and their respective selling prices.
- The online store does not perform accounting operations and is not responsible for calculating product costs, purchase prices, trade markups, profits, or taxesв.
- The same selling price, received from the accounting system, is used both when importing product data into the online store and when processing sales.

### 1.2. System Users

- The current MVP version supports three user types:
- **User with the ADMIN role** — Delivery of products to customers via Nova Poshta, manages products, catalog data, orders, reviews, promotions, FAQ and site content.
- **User with the BUYER role** — browses the online store, views products and their specifications, manages the shopping cart, proceeds through checkout, and places orders.
- **User with the ACCOUNT role** — processes internal accounting documents containing product lists and selling prices, and transfers the relevant product data to the online store

## 2. Architecture

### 2.1. Frontend

- **Web Interface Architecture:** MPA (Multi-Page Application) + SSR (Server-Side Rendering) + AJAX (Asynchronous JavaScript and XML)
- **Technology Stack:** HTML5, CSS3, JavaScript (ES6), AJAX (Fetch API)
- **Client–Server Communication:** Server-side rendering (SSR) using Spring MVC and Thymeleaf, combined with asynchronous AJAX requests to REST API endpoints for dynamic data updates without full page reloads.

### 2.2. Backend

- **Architecture:** Feature-Based Modular Monolith with layered architecture (Entity, Controller, Service, Repository)
- **Database:** MySQL, Spring Data JPA, Hibernate
- **Security:** Spring Security-based authentication and role-based authorization, with BCrypt password hashing, form-based login, session management, logout, and role-specific access control.
- **Technology Stack:** Java 21, Spring Boot, Spring Data JPA, Hibernate, Maven

## 3. Launch and Deployment

### 3.1. Prerequisites

-   Java 21
-   Maven 3.6+
-   MySQL 8.0+

### 3.2. Run the Project Using the Executable JAR File

#### 3.2.1. Create the Database

- Create a new database named `auto_parts_store` using the following SQL command:

```sql
CREATE DATABASE auto_parts_store
CHARACTER SET utf8mb4
COLLATE utf8mb4_unicode_ci;
```

#### 3.2.2. Create an External application.properties File

- Create a file named application.properties in the same folder where the executable JAR will be placed.
- Add the following configuration:

```
spring.application.name=autoPartsStoreSystem

server.port=8080

spring.thymeleaf.cache=false

# MySQL
spring.datasource.url=jdbc:mysql://localhost:3306/auto_parts_store?useSSL=false&serverTimezone=UTC&allowPublicKeyRetrieval=true
spring.datasource.username=YOUR_MySQL_USERNAME
spring.datasource.password=YOUR_MySQL_PASSWORD

# JPA / Hibernate
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.format_sql=true
spring.jpa.database-platform=org.hibernate.dialect.MySQLDialect


# NovaPoshta
nova.poshta.api-key=DEV_TEST_KEY
nova.poshta.api.enabled=false

```
- Replace `YOUR_MySQL_USERNAME` and `YOUR_MySQL_PASSWORD` with your own MySQL username and password.
- The application uses Hibernate (`spring.jpa.hibernate.ddl-auto=update`), which automatically creates all required database tables when the application is started for the first time.

#### 3.2.3. Download the Executable JAR

- Download the executable JAR from the link below and place it in the same folder as `application.properties`

👉 [** Download autoPartsStoreSystem-0.0.1-SNAPSHOT.jar**](https://github.com/NovakEvgeniy/autoPartsStoreSystem-public/releases/download/v1.0.0/autoPartsStoreSystem-0.0.1-SNAPSHOT.jar)

#### 3.2.4. Run the Application

- Run the following command from the folder containing both files: 

``` bash
java -jar autoPartsStoreSystem-0.0.1-SNAPSHOT.jar
```

#### 3.2.5. Launch in the Browser

-   **URL: http://localhost:8080/**

## 4. Application Functional Module Structure

### 4.1. Main Product Flow Across the Application's Functional Modules

```
Stage 1 — ACCOUNT: Accounting
Recording incoming goods in the store.
↓
Stage 2 — BUYER: Purchasing
Selecting products, placing an order, and making payment.
↓
Stage 3 — ADMIN: Delivery
Shipping products to the customer via Nova Poshta through the Admin Dashboard – Delivery section.
```

#### 4.1.1. Instructions for Managing Product Flow Through the Functional Sequence  
```
I. Creating an ADMIN Account
Participants: Application owner and Spring Boot application.
1. Before the first application startup, open application.properties.
2. Replace the placeholder configuration:

app.initial-admin.email=${ADMIN_EMAIL:}
app.initial-admin.password=${ADMIN_PASSWORD:}

with the application owner's email address and password (for example):

app.initial-admin.email=admin@gmail.com
app.initial-admin.password=Admin

3. Start the application. If the database is empty, InitialAdminDataLoader.java will create an account with the ADMIN role
 and store the encoded credentials in the users table.

II. Description of the Process for Creating an ACCOUNT Role
Participant: User with the ADMIN role.
ADMIN
↓
Open [http://localhost:8080], where only the Login menu is displayed.
↓
Log in at [http://localhost:8080/login].
↓
Return to [http://localhost:8080/] with the role-specific navigation menu:
Home | Admin Dashboard | Logout.
A message confirms that the user is logged in as ADMIN.
↓
Open Admin Dashboard from the navigation menu.
↓
The Administration Dashboard page opens at [http://localhost:8080/admin] (admin.html).
The dashboard contains functional sections, including Open Delivery.
↓
Open the Open Accountant Accounts section.
↓
The Accountant Accounts page opens at [http://localhost:8080/admin/accountants] (accountants.html).
↓
The administrator specifies the accountant's email address. The password is generated automatically or entered manually.
In either case, the email address and password are provided to the accounting department for future use, for example:
- Email: Account@gmail.com
- Password: Account
The registration process is completed by clicking Create Accountant.

Result:
The Accountant Accounts form is cleared, and a confirmation message appears indicating that the ACCOUNTANT account
has been successfully created.
A corresponding record is added to the database users table.
The accounting department uses the same account credentials for all subsequent product deliveries.

III. Description of the Process of Populating the Database with Product Data by the Accounting Department
Participant: User with the ACCOUNT role.
After receiving the login credentials, the process is continued by the accounting department.
ACCOUNT
↓
Opens the `home.html` page at [http://localhost:8080/].
The navigation bar displays a single option: Login.
↓
Click Login.
↓
The login page opens at [http://localhost:8080/login] (login.html).
The accountant enters their email address and the password received from the ADMIN.
↓
Returns to [http://localhost:8080/] (home.html), now with the role-specific navigation menu:
Home | Product Input | Logout
A message confirms that the user is logged in as ACCOUNTANT.
↓
Click Product Input.
↓
The Accountant — Internal Document page opens at [http://localhost:8080/accountant] (dashboard.html).
The navigation bar contains Home and Logout.
The form on this page is designed to enter product details for goods received by the store, based on internal
accounting documents.
The Add Product button allows the accountant to add multiple products listed under the same Price List Number
in the document.
After entering all product details, click Process Document.
↓
Process Document
↓
The current page (accountant.html) is cleared, and a confirmation message appears stating that the document with
the specified number has been successfully processed and saved to the database.
This completes the accounting department's work.

Result:
The submitted data is saved in the four main product-related database tables:
- categories
- brands
- products
- product_specifications
It is also saved in the associated accounting document tables:
- accountant_document_item_specifications
- accountant_document_item
- accountant_document

IV. Description of the Online Store Purchasing Process
Participant: User with the BUYER role.
Opens the `home.html` page at [http://localhost:8080/].
The navigation bar displays a single option: Login.
↓
Click Login.
↓
The login.html page opens at [http://localhost:8080/login].
Unlike ADMIN and ACCOUNTANT users, a buyer registers their own account and then logs in on the same page.
↓
Login
↓
The Home page opens at [http://localhost:8080/] (home.html).
The navigation bar displays:
Home | Catalog | Promotions | Cart | My Account | Logout
A message confirms that the user is logged in as BUYER.
The Home page displays only three products from the entire catalog, with links to Catalog and View Catalog,
and only three promotions from the complete list of available promotions.
↓
Catalog
↓
The Catalog page opens at [http://localhost:8080/catalog] (catalog.html).
The navigation bar displays:
Home | Catalog | Promotions | Cart
Here, the buyer can browse the entire product catalog and open individual product pages by clicking View Product.
↓
View Product
↓
The Product Description page opens at [http://localhost:8080/product/] (product.html).
The navigation bar displays:
Home | Catalog | Promotions | Cart
This page allows the buyer to review the selected product and its details before adding it to the shopping cart.
↓
Click Add to Cart.
↓
Cart
The Cart page opens at [http://localhost:8080/cart] (cart.html).
The navigation bar displays:
Home | Catalog | Promotions | Cart
Selected products are accumulated on this page. The buyer can continue shopping by clicking Continue Shopping.
A permanent purchase record has not yet been created in the database.
The buyer can modify the Quantity value or remove products using Remove.
↓
Click Proceed to Checkout.
↓
Checkout
The Checkout page opens at [http://localhost:8080/checkout] (checkout.html).
The navigation bar displays:
Home | Catalog | Cart
The BUYER enters the following information:
- Full Name
- Email
- Phone
- City
- Delivery Method
- Nova Poshta Branch
- Payment Method
↓
Click Place Order.
↓
Bank Card Payment (Payment Simulation)
The payment page opens at [http://localhost:8080/payment/] (payment.html).
The navigation bar is not displayed.
1. The page displays the order number and the total amount to be paid.
2. The BUYER enters their name and fills in the simulated bank card details, using the predefined test card number:
   4242 4242 4242 4242
↓
Click Pay.
↓
Order Confirmed
The final order confirmation page opens at [http://localhost:8080/order/] (order.html).
The navigation bar displays:
Home | Catalog | Cart
This is the final step performed by the buyer to complete the purchase.
The page displays all purchase details, including the order status: **PAID**.

Purchase Result:
The completed purchase is confirmed on the Order Confirmed page at [http://localhost:8080/order/].
The corresponding records are saved in the following database tables:
- customers
- orders
- order_items
The purchased product quantities are also deducted from the stock recorded in the products table.

V. Description of the ADMIN Delivery Process – Shipping Products Purchased by Customers Through the Online Store
Participant: User with the ADMIN role.
ADMIN
↓
Opens [http://localhost:8080], where only the Login menu is displayed.
↓
Logs in at [http://localhost:8080/login].
↓
Returns to [http://localhost:8080/], now with the role-specific navigation menu:
Home | Admin Dashboard | Logout
A message confirms that the user is logged in as ADMIN.
↓
Opens Admin Dashboard from the navigation menu.
↓
The Administration Dashboard page opens at [http://localhost:8080/admin] (admin.html).
The dashboard contains functional sections, including Open Delivery.
↓
Opens the Open Delivery section.
↓
The Delivery Register page opens at [http://localhost:8080/admin/delivery] (admin-delivery.html).
The navigation bar displays:
Home | Admin Dashboard
By default, this page displays information about orders that customers have completed through the Order Confirmed checkout process.
Newly purchased orders have the following default values:
- TTN: Not assigned
- Status: New
The shipments database table has tracking_number = NULL for these orders.
For orders with the New status, the administrator clicks Open Delivery.
↓
Open Delivery
↓
The Delivery for Order ORD-* page opens at [http://localhost:8080/admin/delivery/*] (admin-delivery-details.html).
The navigation bar displays:
Home | Admin Dashboard
This page is used to prepare the purchased products for shipment via Nova Poshta.
Initially, the fields have the following default values:
- Status: Paid
- Shipment / Nova Poshta Status: New
- TTN / Tracking Number: Not created yet
The administrator clicks Create Nova Poshta Shipment (DEV).
↓
Create Nova Poshta Shipment (DEV)
↓
The same page (admin-delivery-details.html) is updated with the following values:
- Status: Shipped
- Shipment / Nova Poshta Status: Shipped
- TTN / Tracking Number: A specific automatically generated numeric value.
A confirmation message appears:
Nova Poshta DEV shipment successfully created.
The generated TTN is also displayed on the admin-delivery.html page.
The shipments database table is updated with the corresponding values in its relevant fields.
The administrator performs the final action by clicking Confirm Pickup / Complete Order.
↓
Confirm Pickup / Complete Order

Shipment Result:
The same page (admin-delivery-details.html) displays the following final status information:
- Status: Completed
- Nova Poshta Shipment Status: Completed
The following confirmation messages appear:
- Order successfully completed
- This order is completed
The shipments and orders database tables are also updated with the corresponding final values in their relevant fields.

```
### 4.2. Additional Information Modules

- **The following functional modules are currently included in the application structure but have not yet been fully implemented:**
- Promotion
- Review
- Product Q&A
- Availability
- Site Content / SEO

## 5. Application Access and Limitations

- The current MVP version is available for demonstration, and the main store functionality is accessible to all visitors.
- The current MVP operates with authentication, login credentials, passwords and user accounts. Customer order history is not implemented in the current MVP version.

## 6. Future Development

- The application is intended as an MVP for demonstration, evaluation, and further development.
- Other additional features and system modifications can be implemented according to future customer requirements.

## 7. Licensing

-   This software is proprietary.
-   The source code is closed and represents the intellectual property of the author.

## 8. Commercial Use

-   Source code and extended project versions are provided by agreement.

## 9. Contacts
 
-   **Author:** **Evgeniy Novak**
-   **Email:** **novakevgeniy1953@gmail.com**
-   **GitHub:** **https://github.com/NovakEvgeniy?tab=repositories**
















