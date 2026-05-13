# Project: Fashion Rental System (LuxeyFRS)

## Project Summary

This Fashion Rental System (LuxeyFRS) allows users to browse, rent, and manage apparel and fashion accessories within an inventory. Both customers and admins have defined roles, but both can rent and manage their own rental records. The system includes cloth ratings, user authentication, and CRUD functionality, and will be hosted on a cloud.

## Wireframe

## 1. Project Requirements

### 1.1 Functional Requirements

1. **User Roles:**
   - **Admin**: Full CRUD permissions on items and users, the ability to view all renting history, and also the ability to rent for themselves as customers do.
   - **Customers:** Can rent/return clothes, view their rental history, and rate clothes upon return.

2. **Wardrobe Management:**
   - Admins can add, edit, delete, and search items in the business database.
   - Both customers and admins can view item details such as title, brand, item number, type, availability, and average rating.

3. **User Authentication:**
   - Use JWT-based token authentication.
   - Admin and Customer roles should have different permissions, particularly in items and user management.

4. **Rental System:**
   - Both admins and customers can rent available items.
   - Users will receive due dates, and they can rate the item upon return.
   - Admins can view the renting history of all users, while customers can view only their own.

5. **Review Feature:**
   - Users can review and rate items after returning them.
   - Rating will be done on a 1-5 star scale.
   - Item ratings will be averaged and displayed alongside item details.

6. **Reports & History:**
   - Admins can view reports and analytics, including item popularity and overall renting activity.
   - History logs for each user’s renting and rating activities.

### 1.2 Non-Functional Requirements

- **Scalability:** Designed to handle simultaneous users.
- **Security:** Robust handling of sensitive data, such as password encryption and token storage.
- **Request validation:** Validate and sanitize incoming data to prevent invalid entries and reduce security risks.
- **User-Friendly Interface:** A responsive design for ease of use on all devices.
- **Performance:** Optimized API endpoints, lazy loading where applicable, and caching frequently accessed Data.

## 2. Tech Stack

### Frontend:

- ReactJS: Web app development
- CSS: styling
- React Router Dom: Building multiple page application
- React-Bootstrap: UI components
- Axios: Making API request

### Backend:

- Node.js: Runtime engine to build backend with Javascript
- Express: Framework to build Web API and routing.
- Bcrypt: Password encryption
- Nodemailer: Email automation
- Gmail: Real Mail server
- Database: MongoDB: Storing users, books, ratings and borrowing data.
- Authentication: JWT for secure, token-based session management.
- Deployment: Cloud hosting on Vercel and Cyclic(OR any free cloud hosting)
- Version Control: Git with GitHub collaboration and versioning.

## 3. Project Architecture

### 1. Frontend (React):

#### Pages:

1. **Sign up:** Business customers can create a new account.
2. **Login:** Both Users login to access their account.
3. **Forgot Password**: Both Users can reset the password.
4. **Home**: Display item categories, featured items, and search functionality.
5. **Item Details**: View detailed information and user ratings.
6. **Profile**: Shows user information, renting history, and Reviews.
7. **Admin Dashboard:**
   - Dashboard summary
   - Manage items,
   - Manage Users,
   - Manage Renting History
   - Manage Reviews.

### 2. Backend (Node.js + Express):

1. **APIs:** RESTful APIs for each core functionality (User, Item, Renting and Ratings).
2. **Controllers:** Handle business logic for each route.
3. **Models:** Mongoose schemas for MongoDB collections.
4. **Middleware:** Authentication (JWT), input validation (JOI), error handling.

### 3. Database (MongoDB):

#### Collections:

1. **Users:** Stores user details, role, authentication tokens, and renting history.
2. **Items:** Item metadata, availability, and cumulative ratings.
3. **RentHistory:** Dedicated collection for tracking item renting and return details.
4. **Reviews:** Dedicated collection for tracking item reviews and ratings given by users.
5. **Sessions:** Dedicated collection for managing User login details.

## 4. Updated Database Schema Design

The data model for the application is defined across the following MongoDB collections:

### User Collection

{
"\_id": "ObjectId",
"status": "inactive",
"fName": "string",
"lName": "string",
"email": "string",
"phone": "string",
"password": "string",
"role": "string", // "admin" or "user"
"refreshJWT": "string"
}

### Item Collection

