# 🏡 Stay Ease Airbnb Clone Backend

## 🚀 Objective
This backend is the foundation for the **Airbnb Clone Project**, designed to manage users, property listings, bookings, payments, and reviews. It supports robust functionality to mirror the core experience of Airbnb for both guests and hosts.

---

## 🏆 Project Goals
- **User Management:** Secure user registration, authentication, and profile handling.
- **Property Management:** Host property listings with full CRUD capabilities.
- **Booking System:** Reserve and manage stays, including check-in/check-out flows.
- **Payment Processing:** Handle secure payments for bookings.
- **Review System:** Allow guests to rate and review stays.
- **Map Integration:** Support location based property searching and map displays
- **Data Optimization:** Ensure efficient queries and scalability using caching and indexing.

---

## 🛠️ Technology Stack

| Technology        | Purpose                                                                 |
|-------------------|-------------------------------------------------------------------------|
| Django            | A high-level Python web framework for building RESTful APIs             |
| Django REST Framework | Provides tools for creating and managing RESTful endpoints          |
| PostgreSQL        | Relational database for storing structured project data                 |
| GraphQL           | Enables flexible querying of data via schemas                           |
| Celery            | Asynchronous task queue (e.g., for email or payment processing)         |
| Redis             | In-memory data store for caching and session handling                   |
| Docker            | Containerization to ensure consistent environments                      |
| JWT               | JSON web token for user authentication                                  |
| Stripe            | For secure payment processing                                           |
| Mapbox/ Google Maps API | For property location services                                    |
| CI/CD Pipelines   | Automates testing, deployment, and integration using GitHub Actions etc.|

---

## 🧠 Database Design

The core entities and their relationships:

### 📄 Entities & Fields

- **User**
  - `id`, `name`, `email`, `password_hash`, `created_at`
  - A user can host multiple properties and make multiple bookings.

- **Property**
  - `id`, `host_id`, `title`, `description`, `location`, `price_per_night`
  - Belongs to a user (host), and can have many bookings and reviews.

- **Booking**
  - `id`, `user_id`, `property_id`, `check_in`, `check_out`, `status`
  - Connects a user with a property and tracks reservation details.

- **Payment**
  - `id`, `booking_id`, `amount`, `payment_status`, `timestamp`
  - Associated with a booking; ensures payment is tracked.

- **Review**
  - `id`, `user_id`, `property_id`, `rating`, `comment`
  - Tied to both user and property.
  - **Amenity**
  - `id`, `name`, `icon`
  - Many-to-many relationship with properties.

- **Image**
  - `id`, `property_id`, `image_url`, `is_primary`
  - A property can have multiple images.

- **Saved Property**
  - `id`, `user_id`, `property_id`, `saved_at`
  - Tracks properties saved by users for later viewing.

**Relationships**:
- One user can list many properties.
- One user can make many bookings.
- A booking is linked to one user and one property.
- A payment is linked to one booking.
- A review is linked to one user and one property.
- A property can have many images
- A user can save many properties

---

## 🧩 Feature Breakdown

### 1. User Management
- User registration and authentication
- User profile creation and management
- Role based access control (guest, host, admin)
### 2. Property Management
- Create, Read, Update, Delete (CRUD) operations with property listings
- Property search with filtering (location, price, dates,capacity and amenities)
- Property image management with multiple uploads
- Location-based property discovery with map integration
### 3. Booking system
- Check availability for specific dates
- Create and manage bookings
- Booking status updates (pending, confirmed, completed, cancelled)
- Conflict prevention for double booking

### 4. Payment processing
- Secure payment intergration with stripe
- Payment status tracking
- Refund processing for cancellation
- Invoice integration

### 5. Review System
- Create, Read, Update, Delete reviews
- Rating system (1 - 5) stars
- Review verification (allow only reviews from completed stays)
- Response system for hosts to reply to reviews
  
### 6. Map Integration
- Geocoding for property address
- Map visualization API endpoints
- Location based search queries
- Distance calculation from points of interest

### 7. Notification system
-Email notifications for booking confirmations
- Reminder for upcoming stays
- Review requests after check outs
- Host notifications for new bookings

### 8. API Optimization
- Response pagination for large datasets
- Data caching with redis
- Database indexing for common queries
- API rate limiting to prevent abuse.
---

## 🔐 API Security

### Key Measures
- **Authentication**: Using token-based authentication (JWT or session-based) to ensure only verified users can access protected endpoints.
- **Authorization**: Only property owners can modify their listings; only guests can leave reviews for places they’ve booked.
- **Rate Limiting**: Prevents abuse by limiting API calls per user/IP.
- **Input Validation**: Prevents SQL injection, XSS, and other common vulnerabilities.
- **HTTPS**: All API endpoints require secure HTTPS connections.
- **Password Security**: Secure password hashing and storage.

### Importance
- **User Data Protection**: Secure credentials and private information.
- **Booking & Payment Security**: Prevent fraud or manipulation of bookings and payments.
- **System Integrity**: Keeps the platform stable and resilient against attacks.

---

## 📦 API Integration with Frontend

### 1. Authentication Flow
- Provides JWT tokens for frontend authentication
- Handles token refresh and validation
- Exposes user profile data for the frontend profile page

### 2. Property Data for UI Components
- Optimized endpoints for property cards in the listing grid
- Detailed property information for the property detail page
- Image galleries and amenity lists

### 3. Booking Interaction
- Availability checking during date selection
- Real-time booking status updates
- Calendar data for the availability display

### 4. Payment Integration
- Secure payment intent creation for Stripe Elements
- Frontend hooks for payment confirmation
- Payment history for user dashboards

