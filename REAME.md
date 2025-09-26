# Authentication Labs Report

This report contains the implementation and testing of four different authentication mechanisms in Node.js.

---

## **1. simple_auth**

### **a. Basic Authentication**
- **Run server**
  ```bash
  node basic_auth.js
  ```
- **Test with Postman**
  - Endpoint: `GET /secure`
  - Must add `Authorization` header with value:
    ```
    Basic base64(username:password)
    ```
  - Example: `admin:12345` → `YWRtaW46MTIzNDU=`

Expected result:
- If credentials correct → return protected resource.
- If missing or wrong → return error.

---

## **2. cookie_auth**

### **a. Run server**
```bash
node cookie_auth.js
```

Server runs at: `http://localhost:3001`

### **b. Login**
- Endpoint: `POST /login`
- Body:
```json
{
  "username": "admin",
  "password": "12345"
}
```
- Sets a cookie `auth_cookie_token` and stores it in MongoDB.

### **c. Profile**
- Endpoint: `GET /profile`
- Requires cookie `auth_cookie_token`.
- Returns user info if valid.

### **d. Logout**
- Endpoint: `POST /logout`
- Deletes cookie from client and removes session from MongoDB.

---

## **3. cookie_session_auth**

### **a. Run server**
```bash
node app.js
```

Server runs at: `http://localhost:3000`

### **b. Register**
- Endpoint: `POST /auth/register`
- Body:
```json
{
  "username": "admin",
  "password": "12345"
}
```
- Creates a new user in MongoDB.

### **c. Login**
- Endpoint: `POST /auth/login`
- Body: same as register.
- On success → session cookie (`connect.sid`) created.

### **d. Profile**
- Endpoint: `GET /auth/profile`
- Requires session cookie.
- Returns user info (without password).

### **e. Logout**
- Endpoint: `GET /auth/logout`
- Destroys session and clears cookie.

---

## **4. local_passport_website**

A website using Passport.js Local Strategy + MongoDB + EJS templates.

### **a. Run server**
```bash
node app.js
```
Server runs at: `http://localhost:3000`

### **b. Register**
- URL: `http://localhost:3000/register`
- Fill form with username & password.
- User is saved to MongoDB with hashed password.

### **c. Login**
- URL: `http://localhost:3000/login`
- Enter credentials → if correct → redirect to profile.

### **d. Profile**
- URL: `http://localhost:3000/profile`
- Shows:
  ```
  Welcome <username>
  Logout
  ```
- If not authenticated → redirected to login.

### **e. Logout**
- Click Logout link → session destroyed → redirected to login.

---

## **Notes (common for all labs)**
- **Basic Auth**: stateless, credentials sent in every request.
- **Cookie Auth**: cookie token stored in DB + browser.
- **Session Auth**: session stored in MongoDB, client only holds `connect.sid`.
- **Passport Local**: higher-level library, integrates with sessions, simplifies login & logout flows.
- Passwords always stored as **bcrypt hash** for security.