{
"\_id": "ObjectId",
"status": "string",
"title": "string",
"year": "number",
"author": "string",
"imgUrl": "string",
"itemNumber": "string",
"genre": "string",
"available": "boolean",
"averageRating": "number", // average of all ratings,
"description": "string",
"expectedAvailable": "date",
"AddedBy":{
"name": "string",
"adminId": "ObjectId"
},
"lastUpdated":{
"name": "string",
"adminId": "ObjectId"
},
"\*\*\*": "More properties will be added as we go and develop."
}

### RentHistory Collection

{
"\_id": "ObjectId",
"userId": "ObjectId",
"itemId": "ObjectId",
"itemTitle": "String",
"thumbnail": "String",
"dueDate": "Date",
"isReturned": "boolean",
"returnedDate": "Date",
"reviewId": "ObjectId", //if no id means, no review yet.
"\*\*\*": "More properties may be added as we go and develop."
}

### Review Collection

{
"\_id": "ObjectId",
"userId": "ObjectId",
"userName": "String",
"itemId": "ObjectId",
"rentId": "ObjectId",
"title": "String",
"review": "String",
"rating": "Number", // between 1 and 5
"isApproved": "boolean",
"\*\*\*": "More properties may be added as we go and develop."
}

### Session Collection

{
"\_id": "ObjectId",
"token": "String",
"association": "String",
"expire": "Date", //or timestamp, add expire: 0 to delete the document automatically once the date expires
"\*\*\*": "More properties May be added as we go and develop."
}

## 5. API Endpoints

### 5.1 Authentication

- POST /api/v1/auth/register: Register a new user.
- POST /api/v1/auth/activate-user: Verify user email and activate the account.
- POST /api/v1/auth/login: Login as a user and receive a JWT token.
- GET /api/v1/auth/logout: Logout and invalidate JWT.
- GET /api/v1/auth/renew-jwt: Renew JWT Token.
- POST /api/v1/auth/otp: Request OTP.
- POST /api/v1/auth/reset-password: Request OTP.

### 5.2 Items

- GET /api/v1/items/admin/:?id: Retrieve single or list of all items by admin only
- GET /api/v1/items/:?id: Retrieve a single or list of all the active items only
- POST /api/v1/items: Admin-only, add a new item.
- PUT /api/v1/items/:id: Admin-only, update item details.
- DELETE /api/v1/items/:id: Admin-only, delete an item.

### 5.3 Renting

- POST /api/v1/rents: Allows users (both admin and customers) to rent an item.
- PUT /api/v1/rents/return: Allows users to return a rented item and add a review.
- GET /api/v1/rents/history: View renting history; customers see only their history, admins can view all.

### 5.4 Reviews

- POST /api/v1/review: Submit a review with a rating when returning an item.
- PATCH /api/v1/review/:id: Admin only, approve the review.
- GET /api/v1/items/:id/reviews: Retrieve reviews and ratings with average rating for a specific item.

### 5.5 Admin

- GET /api/v1/admin/reports: Admin-only endpoint to view business reports and analytics.

### 5.6 Users & Admin

- GET /api/v1/users: Admin-only get all user.
- GET /api/v1/users/profile: Get logged in user profile details.
- PUT /api/v1/users: Update their profile details.
- PATCH /api/v1/users/update-password: Update their password.
- DELETE /api/v1/users: Delete their own profile.

## 6. Project Setup & Hosting

### 6.1 Development Setup

1. **Environment:**
   - Install Node.js and MongoDB locally.
   - Use an .env file for managing environment variables (e.g., JWT_SECRET, MONGODB_URI).
2. **Frontend:**
   - Set up React with Vite.
   - Use Axios for handling API requests.
   - State management with Redux if needed.

### 6.2 Hosting

- Cloud Provider: Deploy on Vercel and Cyclic (Or any cloud provider)
- CI/CD Pipeline: Set up using GitHub Actions for seamless deployment.
- Monitoring: Set up monitoring tools like LogDNA, DataDog, or ELK Stack for error and performance tracking.

## 7. Security Considerations

- JWT Token Security: Secure tokens on the client and server.
- Prevent Private Page Access: Allow only loginedIn users to access private pages
- Prevent Private API endpoint Access: Allow only loginedIn user account to make API call
- Password Encryption: Use bcrypt to hash passwords.
- Role-Based Access: Implement middleware to restrict access to admin-only routes.
- Input Validation: Validate all user inputs to prevent SQL injection and cross-site scripting (XSS).