### 5. Review Management
- Review submission from the frontend
- Rating display and calculation
- Host response integration

---

## 📈 API Documentation

### REST API Endpoints

#### 👤 Users & Authentication
- `POST /api/auth/register/` – Register a new user
- `POST /api/auth/login/` – User login (returns JWT)
- `POST /api/auth/refresh/` – Refresh JWT token
- `GET /api/auth/me/` – Get current user profile
- `PUT /api/auth/me/` – Update user profile
- `POST /api/auth/password/change/` – Change password

#### 🏘 Properties
- `GET /api/properties/` – List all properties with filtering
- `POST /api/properties/` – Create new property (host only)
- `GET /api/properties/{id}/` – Get property details
- `PUT /api/properties/{id}/` – Update property (owner only)
- `DELETE /api/properties/{id}/` – Delete property (owner only)
- `POST /api/properties/{id}/images/` – Add images to property
- `GET /api/properties/featured/` – Get featured properties
- `GET /api/properties/nearby/?lat={lat}&lng={lng}` – Find nearby properties

#### 📅 Bookings
- `GET /api/bookings/` – List user's bookings
- `POST /api/bookings/` – Create a booking
- `GET /api/bookings/{id}/` – View booking details
- `PUT /api/bookings/{id}/` – Update booking (status changes)
- `DELETE /api/bookings/{id}/` – Cancel booking
- `GET /api/properties/{id}/availability/` – Check property availability

#### 💰 Payments
- `POST /api/payments/create-intent/` – Create payment intent
- `POST /api/payments/confirm/` – Confirm payment
- `GET /api/payments/history/` – View payment history
- `POST /api/payments/refund/{booking_id}/` – Request refund

#### ⭐ Reviews
- `GET /api/reviews/property/{property_id}/` – List property reviews
- `POST /api/reviews/` – Add a review
- `GET /api/reviews/{id}/` – View a review
- `PUT /api/reviews/{id}/` – Edit review (author only)
- `DELETE /api/reviews/{id}/` – Delete review (author only)
- `POST /api/reviews/{id}/response/` – Host response to review

#### 🗺️ Maps & Locations
- `GET /api/locations/geocode/?address={address}` – Geocode an address
- `GET /api/locations/properties/?bounds={sw_lat},{sw_lng},{ne_lat},{ne_lng}` – Get properties in map bounds

#### 💾 Saved Properties
- `GET /api/saved/` – List user's saved properties
- `POST /api/saved/{property_id}/` – Save a property
- `DELETE /api/saved/{property_id}/` – Remove a saved property

---

## 🚀 Getting Started

### Prerequisites
- Python 3.8+
- PostgreSQL
- Redis
- Docker (optional but recommended)
---

## 🛠 CI/CD Pipeline

### What is CI/CD?
CI/CD stands for Continuous Integration and Continuous Deployment. It automates testing and deployment of code changes, ensuring high-quality, reliable releases.

### Tools Used
- **GitHub Actions**: Automates testing and builds on every pull request and push.
- **Docker**: Ensures consistent development, testing, and production environments.
- **Heroku / AWS / Render**: For deployment.

### Benefits
- Faster integration of features and bug fixes.
- Automated checks prevent broken code from reaching production.
- Ensures every push meets project standards.

---

## 👥 Team Roles

### 1. **Backend Developer**
- Implements API endpoints, core logic, and integrations.
- Ensures adherence to project architecture and business rules.

### 2. **Database Administrator (DBA)**
- Designs the database schema, optimizes queries, manages indexes and migrations.

### 3. **DevOps Engineer**
- Sets up the deployment pipeline, manages server infrastructure, and monitors performance.

### 4. **QA Engineer**
- Writes automated and manual tests, identifies bugs, and verifies functionality.

### 5. **Project Manager (Optional)**
- Coordinates tasks, manages timelines, and ensures all team members meet deliverables.

---

## 📈 API Documentation

### REST API Endpoints

#### 👤 Users
- `GET /users/` – List all users
- `POST /users/` – Register user
- `GET /users/{id}/` – Retrieve a specific user
- `PUT /users/{id}/` – Update a user
- `DELETE /users/{id}/` – Delete a user

#### 🏘 Properties
- `GET /properties/` – List all
- `POST /properties/` – Create new
- `GET /properties/{id}/` – Get property
- `PUT /properties/{id}/` – Update
- `DELETE /properties/{id}/` – Delete

#### 📅 Bookings
- `GET /bookings/` – List bookings
- `POST /bookings/` – Make a booking
- `GET /bookings/{id}/` – View details
- `PUT /bookings/{id}/` – Update booking
- `DELETE /bookings/{id}/` – Cancel booking

#### 💰 Payments
- `POST /payments/` – Process a booking payment

#### ⭐ Reviews
- `GET /reviews/` – List all reviews
- `POST /reviews/` – Add review
- `GET /reviews/{id}/` – View a review
- `PUT /reviews/{id}/` – Edit review
- `DELETE /reviews/{id}/` – Delete review

---

## 📌 Additional Resources
- 📘 [System Design for Booking Apps](https://www.educative.io/courses/grokking-the-system-design-interview)
- 🛠 [ITRexGroup: Roles in Software Development](https://itrexgroup.com/blog/software-development-team-structure/)
- 📚 [Django REST Framework Docs](https://www.django-rest-framework.org/)
- ⚙️ [GraphQL Docs](https://graphql.org/learn/)

---

## 🧾 License
This project is part of the **ALX Software Engineering Program** and is open-source for learning and collaboration.

---

