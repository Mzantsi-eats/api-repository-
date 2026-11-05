# The Mzantsi Table - Backend API

This repository contains the ASP.NET Core Web API backend for **The Mzantsi Table** - a digital living library dedicated to preserving and celebrating South African culinary heritage. 

The API is backed by **Firebase Authentication** and **Cloud Firestore**, serving as a secure, centralized data layer for the Android application and future web clients.

## Tech Stack

- **Framework:** ASP.NET Core Web API (.NET 8)
- **Language:** C# 12
- **Database:** Cloud Firestore (NoSQL)
- **Authentication:** Firebase Admin SDK (ID token verification)
- **Documentation:** Swagger / OpenAPI
- **Architecture:** Controllers → Services → Repositories

## Project Structure

```text
MzantsiTableApi/
├── Authentication/       # Firebase token verification middleware
├── Controllers/          # API endpoints (Recipes, Users)
├── Models/               # C# data models matching Firestore documents
├── Repositories/         # Firestore data access layer
├── Services/             # Business logic layer
├── Program.cs            # Application entry point & dependency injection
├── appsettings.json      # Configuration (ProjectId, credential paths)
└── MzantsiTableApi.sln   # Visual Studio solution file

Getting Started
Prerequisites
.NET 8 SDK

Visual Studio 2022 (with the ASP.NET and web development workload)

A Firebase project with Authentication and Firestore enabled

1. Clone the repository
bash
git clone https://github.com/Mzantsi-eats/api-repository-.git
cd api-repository-
2. Configure Firebase Credentials
This API uses the Firebase Admin SDK, which requires a service account key.

Go to the Firebase Console for your project.

Navigate to Project settings (gear icon) → Service accounts.

Click Generate new private key.

Rename the downloaded .json file to exactly firebase-service-account.json.

Place this file in the root of the MzantsiTableApi folder (next to Program.cs).

⚠️ SECURITY WARNING: The firebase-service-account.json file contains admin credentials that can read, write, and delete your entire database. Never commit this file to GitHub. It is already added to .gitignore.

3. Run the API
Open MzantsiTableApi.sln in Visual Studio.

Ensure appsettings.json contains the correct ProjectId for your Firebase project.

Press F5 to build and run the API.

Swagger UI will open automatically in your browser (e.g., at https://localhost:5081/swagger/index.html).

API Endpoints
The API is fully documented via Swagger. When running locally, you can test every endpoint directly from your browser. Protected endpoints require a valid Firebase ID token, which can be pasted into the Authorize dialog in Swagger.

Recipes
Method	Endpoint	Auth	Description
GET	/api/Recipes	Public	List all recipes (optional ?cuisine= filter)
GET	/api/Recipes/{id}	Public	Get a specific recipe
POST	/api/Recipes	🔒	Create a new recipe
POST	/api/Recipes/{id}/reviews	🔒	Submit a review for a recipe
Users
Method	Endpoint	Auth	Description
GET	/api/Users/me	🔒	Get the current user's profile
POST	/api/Users/me	🔒	Create a user profile on first login
PUT	/api/Users/me/settings	🔒	Update user settings
🔒 = Requires a valid Firebase ID token in the Authorization: Bearer <token> header.

Security Model
Authentication: All protected routes are guarded by a custom FirebaseAuthenticationMiddleware. It intercepts requests, verifies the Bearer token with the Firebase Admin SDK, and injects the user's UID into the request context.

Identity Enforcement: The API never trusts a user ID sent in the request body. The POST /api/Recipes endpoint extracts the user's UID directly from the verified Firebase token, ensuring users cannot impersonate others.

Database Access: The API writes to Firestore using the Admin SDK, bypassing client-side security rules. The Android app writes to Firestore directly using client-side rules. Both interact safely with the same database.

Deployment
This API is designed to be hosted on Microsoft Azure App Services (or any standard .NET hosting provider). Ensure the firebase-service-account.json is securely provided via environment variables or Azure Key Vault in production.

**Developed as part of the Open Source 6312 Portfolio of Evidence at Rosebank International University College.**