## 8. Frontend Folder Structure (React)

The following structure organises components, pages, and assets effectively, making the codebase modular and scalable.

frontend/
├── public/
│ ├── index.html # Main HTML file
│ └── favicon.ico # Favicon
│
├── src/
│ ├── redux/ # Redux store setup
│ │ ├── store.js # Configures the Redux store
│ │ └── rootReducer.js # Combines all reducers
│ │
│ ├── assets/ # Static files: images,fonts,etc.
│ │ ├── images/ # Images
│ │ └── styles/ # Global styles (CSS, SASS files)
│ │
│ ├── components/ # Reusable UI components
│ │ ├── Button/
│ │ │ ├── Button.js # Component file
│ │ │ └── Button.css # Component styling
│ │ └── Header/
│ │ ├── Header.js
│ │ └── Header.css
│ │
│ ├── features/ # Redux slices and asynchronous logic
│ │ ├── user/ # User feature
│ │ │ ├── userSlice.js # Redux slice for user
│ │ │ ├── userAction.js # Action Functions for user
│ │ │ └── userApi.js # API functions related to the user
│ │ ├── auth/ # Auth slice
│ │ │ ├── authSlice.js # Redux slice for authentication
│ │ │ └── authApi.js # API functions related to auth
│ │ ├── items/ # Items slice
│ │ │ ├── itemsSlice.js # Redux slice for items
│ │ │ ├── itemAction.js # Action Functions for items
│ │ │ └── itemsApi.js # API functions related to items
│ │ ├── rent/ # Renting slice
│ │ │ ├── rentSlice.js # Redux slice for renting
│ │ │ └── rentApi.js # API functions related to renting
│ │ └── review/ # Review slice
│ │ ├── reviewSlice.js # Redux slice for reviews
│ │ └── reviewApi.js # API functions related to reviews
│ │
│ ├── hooks/ # Custom React hooks
│ │ ├── useFetch.js # Custom hook for fetching data
│ │ └── useForm.js # Custom hook for form data
│ │
│ ├── pages/ # Route pages
│ │ ├── auth/ # Authentication-related pages
│ │ │ ├── SignUpPage.js # Sign Up page
│ │ │ ├── SignInPage.js # Sign In page
│ │ │ ├── VerifyUser.js # To verify user page
│ │ │ └── ForgetPasswordPage.js # Forgot Password page
│ │ ├── items/ # Item-related pages
│ │ │ ├── ItemLandingPage.js # Landing page for items (item overview)
│ │ │ ├── NewItemPage.js # Page for creating a new item
│ │ │ └── EditItemPage.js # Page for editing an existing item
│ │ ├── dashboard/ # Dashboard-related pages
│ │ │ └── DashboardPage.js
│ │ ├── home/ # Home page
│ │ │ └── HomePage.js
│ │ ├── reviews/ # Review-related pages
│ │ │ └── ReviewsPage.js
│ │ ├── user/ # User-related pages
│ │ │ └── UserPage.js # Main page for user profile or management
│ │
│ ├── services/ # External API and HTTP request services
│ │ └── api.js # General API configurations
│ │
│ ├── utils/ # Resuable function
│ │ └── validatePassword.js # Validate to enforce strong password
│ │
│ ├── App.jsx # Main app component
│ ├── main.jsx # Entry point for the app
│ └── routes/ # App routes
│ └── AppRoutes.js # Define and manage all app routes
│
├── .env # Environment variables
├── .gitignore # Git ignore file
└── package.json

### Explanation

- assets: For static resources like images, fonts, and global stylesheets.
- components: Houses reusable UI components (e.g., Button, Header).
- Redux: Stores Redux files to manage global state.
- hooks: Contains custom React hooks for handling logic outside of components.
- pages: Contains page-level components that map to specific routes.
- services: Organizes API calls for a clean separation of business logic from UI.
- routes: Defines application routes in a single place for better readability.

## 9. Backend Folder Structure (Node.js + Express)

This structure organizes the backend into models, controllers, and routes, keeping the code modular and maintainable.

