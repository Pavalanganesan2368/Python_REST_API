# 🐍 Basic Python REST API using Flask

A simple **REST API built with Python and Flask** that demonstrates how to create API endpoints and perform basic **CRUD (Create, Read, Update, Delete)** operations.

This project is suitable for beginners who want to understand how REST APIs work before moving into frameworks such as Django REST Framework or FastAPI.

---

## 📌 Features

* ✅ Create a new user
* ✅ Get all users
* ✅ Get a user by ID
* ✅ Update a user
* ✅ Delete a user
* ✅ JSON request and response handling
* ✅ HTTP methods: `GET`, `POST`, `PUT`, `DELETE`
* ✅ Simple Flask project structure

---

## 🛠️ Technologies Used

* **Python 3**
* **Flask**
* **REST API**
* **JSON**
* **Postman** – for API testing
* **Git & GitHub** – for version control

---

## 📂 Project Structure

```text
flask-rest-api/
│
├── api.py
├── requirements.txt
├── README.md
└── .gitignore
```

---

## ⚙️ Prerequisites

Before running this project, make sure you have:

* Python 3.x installed
* Git installed
* A code editor such as VS Code
* Postman or another API testing tool

Check Python installation:

```bash
python --version
```

Check Git installation:

```bash
git --version
```

---

## 🚀 Installation

### 1. Clone the Repository

```bash
git clone https://github.com/your-username/flask-rest-api.git
```

Move into the project directory:

```bash
cd flask-rest-api
```

---

### 2. Create a Virtual Environment

Creating a virtual environment is recommended to keep project dependencies isolated.

```bash
python -m venv venv
```

### Windows

```bash
venv\Scripts\activate
```

### macOS / Linux

```bash
source venv/bin/activate
```

---

### 3. Install Flask

```bash
pip install flask
```

---

### 4. Create `requirements.txt`

Generate the dependency file using:

```bash
pip freeze > requirements.txt
```

Example:

```text
Flask==3.x.x
```

Install dependencies later using:

```bash
pip install -r requirements.txt
```

---

# 💻 Application Code

Create a file named:

```text
app.py
```

Add the following code:

```python
from flask import Flask, jsonify, request

app = Flask(__name__)

users = [
    {
        "id": 1,
        "name": "John",
        "email": "john@example.com"
    },
    {
        "id": 2,
        "name": "Alice",
        "email": "alice@example.com"
    }
]


# GET - Get all users
@app.route("/api/users", methods=["GET"])
def get_users():
    return jsonify(users)


# GET - Get user by ID
@app.route("/api/users/<int:user_id>", methods=["GET"])
def get_user(user_id):

    user = next(
        (user for user in users if user["id"] == user_id),
        None
    )

    if user is None:
        return jsonify({"message": "User not found"}), 404

    return jsonify(user)


# POST - Create a new user
@app.route("/api/users", methods=["POST"])
def create_user():

    data = request.get_json()

    if not data or "name" not in data or "email" not in data:
        return jsonify({
            "message": "Name and email are required"
        }), 400

    new_user = {
        "id": len(users) + 1,
        "name": data["name"],
        "email": data["email"]
    }

    users.append(new_user)

    return jsonify(new_user), 201


# PUT - Update an existing user
@app.route("/api/users/<int:user_id>", methods=["PUT"])
def update_user(user_id):

    user = next(
        (user for user in users if user["id"] == user_id),
        None
    )

    if user is None:
        return jsonify({"message": "User not found"}), 404

    data = request.get_json()

    user["name"] = data.get("name", user["name"])
    user["email"] = data.get("email", user["email"])

    return jsonify(user)


# DELETE - Delete a user
@app.route("/api/users/<int:user_id>", methods=["DELETE"])
def delete_user(user_id):

    user = next(
        (user for user in users if user["id"] == user_id),
        None
    )

    if user is None:
        return jsonify({"message": "User not found"}), 404

    users.remove(user)

    return jsonify({
        "message": "User deleted successfully"
    })


if __name__ == "__main__":
    app.run(debug=True)
```

---

# ▶️ Running the Application

Start the Flask development server:

```bash
python app.py
```

The API will run at:

```text
http://127.0.0.1:5000
```

You should see something similar to:

```text
Running on http://127.0.0.1:5000
```

---

# 🔗 API Endpoints

