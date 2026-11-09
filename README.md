# 🚗 BroCar – Vehicle Rental Platform

[![PHP](https://img.shields.io/badge/PHP-7.4%2B-777BB4?style=for-the-badge&logo=php&logoColor=white)](https://www.php.net/)
[![MySQL](https://img.shields.io/badge/MySQL-8.0%2B-4479A1?style=for-the-badge&logo=mysql&logoColor=white)](https://www.mysql.com/)
[![JavaScript](https://img.shields.io/badge/JavaScript-ES6%2B-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
[![eSewa](https://img.shields.io/badge/Payment-eSewa_API-60BB46?style=for-the-badge)](https://esewa.com.np/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](LICENSE)

An end-to-end full-stack vehicle rental management portal designed for seamless online vehicle reservations, real-time fleet availability management, secure session-based user authentication, and automated local payment processing via the eSewa API.

---
## 🖼️ Application Screenshots

<p align="center">
  <img src="screenshots/Screenshot (160).png" width="45%" alt="Screenshot 160" />
  <img src="screenshots/Screenshot (161).png" width="45%" alt="Screenshot 161" />
</p>

<p align="center">
  <img src="screenshots/Screenshot (162).png" width="45%" alt="Screenshot 162" />
  <img src="screenshots/Screenshot (163).png" width="45%" alt="Screenshot 163" />
</p>

<p align="center">
  <img src="screenshots/Screenshot (164).png" width="45%" alt="Screenshot 164" />
  <img src="screenshots/Screenshot (165).png" width="45%" alt="Screenshot 165" />
</p>

<p align="center">
  <img src="screenshots/Screenshot (166).png" width="45%" alt="Screenshot 166" />
  <img src="screenshots/Screenshot (167).png" width="45%" alt="Screenshot 167" />
</p>

<p align="center">
  <img src="screenshots/Screenshot (168).png" width="45%" alt="Screenshot 168" />
  <img src="screenshots/Screenshot (169).png" width="45%" alt="Screenshot 169" />
</p>

<p align="center">
  <img src="screenshots/Screenshot (170).png" width="45%" alt="Screenshot 170" />
  <img src="screenshots/Screenshot (171).png" width="45%" alt="Screenshot 171" />
</p>

<p align="center">
  <img src="screenshots/Screenshot (172).png" width="45%" alt="Screenshot 172" />
  <img src="screenshots/Screenshot (173).png" width="45%" alt="Screenshot 173" />
</p>

<p align="center">
  <img src="screenshots/Screenshot (174).png" width="45%" alt="Screenshot 174" />
  <img src="screenshots/Screenshot (175).png" width="45%" alt="Screenshot 175" />
</p>

<p align="center">
  <img src="screenshots/Screenshot (176).png" width="91%" alt="Screenshot 176" />
</p>
## 📌 Features

### User Facing Portal
* **Interactive Fleet Catalog:** Browse available cars and bikes with detailed specs, pricing per day, and transmission parameters.
* **Reservation Engine:** Select start and end dates with real-time automatic rental cost computation.
* **User Authentication:** Secure user registration, password hashing (`password_hash`), and session handling.
* **Digital Payment Integration:** Automated online transaction verification powered by the **eSewa Payment Gateway API**.
* **Booking Dashboard:** Track past and ongoing rental statuses (Pending, Approved, Completed, Cancelled).

### Admin Management Panel
* **Fleet Control:** Add, edit, toggle availability status, or remove vehicles from the active catalog.
* **Reservation Oversight:** Review incoming rental requests, verify payment verification codes, and update booking statuses.
* **Customer Management:** View registered user accounts and contact records.

---

## 🛠️ Technology Stack

* **Frontend:** HTML5, CSS3, Modern JavaScript (ES6+), FontAwesome Icons
* **Backend:** PHP (Procedural / Object-Oriented Architecture with PDO/MySQLi)
* **Database:** MySQL relational database with relational integrity constraints
* **Payment Gateway:** eSewa ePay Integration API
* **Environment:** Compatible with Apache / NGINX local stacks (XAMPP, WAMP, or LAMP)

---

## 🏗️ System Architecture & Database Structure

The database handles relational mapping across users, vehicle fleets, bookings, and financial transaction records:


```

+------------------+         +--------------------+         +------------------+
|      USERS       |         |      BOOKINGS      |         |     VEHICLES     |
+------------------+         +--------------------+         +------------------+
| user_id (PK)     | <------ | booking_id (PK)    | ------> | vehicle_id (PK)  |
| full_name        |         | user_id (FK)       |         | title / brand    |
| email            |         | vehicle_id (FK)    |         | daily_rate       |
| password_hash    |         | start_date         |         | status           |
| role (user/admin)|         | end_date           |         | image_url        |
+------------------+         | total_cost         |         +------------------+
| payment_status     |
| esewa_ref_id       |
+--------------------+

```

---

## 🚀 Getting Started

Follow these steps to set up the project locally on your machine using **XAMPP** or a similar local server stack.

### Prerequisites
* PHP >= 7.4 installed
* MySQL / MariaDB Server running
* Web Server (Apache via XAMPP/WAMP)
* Composer (optional, if using external libraries)

---

### Installation Steps

1. **Clone the Repository**
   ```bash
   git clone [https://github.com/nikunjpanthi5/brocar_rental.git](https://github.com/nikunjpanthi5/brocar_rental.git)
   cd brocar_rental

```

2. **Move to Web Directory**
Move the cloned project folder into your local web server root (e.g., `C:\xampp\htdocs\brocar_rental` for XAMPP).
3. **Database Configuration**
* Open **phpMyAdmin** (`http://localhost/phpmyadmin`).
* Create a new database named `brocar_db`.
* Import the database schema from the `database/schema.sql` file provided in this repository.


4. **Configure Database Connection**
Open `config/db.php` (or your database configuration file) and update your database credentials:
```php
<?php
define('DB_HOST', 'localhost');
define('DB_USER', 'root');
define('DB_PASS', '');
define('DB_NAME', 'brocar_db');

$conn = new mysqli(DB_HOST, DB_USER, DB_PASS, DB_NAME);

if ($conn->connect_error) {
    die("Database Connection Failed: " . $conn->connect_error);
}
?>

```


5. **Configure eSewa Credentials**
Update your eSewa merchant code and environment URLs in `config/esewa.php` (use test credentials for local testing):
```php
// eSewa Test Merchant Config
$merchant_code = "EPAYTEST";
$esewa_url = "[https://uat.esewa.com.np/epay/main](https://uat.esewa.com.np/epay/main)";

```


6. **Launch Application**
Open your browser and navigate to:
```
http://localhost/brocar_rental/index.php

```



---

## 🔐 eSewa Payment Workflow

1. User selects a vehicle and rental duration.
2. The application computes `total_amount` dynamically via JavaScript.
3. Upon confirming booking, the system creates a pending record and redirects to eSewa's gateway.
4. After payment completion, eSewa redirects back to `payment_verify.php` with response parameters (`oid`, `amt`, `refId`).
5. Server verifies transaction validity directly with eSewa servers before marking the reservation as **Paid**.

---

## 👨‍💻 Developer & Author

**Nikunja Panthi (Nikunj)**

* **Portfolio:** [nikunjapanthi.com.np](https://nikunjapanthi.com.np)
* **GitHub:** [@nikunjpanthi5](https://github.com/nikunjpanthi5)
* **LinkedIn:** [Nikunja Panthi](https://linkedin.com/in/nikunja-panthi-47107a294)
* **Location:** Pokhara, Nepal

---

## 📄 License

This project is open-source and available under the [MIT License](https://www.google.com/search?q=LICENSE).

```

```