backend/
├── src/
│ ├── config/ # Configuration files
│ │ ├── dbConfig.js # Database connection setup
│ │ ├── jwt.js # JWT configuration (optional)
│ │ └── config.js # Env variables and general (optional)
│ │
│ ├── controllers/ # Route handler logic
│ │ ├── authController.js # Authentication logic
│ │ ├── itemController.js # Item-related logic
│ │ └── rentController.js # Renting and history management logic
│ │
│ ├── middleware/ # Custom middleware
│ │ ├── authMiddleware.js # Auth middleware
│ │ ├── Validation/ # Server Side validation
│ │ │ ├── joiValidation.js # Request Validator middleware (Joi-based)
│ │ │ └── authDataValidation.js # All auth related Request Validator
│ │ ├── errorHandler.js # Global error handler middleware
│ │ └── responseClient.js # Centralized response handler middleware
│ │
│ ├── models/ # Mongoose models (schema definitions)
│ │ ├── User/ # User schema
│ │ │ ├── UserSchema.js
│ │ │ └── UserModel.js
│ │ ├── Item/ # Item schema
│ │ │ ├── ItemSchema.js
│ │ │ └── ItemModel.js
│ │ ├── RentHistory/ # Rent history schema
│ │ │ ├── RentHistorySchema.js
│ │ │ └── RentHistoryModel.js
│ │ ├── Review/ # Review schema
│ │ │ ├── ReviewModel.js
│ │ │ └── RentHistoryModel.js
│ │ └── Session/ # Session schema
│ │ ├── SessionSchema.js
│ │ └── SessionModel.js
│ │
│ ├── routes/ # API routes
│ │ ├── authRoutes.js # Auth routes
│ │ ├── userRoutes.js # user management routes
│ │ ├── itemRoutes.js # Item management routes
│ │ └── rentRoutes.js # Renting routes
│ │
│ ├── services/ # Business logic and external services
│ │ ├── email/ # Email-related services
│ │ │ ├── emailService.js # Main email sending logic
│ │ │ ├── emailTemplates.js # Email templates (e.g., welcome, reset password)
│ │ │ └── transport.js # Email transporter (e.g., nodemailer)
│ │ ├── file/ # File upload-related services (e.g., Cloudinary)
│ │ │ └── fileService.js # Main file upload logic
│ │
│ └── utils/ # utility functions
│ ├── bcrypt.js # Password encryption and compare
│ └── jwt.js # generate and decode JWTs
|
├── .env # Environment variables
├── .gitignore # Git ignore file
├── package.json # Dependencies and scripts
├── server.js # Main entry point of the app
└── README.md # Project documentation

### Explanation

- config: Stores configuration files, such as database setup, environment variables, and JWT settings.
- controllers: Holds all the business logic and methods for each route.
- middleware: Contains custom middleware for authentication, error handling, and request validation.
- models: Stores Mongoose schema definitions for each collection in MongoDB.
- routes: Organizes API routes by feature (e.g., auth, item, rent).
- utils: Contains helper functions, such as token generation and data validation.

### Notes on Best Practices

- Environment Variables: Store sensitive information like API keys and database URIs in .env files.
- Code Splitting: Keep controller functions and route definitions separate for maintainability.
- Modularization: Each feature or functionality has its own directory to minimize cross-dependencies, making the codebase easier to navigate and scale.

## 10. Project Sprints and Milestones

### 10.1 Sprint 1: Project Setup & Basic Infrastructure

**Goal:** Set up the project infrastructure, initialize repositories, configure the development environment, and establish database connections.

**Tasks:**

1. **Project Initialization:**
   - Initialize Git repositories and set up .gitignore.
   - Set up environment variables (.env files) for both frontend and backend.
2. **Backend Setup:**
   - Configure Node.js and Express environment.
   - Install backend dependencies (express, mongoose, jsonwebtoken, bcryptjs, cors, morgan, joi, Nodemailer).
   - Set up MongoDB connection using Mongoose.
3. **Frontend Setup:**
   - Initialize React app using vite.
   - Install frontend dependencies (redux-toolkit, react-router-dom, axios, react-redux, react-toastify, react-bootstrap, bootstrap, react-icons).
4. **Version Control & Documentation:**
   - Document setup steps in a README file for the team.
   - Commit the initial setup to the repository.

### 10.2 Sprint 2: User Authentication and Basic APIs