| Method | Endpoint          | Description       |
| ------ | ----------------- | ----------------- |
| GET    | `/api/users`      | Get all users     |
| GET    | `/api/users/<id>` | Get user by ID    |
| POST   | `/api/users`      | Create a new user |
| PUT    | `/api/users/<id>` | Update a user     |
| DELETE | `/api/users/<id>` | Delete a user     |

---

# 🧪 Testing the API

You can test the API using **Postman**, Thunder Client, or `curl`.

---

## 1. Get All Users

### Request

```http
GET http://127.0.0.1:5000/api/users
```

### Response

```json
[
    {
        "id": 1,
        "name": "John",
        "email": "john@example.com"
    },
    {
        "id": 2,
        "name": "Alice",
        "email": "alice@example.com"
    }
]
```

---

## 2. Get User by ID

### Request

```http
GET http://127.0.0.1:5000/api/users/1
```

### Response

```json
{
    "id": 1,
    "name": "John",
    "email": "john@example.com"
}
```

---

## 3. Create a User

### Request

```http
POST http://127.0.0.1:5000/api/users
```

### Body

Select:

```text
Body → raw → JSON
```

Send:

```json
{
    "name": "David",
    "email": "david@example.com"
}
```

### Response

```json
{
    "id": 3,
    "name": "David",
    "email": "david@example.com"
}
```

---

## 4. Update a User

### Request

```http
PUT http://127.0.0.1:5000/api/users/1
```

### Body

```json
{
    "name": "John Updated",
    "email": "johnupdated@example.com"
}
```

### Response

```json
{
    "id": 1,
    "name": "John Updated",
    "email": "johnupdated@example.com"
}
```

---

## 5. Delete a User

### Request

```http
DELETE http://127.0.0.1:5000/api/users/1
```

### Response

```json
{
    "message": "User deleted successfully"
}
```

---

# 📡 HTTP Methods Used

### GET

Used to retrieve data.

```text
GET /api/users
```

### POST

Used to create new data.

```text
POST /api/users
```

### PUT

Used to update existing data.

```text
PUT /api/users/1
```

### DELETE

Used to remove data.

```text
DELETE /api/users/1
```

---

# 📊 HTTP Status Codes

| Status Code | Meaning               |
| ----------- | --------------------- |
| `200`       | Request successful    |
| `201`       | Resource created      |
| `400`       | Bad request           |
| `404`       | Resource not found    |
| `500`       | Internal server error |

---

# 🧠 Concepts Learned

This project helps understand:

* Python Flask basics
* REST API architecture
* Flask routing
* HTTP methods
* JSON data
* Request handling
* Response handling
* URL parameters
* CRUD operations
* HTTP status codes
* Virtual environments
* API testing with Postman
* Git and GitHub

---

# 🔄 REST API Flow

```text
Client
   │
   │ HTTP Request
   ▼
Flask REST API
   │
   ├── GET
   ├── POST
   ├── PUT
   └── DELETE
   │
   ▼
Data
   │
   │ JSON Response
   ▼
Client
```

---

# 📦 Future Improvements

The current project stores data in a Python list. The following improvements can be made:

* [ ] Add SQLite/MySQL/MongoDB database
* [ ] Add SQLAlchemy
* [ ] Add user authentication
* [ ] Add JWT authentication
* [ ] Add password hashing
* [ ] Add input validation
* [ ] Add error handling
* [ ] Add pagination
* [ ] Add search and filtering
* [ ] Add API documentation using Swagger/OpenAPI
* [ ] Deploy the API online
* [ ] Connect the API with a React frontend

---

# 🚀 GitHub Upload

Initialize Git:

```bash
git init
```

Add all files:

```bash
git add .
```

Commit the project:

```bash
git commit -m "Create basic Flask REST API"
```

Add your GitHub repository:

```bash
git remote add origin https://github.com/your-username/flask-rest-api.git
```

Push the project:

```bash
git branch -M main
git push -u origin main
```

---

# 🔐 `.gitignore`

Create a `.gitignore` file and add:

```text
venv/
__pycache__/
*.pyc
.env
```

This prevents unnecessary or sensitive files from being uploaded to GitHub.

---

# 👨‍💻 Author

**Pavalan Ganesan**

GitHub: `https://github.com/your-username`

---

# ⭐ Conclusion

This project provides a basic introduction to building REST APIs using **Python and Flask**. After understanding this project, the next step is to connect Flask with a database and build a complete backend application with authentication and a frontend such as React.
