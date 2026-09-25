# AI FitTrack API 🏋️‍♂️🤖

AI FitTrack API is a fitness tracking backend application built using **Node.js, Express.js, MongoDB, JWT Authentication, bcrypt, and Google Gemini AI**.

It enables users to securely register and authenticate, manage their workout activities, search workout history, and receive personalized AI-powered workout recommendations and fitness insights.

---

## 🚀 Features

* 🔐 Secure user registration and login
* 🔑 JWT-based authentication
* 🔒 Password hashing with bcrypt
* 🏃 Workout management
* 🔍 Workout search by keyword and date
* 👤 User-specific workout data
* 🤖 AI-powered workout recommendations
* 📊 AI-powered fitness progress insights
* 🗄️ MongoDB Atlas database integration
* 🛡️ Protected API routes
* 📝 Request logging with Morgan
* ⚡ Development support with Nodemon

---

## 🎨 Tech Stack

| Technology               | Purpose                         |
| ------------------------ | ------------------------------- |
| **Node.js**              | Runtime environment             |
| **Express.js**           | REST API framework              |
| **MongoDB Atlas**        | Cloud database                  |
| **Mongoose**             | MongoDB ODM                     |
| **JWT (JSON Web Token)** | Authentication                  |
| **bcryptjs**             | Password hashing                |
| **Google Gemini AI**     | AI recommendations and insights |
| **@google/genai**        | Gemini API SDK                  |
| **Morgan**               | HTTP request logging            |
| **Nodemon**              | Development server              |

---

## 📁 Project Structure

```text
AI-FitTrack/
│
├── src/
│   ├── config/
│   │   └── db.js
│   │
│   ├── controllers/
│   │   ├── authController.js
│   │   ├── workoutController.js
│   │   └── aiController.js
│   │
│   ├── middleware/
│   │   ├── auth.js
│   │   └── errorHandler.js
│   │
│   ├── models/
│   │   ├── User.js
│   │   └── Workout.js
│   │
│   ├── routes/
│   │   ├── authRoutes.js
│   │   ├── workoutRoutes.js
│   │   ├── aiRoutes.js
│   │   └── index.js
│   │
│   ├── services/
│   │   └── geminiService.js
│   │
│   └── app.js / server.js
│
├── .env
├── .env.example
├── .gitignore
├── package.json
├── package-lock.json
├── FitTrack.postman_collection.json
└── README.md
```

> **Note:** The exact entry file may be `app.js` or `server.js` depending on the project version.

---

# 🚀 Getting Started

## 1. Prerequisites

Make sure you have the following installed:

* **Node.js** (v20 or higher recommended)
* **MongoDB Atlas** or a local MongoDB server
* **Google Gemini API Key**
* Git

---

## 2. Clone the Repository

```bash
git clone https://github.com/YOUR-USERNAME/AI-FitTrack.git
```

Navigate into the project:

```bash
cd AI-FitTrack
```

---

## 3. Install Dependencies

```bash
npm install
```

---

## 4. Configure Environment Variables

Create a `.env` file in the project root.

```env
PORT=5000

MONGO_URI=your_mongodb_connection_string

JWT_SECRET=your_jwt_secret_key

GEMINI_API_KEY=your_google_gemini_api_key

GEMINI_MODEL=gemini-3.5-flash
```

### ⚠️ Security

Never upload your `.env` file to GitHub.

Make sure `.gitignore` contains:

```gitignore
node_modules/
.env
```

You can provide a safe `.env.example` file:

```env
PORT=5000
MONGO_URI=your_mongodb_connection_string
JWT_SECRET=your_jwt_secret_key
GEMINI_API_KEY=your_google_gemini_api_key
GEMINI_MODEL=gemini-3.5-flash
```

---

# ▶️ Running the Server

## Development Mode

```bash
npm run dev
```

The API will run at:

```text
http://localhost:5000
```

## Production Mode

```bash
npm start
```

---

# 🩺 API Health Check

### GET

```text
/
```

Example response:

```json
{
  "success": true,
  "message": "AI FitTrack API is running successfully",
  "version": "1.0.0"
}
```

---

# 🔐 Authentication

## Register User

### POST

```text
/api/auth/register
```

**Authentication:** Not required

### Request Body

```json
{
  "name": "John Doe",
  "email": "john@gmail.com",
  "password": "123456"
}
```

### Response

```json
{
  "success": true,
  "token": "JWT_TOKEN",
  "user": {
    "id": "USER_ID",
    "name": "John Doe",
    "email": "john@gmail.com"
  }
}
```

---

## Login User

### POST

```text
/api/auth/login
```

**Authentication:** Not required

### Request Body

```json
{
  "email": "john@gmail.com",
  "password": "123456"
}
```

### Response

```json
{
  "success": true,
  "token": "JWT_TOKEN",
  "user": {
    "id": "USER_ID",
    "name": "John Doe",
    "email": "john@gmail.com"
  }
}
```

---

## Get User Profile

### GET

```text
/api/auth/profile
```

**Authentication:** Required

Header:

```text
Authorization: Bearer YOUR_JWT_TOKEN
```

### Response

