# Project: Fashion Rental System (LuxeyFRS) — Frontend Client

LuxeyFRS is a responsive web application designed for renting premium apparel and fashion accessories. The client application offers tailored user experiences for both regular Customers and administrative staff, managing catalogs, inventories, user authentications, automated styling returns, and rental review scoring.

## 1. Core System Architecture

The frontend communicates with a cloud-hosted RESTful API built on Node.js/Express. State is synchronized using Redux Toolkit, and secure operations are maintained via short-lived JWT signatures and contextual routing rules.

                  +-----------------------------------+
                  |      React Application (SPA)      |
                  +-----------------------------------+
                                    |
          +-------------------------+-------------------------+
          |                                                   |
          v                                                   v

+-------------------+ +-------------------+
| Public Context | | Private Context |
+-------------------+ +-------------------+
| - Home Page | | - User Profile |
| - Authentication | | - Order Checkout |
| - Wardrobe Browse | | - Admin Control |
+-------------------+ +-------------------+
| |
+-------------------------+-------------------------+
|
v
+-----------------------------------+
| Axios HTTP Client Layer |
+-----------------------------------+
| (JWT Bearer Token)
v
+-----------------------------------+
| Backend REST API |
+-----------------------------------+

## 2. Tech Stack & Engineering Tools

Core Library: ReactJS (v18+)

Build Tool & Bundler: Vite (for rapid HMR and optimized asset compilation)

Routing Engine: React Router Dom (Declarative browser routing)

State Architecture: Redux Toolkit & React-Redux (Global application store & asynchronous dispatch triggers)

Network layer: Axios (HTTP client featuring automatic request/response token interceptors)

UI Framework: React-Bootstrap & Bootstrap 5 (Responsive flexbox grid layouts and components)

## 3. Interface Design & Route Architecture

The user interface maps directly across the following structured single-page application entry points:

### Public Layouts & Identity Access

Sign Up (/register): Consumer account creation with integrated input sanitization.

Login (/login): Credentials portal establishing stateful JWT handshakes.

Forgot Password (/forgot-password): Secure password recovery sequence via automated verification tokens.

Home / Storefront (/): Display window highlighting item categories, featured garments, comprehensive inventory sorting, and filtering parameters.

Item Detail Profile (/items/:id): Isolated garment parameters showing designer brand, condition, historical rating averages, real-time availability logs, and checkout portals.

### Authenticated / Restricted Layouts

User Dashboard / Profile (/profile): Customer profile management dashboard including live rental tracking metrics, historical item logs, outstanding late return notifications, and pending review assignments.

Admin Control Suite (/admin/dashboard): Unified administration board isolating management features into sub-views:

Inventory Controller: Complete CRUD capabilities over physical fashion assets.

User Directory: System profile audits and privilege elevation controls.

Rental Auditing Logs: Full system transaction tracking for items currently on lease.

Review Moderator: Content monitoring tools for customer feedback submissions.

## 4. Codebase Directory Map

Plaintext
frontend/
├── public/ # Static unmodified assets (icons, manifests)
└── src/
├── assets/ # Global stylistic and branding components
│ ├── images/ # Image configurations and media banners
│ └── styles/ # Overriding stylesheets and Bootstrap components
├── components/ # Stateless, atomic UI modules
│ ├── Button/ # Reusable customized action targets
│ └── Header/ # Global navigation systems
├── features/ # Domain-driven state management blocks
│ ├── auth/ # Verification slices, tokens, and storage handlers
│ ├── items/ # Wardrobe state matrices, filters, and records
│ ├── rent/ # Rental transaction records and current lease histories
│ └── review/ # User-generated scaling scores and content payloads
├── hooks/ # Custom business logic hooks
│ ├── useFetch.js # Declarative HTTP data fetching abstractions
│ └── useForm.js # Form state encapsulation and handling
├── pages/ # High-level route wrapper templates
├── routes/ # Explicit application routing boundaries
│ └── AppRoutes.js # Route gatekeeper configuration files
├── services/ # Axios API core network abstraction layers
│ └── api.js # Network interceptor initialization scripts
├── utils/ # Reusable functional code snippets
│ └── validatePassword.js
├── App.jsx # Core UI context provider wrapper
└── main.jsx # DOM mounting target script

## 5. API Network Layer Contract

The client queries the underlying data model through the following strict REST architecture configurations:

Domain Request Route Payload Requirements / Responses Access Credentials
Auth POST /api/v1/auth/register Register fields → Create account Public
Auth POST /api/v1/auth/login Return signed JWT access credentials Public
Auth GET /api/v1/auth/logout Revoke Client Tokens Authenticated
Items GET /api/v1/items Query catalog variations and details Public
Items POST /api/v1/items Payload containing garment schema parameters Admin Only
Items PUT /api/v1/items/:id Delta modifications to existing asset indices Admin Only
Items DELETE /api/v1/items/:id Drop target primary key index from storage Admin Only
Leasing POST /api/v1/rents Allocate item ID → Initialize lease period Authenticated
Leasing PUT /api/v1/rents/return Commit item ID return → Trigger feedback modal Authenticated
Leasing GET /api/v1/rents/history Return collection logs (Admins filter all, Users filter self) Authenticated
Reviews POST /api/v1/review Dispatch rating value integers (1−5) & commentary Authenticated
Reviews PATCH /api/v1/review/:id Modify visibility flag settings Admin Only
Metrics GET /api/v1/admin/reports Returns overall analytic summaries Admin Only

## 6. Client Security Implementations

JWT Context State Isolation: Application access tokens are held in securely enclosed transient React memory pools. Persistent storage hooks leverage automated state token validation strategies against the /renew-jwt routing module to neutralize local cross-site scripting vulnerabilities.

Navigation Guard Rails: Route configuration wrappers process authentication contexts on page transitions. Private user accounts and Admin contexts evaluate structural data properties inside the authSlice to stop layout access by unauthenticated or lower-privilege users.

Outbound Data Sanitization: Form interaction states are captured within controlled UI instances, executing strict length limitations and pattern validation schemas prior to dispatching network payloads.

## 7. Local Environment Setup & Cloud Deployment

### Local Setup Configuration

Clone the codebase and move to the project root directory:

Bash
cd frontend
Install the system package library items:

Bash
npm install
Establish a standard configuration file named .env inside the root layout path:

Code snippet
VITE_API_BASE_URL=http://localhost:8080/api/v1
Spin up the local HMR development node server environment:

Bash
npm run dev

### Build Options & Production Hosting

Production Compilation: Transpile the source code files down to optimized native browser modules via Vite:

Bash
npm run build
Target Cloud Deployments: The generated /dist build outputs are ready for static hosting configuration environments like Vercel, Netlify, or AWS S3 buckets. Ensure fallback navigation routing instructions are explicitly directed to index.html to prevent 404 routing failures across single-page client transitions.
