## 🛠️ Setup Instructions for API_Testing_Project

### 1. **Clone the Repository**
```bash
git clone https://github.com/Tamanna871/API_Testing_Project.git
cd API_Testing_Project
```

### 2. **Start Mock Server**
```bash
json-server --watch students.json --port 3000
```
> This runs the server at `http://localhost:3000/students`

### 3. **Import Collection into Postman**
- Open Postman
- Import `Student_API_Test_Cases_work_flow.postman_collection.json`

### 4. **Run from Postman Interface**
- Select the collection → Click "Run"
- Click **Select File** and choose `students_id.csv`  
  _📌 Note: This file is only required for the **Get single student data** request_
- Ensure requests are ordered correctly in the runner
- Click **Run Student_API_Test_Cases_work_flow**

### 5. **Run from Command Line (Newman)**
- Open a terminal in the API_Testing_Project folder (the one you just cloned from GitHub), then run:
```bash
newman run Student_API_Test_Cases_work_flow.postman_collection.json --iteration-data students_id.csv
```

> ⚠️ The students_id.csv file is only used in the Get Single Student data request. However, all other requests in the collection will still execute and pass normally.


### 6. **Run Collection Remotely via URL**
- Open a terminal in the API_Testing_Project folder (the one you just cloned from GitHub), then run:
```bash
newman run "https://api.postman.com/collections/<collection-id>?access_key=<your-access-key>" --iteration-data students_id.csv
```


---

# About API Testing Project Overview with Postman & Newman

## 📁 Project Setup

First, I created a folder named `API_Testing_Project`. Inside that folder:

- `students.json` — to store the mock data for the JSON server.
- `students_id.csv` — for data-driven testing.

This is the environment where the API testing will be performed.

## ⚙️ Starting the JSON Server

Navigate to the location of the `students.json` file and run:

```bash
json-server students.json
```

---

## 📬 Postman Setup

I opened Postman and created a collection named **`Student_API_Test_Cases_work_flow`**.

### 🔹 1. Collection Structure

- **Base URL:** `http://localhost:3000/students` (set as a collection variable `{{baseUrl}}`)
- **Folder:** `New Folder` containing 5 API requests covering full CRUD operations

---

### 🔹 2. API Requests & Test Cases

#### a) Get single student data

- **Method:** GET  
- **Endpoint:** `{{baseUrl}}/{{id}}`  
- **Tests:**
  - Status code is 200
  - Validates `Content-Type` header is `application/json`
  - Response time is less than 200ms
  - Schema validation for fields: `id`, `name`, `location`, `phone`, `courses`
  - Validates data types
  - Specific assertions for `id = 1` (e.g., `name = "John"`, `courses = ["JAVA", "Selenium"]`)

#### b) Get all students data

- **Method:** GET  
- **Endpoint:** `{{baseUrl}}`  
- **Tests:**
  - Status code is 200
  - Validates `Content-Type`
  - Response time < 200ms
  - Schema validation for an array of student objects

#### c) Create new student

- **Method:** POST  
- **Endpoint:** `{{baseUrl}}`  
- **Pre-request Script:**
  - Generates a random username (e.g., `"Jim" + random string`)
- **Body:**

```json
{
  "name": "{{userName}}",
  "location": "France",
  "phone": "123456",
  "courses": ["C", "C++"]
}
```

- **Tests:**
  - Status code 201
  - Validates `Content-Type`
  - Response time < 200ms
  - Saves response ID as env variable `idFromResponse`
  - Schema validation

#### d) Update student

- **Method:** PUT  
- **Endpoint:** `{{baseUrl}}/{{idFromResponse}}`  
- **Body:**

```json
{
  "name": "Scott",
  "location": "Germany",
  "phone": "654321",
  "courses": ["C", "C++"]
}
```

- **Tests:**
  - Status code 200
  - Validates `Content-Type`
  - Response time < 200ms
  - Ensures response ID matches `idFromResponse`
  - Schema validation

#### e) Delete student

- **Method:** DELETE  
- **Endpoint:** `{{baseUrl}}/{{idFromResponse}}`  
- **Tests:**
  - Status code 200
  - Validates `Content-Type`
  - Response time < 200ms
  - Schema validation
  - Clears `idFromResponse` variable after deletion

---

### 🔹 3. Key Features

- **Dynamic Data:** Random usernames to avoid duplicate entries
- **Environment Variables:** `idFromResponse` used to chain operations (create → update → delete)
- **Schema Validation:** Used `tv4` library
- **Performance Testing:** Ensures response time < 200ms
- **Comprehensive Assertions:** Headers, body, schema

---

### 🔹 4. Running Collection in Postman

- Click collection → Run
- Select `students_id.csv` for data-driven test on **Get single student data**
- Click **Run Student_API_Test_Cases_work_flow**

#### After Running:

- Export results as `.json`:  
  `Student_API_Test_Cases_work_flow.postman_test_run.json`
- Export collection:  
  `Student_API_Test_Cases_work_flow.postman_collection.json`

---

## 🧪 Running with Newman (Command Line)

### 🔹 5. Run Collection from Command Line

```bash
newman run Student_API_Test_Cases_work_flow.postman_collection.json --iteration-data students_id.csv
```

> Only **"Get single student data"** uses the CSV. Others run normally.

---

### 🔹 6. Generate HTML Report (Local Collection)

```bash
newman run Student_API_Test_Cases_work_flow.postman_collection.json --iteration-data students_id.csv -r html
```

- HTML report saved inside `newman` folder

---

### 🔹 7. Run Collection from Remote URL

#### URL:

```bash
https://api.postman.com/collections/<collection-id>?access_key=<your-access-key>
```

#### Run from terminal:

```bash
newman run "https://api.postman.com/collections/<collection-id>?access_key=<your-access-key>" --iteration-data students_id.csv
```

#### Generate HTML Report:

```bash
newman run "https://api.postman.com/collections/<collection-id>?access_key=<your-access-key>" --iteration-data students_id.csv -r html
```

> Saved in the `newman` folder

---

### 🔹 8. Publish Postman Documentation

1. Click collection → View complete documentation
2. Select `curl` format
3. Click **Publish** → Confirm again

#### 📄 Published Documentation:

[https://documenter.getpostman.com/view/40840475/2sB2ca7zqs](https://documenter.getpostman.com/view/40840475/2sB2ca7zqs)

Anyone with the URL can:
- View full docs
- Use **Run in Postman**
- See all scripts

## 🙋‍♀️ Author

**Tamanna Kawser Chowdhury**  
[GitHub Profile](https://github.com/Tamanna871)