**Goal:** Implement user authentication and core backend APIs for managing items.

**Tasks:**

1. **User Authentication:**
   - Create backend routes for /api/v1/auth.
   - Implement JWT-based authentication.
   - Create authorization middleware to differentiate admin and customer access.
2. **Basic Item Management API:**
   - Create backend routes for item CRUD operations (/api/v1/items).
   - Define item controller with getAllItems, getItemById, createItem, updateItem, and deleteItem.
   - Set up authorization for admin-only routes.
3. **Initial Frontend Pages:**
   - Develop LoginPage and RegisterPage, ForgetPasswordPage components.
   - Set up Redux authSlice for handling the authentication state.

### 10.3 Sprint 3: Admin Item Management and Redux Integration

**Goal:** Implement Redux for state management and build core pages for item mgmt and details.

**Tasks:**

1. **Redux Store Setup:**
   - Configure Redux store with Redux Toolkit.
   - Create Redux slices: authSlice, itemsSlice.
2. **Frontend Item Pages And API:**
   - Develop HomePage for listing all items.
   - Develop ItemDetailsPage for displaying individual item details.
   - Integrate API calls in Redux slices to fetch items and item details.
3. **Basic UI Styling:**
   - Set up global styles and CSS framework (e.g., Bootstrap or Material-UI).
   - Implement basic UI elements for item display.

### 10.4 Sprint 4: Renting System & User Profile

**Goal:** Build the renting system and user profile features, allowing customers to rent and return items.

**Tasks:**

1. **Renting APIs:**
   - Define /api/rent routes for handling renting actions.
   - Create rentItem and returnItem controllers.
   - Implement getRentHistory for viewing user renting history.
2. **Frontend Renting Pages:**
   - Develop UserProfilePage to show renting history.
   - Add rent and return buttons on ItemDetailsPage.
   - Create rentSlice in Redux for renting-related actions.
3. **User Feedback on Renting Actions:**
   - Implement success/error notifications on renting and returning actions.

### 10.5 Sprint 5: Admin Dashboard & Item Reviews/Ratings

**Goal:** Build the admin dashboard for item management and implement the rating feature for items.

**Tasks:**

1. **Admin Dashboard:**
   - Create AdminDashboard with item management functions (CRUD).
   - Integrate API calls in itemsSlice for creating, updating, and deleting items.
2. **Item Review Feature:**
   - Add a rating component on the ItemDetailsPage.
   - Create an API endpoint for submitting ratings upon item return.
   - Update item model to store and retrieve average ratings.
3. **Access Control:**
   - Ensure access control for admin-only features in the frontend.

### 10.6 Sprint 6: Testing & Quality Assurance

**Goal:** Conduct unit and integration testing on the frontend and backend, ensuring reliability and smooth functionality.

**Tasks:**

1. **Backend Testing:**
   - Write unit tests for authentication, item, and renting controllers.
   - Test authorization middleware to validate role-based access.
2. **Frontend Testing:**
   - Write component tests for critical components (e.g., LoginPage, HomePage, ItemDetailsPage).
   - Write integration tests for Redux slices (authSlice, itemsSlice, rentSlice).
3. **End-to-End Testing:**
   - Create end-to-end tests to simulate user flows (login, rent, return, rate).
   - Ensure that admin access control works as expected.

### 10.7 Sprint 7: Styling, UI/UX Enhancements & Deployment

**Goal:** Finalize the styling and deploy the app to production.

**Tasks:**

1. **UI/UX Enhancements:**
   - Improve responsive design for all pages (desktop and mobile).
   - Add loading states and success/error feedback for all actions.
2. **Deployment:**
   - Deploy the backend to a cloud provider (e.g., Cyclic or Render).
   - Deploy the frontend to a cloud platform (e.g., Netlify or Vercel).
   - Configure environment variables for production.
3. **Final Documentation:**
   - Update README with deployment steps and usage instructions.
   - Document API endpoints and Redux store structure.

## 11. Trello Board Structure

### Suggested Lists:

- Backlog: Contains all tasks that haven’t been assigned to a sprint yet.
- Sprint 1 - Setup to Sprint 7 - Deployment: Individual lists for each sprint, with specific tasks as cards.
- In Progress: Move tasks currently being worked on.
- Review/QA: Tasks that need review or testing.
- Done: Completed tasks.