```json
{
  "success": true,
  "user": {
    "id": "USER_ID",
    "name": "John Doe",
    "email": "john@gmail.com"
  }
}
```

---

# 🏃 Workout Management

All workout endpoints require JWT authentication.

Header:

```text
Authorization: Bearer YOUR_JWT_TOKEN
```

## Add Workout

### POST

```text
/api/workouts
```

### Request Body

```json
{
  "workoutName": "Evening Jog",
  "category": "Running",
  "duration": 30,
  "caloriesBurned": 350
}
```

Optional workout date:

```json
{
  "workoutName": "Evening Jog",
  "category": "Running",
  "duration": 30,
  "caloriesBurned": 350,
  "workoutDate": "2026-06-26T18:00:00.000Z"
}
```

If `workoutDate` is not provided, the current timestamp is used.

---

## View All Workouts

### GET

```text
/api/workouts
```

Returns workouts belonging to the authenticated user.

---

## View Workout by ID

### GET

```text
/api/workouts/:id
```

Example:

```text
/api/workouts/WORKOUT_ID
```

---

## Update Workout

### PUT

```text
/api/workouts/:id
```

Example request body:

```json
{
  "workoutName": "Morning Running - Updated",
  "category": "Running",
  "duration": 45,
  "caloriesBurned": 400
}
```

---

## Delete Workout

### DELETE

```text
/api/workouts/:id
```

---

# 🔍 Workout Search

## Search Workouts

### GET

```text
/api/workouts/search?q=Running
```

### Query Parameter

```text
q
```

The search endpoint supports searching workout information such as:

* Workout name
* Workout category
* Date

### Example: Search by Workout

```text
/api/workouts/search?q=Running
```

### Example: Search by Date

```text
/api/workouts/search?q=2026-09-24
```

---

# 🤖 AI Workout Recommendation

AI FitTrack uses **Google Gemini AI** to generate personalized workout recommendations.

## Endpoint

### POST

```text
/api/ai/workout-recommendation
```

**Authentication:** Required

### Request Body

```json
{
  "age": 21,
  "fitnessGoal": "Weight Loss",
  "experience": "Beginner"
}
```

### Response

```json
{
  "recommendation": "To lose weight effectively as a beginner..."
}
```

The recommendation is generated dynamically by Google Gemini based on the user's information.

---

# 📊 AI Fitness Insights

The API can also generate AI-powered insights from workout statistics.

## Endpoint

### POST

```text
/api/ai/fitness-insights
```

**Authentication:** Required

### Request Body

```json
{
  "totalWorkouts": 10,
  "averageDuration": 45,
  "totalCaloriesBurned": 2500
}
```

### Response

```json
{
  "insight": "Logging 10 workouts at a steady 45 minutes shows fantastic consistency..."
}
```

---

# 🔑 Authentication Flow

```text
User
 │
 ▼
Register / Login
 │
 ▼
JWT Token
 │
 ▼
Authorization Header
 │
 ▼
Authentication Middleware
 │
 ▼
Protected Route
 │
 ▼
Controller
 │
 ▼
Response
```

---

# 🤖 AI Integration Flow

```text
User Fitness Data
       │
       ▼
AI API Endpoint
       │
       ▼
AI Controller
       │
       ▼
Gemini Service
       │
       ▼
Google Gemini API
       │
       ▼
AI Generated Response
       │
       ▼
JSON Response
```

---

# 🧪 API Testing

The API was tested successfully using **Thunder Client in Visual Studio Code**.

### Tested Features

* ✅ API health check
* ✅ User registration
* ✅ User login
* ✅ JWT authentication
* ✅ User profile
* ✅ Create workout
* ✅ Get all workouts
* ✅ Get workout by ID
* ✅ Update workout
* ✅ Delete workout
* ✅ Workout keyword search
* ✅ Workout date search
* ✅ AI workout recommendation
* ✅ AI fitness insights
* ✅ Google Gemini API integration

### AI Integration Test

Both AI endpoints successfully returned:

```text
HTTP 200 OK
```

using:

```text
gemini-3.5-flash
```

---

# 📦 Postman Collection

An importable Postman collection is included in the project:

```text
FitTrack.postman_collection.json
```

The collection contains predefined API requests for testing the backend.

> The project was also tested using Thunder Client in Visual Studio Code.

---

# 🔒 Security

AI FitTrack follows several basic security practices:

* Passwords are hashed using bcrypt
* JWT is used for authentication
* Protected routes require a valid JWT
* Users can only access their own workout records
* Environment variables are used for secrets
* API keys and database credentials should never be committed to GitHub

---

# 🔮 Future Improvements

* 🌐 Web frontend dashboard
* 📱 Mobile application
* 📊 Fitness analytics and charts
* 🔥 Workout streak tracking
* 🎯 Personalized fitness goals
* 🥗 AI-powered nutrition recommendations
* 🔔 Workout reminders
* 🏆 Fitness achievements and challenges
* 📈 Long-term progress tracking
* ☁️ Cloud deployment
* 🧪 Automated API testing
* 📚 Swagger/OpenAPI documentation

---

# 👨‍💻 Developer

**Munieswaran U**

BCA Student
Aspiring Software Developer😎

---

# 📄 License

This project was developed for educational and academic purposes.
