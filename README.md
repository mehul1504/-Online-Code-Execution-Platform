# -Online-Code-Execution-Platform
## 1. Project: CloudCode

CloudCode is an online code execution platform designed to provide users with a secure and efficient way to write, test, and execute code in various programming languages without the need for a local development environment.

## 2. System Design

### Architecture Components

1. User Interface: Web-based frontend for users to interact with the system.
2. Authentication Service: Handles user login and session management.
3. Authorization Module: Manages permissions and access control.
4. Code Executor: Executes user-submitted code snippets.
5. Resource Manager: Allocates and manages resources for code execution.
6. Database: Stores user data, code snippets, and execution results.

### Key Interactions

- User Interface communicates with Authentication Service for login and session management.
- Authentication Service interacts with Authorization Module for permission checks.
- Code Executor works with Resource Manager to allocate and manage resources.
- All components interact with the Database for storing and retrieving data.

## 3. Database Design

### Tables

1. Users
   - id (PK): UUID
   - username: VARCHAR(50)
   - email: VARCHAR(100)
   - password_hash: VARCHAR(255)
   - created_at: TIMESTAMP

2. Languages
   - id (PK): UUID
   - name: VARCHAR(20)
   - description: TEXT

3. Code_Snippets
   - id (PK): UUID
   - title: VARCHAR(100)
   - code: TEXT
   - language_id: FK (Languages.id)
   - user_id: FK (Users.id)
   - created_at: TIMESTAMP

4. Execution_Results
   - id (PK): UUID
   - snippet_id: FK (Code_Snippets.id)
   - output: TEXT
   - status: ENUM ('success', 'error')
   - created_at: TIMESTAMP

## 4. Key APIs

### 1. Submit Code Snippet

Endpoint: `/api/code`
Method: POST
Request Body:
```json
{
  "title": "Example Function",
  "code": "def hello_world():\n    print('Hello, World!')\n"
}
```
Response:
```json
{
  "id": "123e4567-e89b-12d3-a456-426614174000",
  "title": "Example Function",
  "language_id": "abc1234-def5-6789-ijk0-lmnopqrstu"
}
```

### 2. Execute Code Snippet

Endpoint: `/api/code/{snippet_id}/execute`
Method: POST
Request Body:
```json
{
  "timeout": 5000,
  "memory_limit": 128
}
```
Response:
```json
{
  "id": "234e5678-90cd-12de-f345-426614175001",
  "output": "Hello, World!\n",
  "status": "success"
}
```

### 3. Retrieve Execution Results

Endpoint: `/api/results/{result_id}`
Method: GET
Response:
```json
{
  "id": "345e6789-0def-12ef-g456-426614176002",
  "snippet_id": "123e4567-e89b-12d3-a456-426614174000",
  "output": "Hello, World!\n",
  "status": "success",
  "created_at": "2023-09-17T10:30:00Z"
}
```

## 5. Overall Approach

### Technologies Used

- Backend: Spring Boot for RESTful API design and containerization with Docker
- Database: PostgreSQL for robust data storage and management
- Authentication: JWT for secure token-based authentication
- Authorization: Role-Based Access Control (RBAC) for fine-grained permissions
- Language Support: Implementation of virtual machines for Python, Java, JavaScript, and C++
- Security Measures: Input validation, output sanitization, and rate limiting
- Resource Management: Memory allocation and timeout handling

