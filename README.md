# 🧪 Shady Meadows B&B – API Automation (Postman)

## 📌 Overview

This project contains API test automation for the **Shady Meadows B&B** platform using **Postman**.

The tests validate core backend services including:

* Branding API
* Room Inventory API
* Booking Creation API

The collection is designed to be **data-driven, reusable, and executable via Postman or Newman (CLI)**.

---

## 🌐 Base URL

```bash
https://automationintesting.online
```

---

## 🧪 Test Coverage

### ✅ 1. Branding API

**Endpoint:**

```http
GET /branding/
```

**Validations:**

* Status code = 200
* Response is JSON
* `name` = `"Shady Meadows B&B"`
* `contact.email` matches valid email regex
* Required fields exist (name, description, contact)

---

### ✅ 2. Room Inventory API

**Endpoint:**

```http
GET /room/
```

**Validations:**

* Status code = 200
* Response is an array
* At least one room exists
* `roomPrice > 0`
* Capture `roomid` for booking API

---

### ✅ 3. Booking Creation API

**Endpoint:**

```http
POST /booking/
```

**Flow:**

1. Fetch `roomid` from Room API
2. Use dynamic dates (checkin & checkout)
3. Create booking

**Validations:**

* Status code = 201
* `bookingid` is generated
* Response matches request payload

---

### ❌ Negative Test Cases

* Missing `firstname`
* Invalid booking dates (checkout < checkin)

---

## 🔄 Dynamic Data Handling

* `roomid` captured from Room API:

```javascript
pm.collectionVariables.set("capturedRoomId", rooms[0].roomid);
```

* Dates generated dynamically in Pre-request Script:

```javascript
const today = new Date();
```

---

## 🚀 How to Run Tests

### ▶️ Using Postman

1. Import collection
2. Set environment variable:

```text
baseUrl = https://automationintesting.online
```

3. Click **Run Collection**

---

### ⚡ Using Newman (CLI)

```bash
npm install -g newman
```

Run:

```bash
newman run collection.json
```

---

## 📊 Reporting

Generate HTML report using Newman:

```bash
newman run collection.json -r html
```

---

## 🐞 Issues Identified

### 🔴 1. Incorrect API Endpoints

The following endpoints were initially incorrect:

| Wrong           | Correct      |
| --------------- | ------------ |
| `/api/branding` | `/branding/` |
| `/api/room`     | `/room/`     |
| `/api/booking`  | `/booking/`  |

---

### 🔴 2. Unnecessary Headers

* GET requests should not include:

  * `Content-Type`
  * Cookies
  * User-Agent

These caused:

* 400 Bad Request
* HTML responses instead of JSON

---

### 🔴 3. Incorrect Assertions

Some validations were mismatched:

* Email and name fields incorrectly asserted

---

## 🧠 Best Practices Followed

* Used **environment variables**
* Implemented **dynamic data handling**
* Maintained **test independence**
* Added **negative test scenarios**
* Used **regex validation for email**
* Avoided hardcoding IDs

---

## 🔄 CI/CD Integration

This collection can be integrated into pipelines using Newman:

### Example (GitHub Actions / Jenkins / Azure Devops):

* Install Newman
* Run collection
* Generate reports
* Fail build on test failures

---

## 🧰 Tools Used

* Postman
* Newman (CLI)
* JavaScript (Test Scripts)

---

## 🧠 Approach

* Followed API contract validation
* Focused on real-world test scenarios
* Ensured tests are reusable and maintainable
* Used dynamic chaining between APIs

---
