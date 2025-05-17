# StayEase: Database Requirements Analysis Document

## 1. Introduction

This document outlines the database requirements for the **StayEase accommodation booking platform**. It identifies the core data entities, their attributes, relationships, and functional requirements that the database must support.

---

## 2. Project Overview

StayEase is an accommodation booking platform that connects **hosts** with **guests**. The platform allows hosts to list their properties and guests to discover, book, and review these accommodations. The database will serve as the foundation for all platform functionality, including:

- User management  
- Property listings  
- Bookings  
- Payments  
- Reviews  

---

## 3. Core Entities and Attributes

### 3.1 User Entity

**Attributes:**
- User ID (Primary Key)
- Full Name
- Email Address
- Password (hashed)
- Phone Number
- Profile Picture
- Account Type (guest, host, or both)
- Account Creation Date
- Last Login Date
- Account Status (active, inactive, suspended)
- Email Verified Status
- Identity Verification Status

**Functional Requirements:**
- Support secure user authentication  
- Track user activity  
- Maintain host/guest distinction  
- Enable identity verification process  
- Support user profile management  

---

### 3.2 Property Entity

**Attributes:**
- Property ID (Primary Key)
- Host ID (Foreign Key to User)
- Title
- Description
- Property Type (apartment, house, villa, etc.)
- Room Type (entire place, private room, shared room)
- Address, City, State/Province, Country, Postal Code
- Geographic Coordinates (latitude, longitude)
- Number of Bedrooms/Bathrooms
- Maximum Number of Guests
- Base Price per Night
- Cleaning Fee, Service Fee
- Check-in Instructions, House Rules, Cancellation Policy
- Creation Date, Last Update Date
- Property Status (active, inactive, pending approval)

**Functional Requirements:**
- Support property listing creation and management  
- Enable property search and filtering  
- Support location-based discovery  
- Track property performance metrics  
- Handle property availability management  

---

### 3.3 Property Image Entity

**Attributes:**
- Image ID (Primary Key)
- Property ID (Foreign Key to Property)
- Image URL/Path
- Image Caption
- Is Primary Image (boolean)
- Upload Date

**Functional Requirements:**
- Support multiple images per property  
- Identify primary/feature images  
- Track image metadata  

---

### 3.4 Amenity Entity

**Attributes:**
- Amenity ID (Primary Key)
- Name
- Category (essential, safety, special)
- Icon Reference

**Functional Requirements:**
- Support property amenity filtering  
- Group amenities by category  
- Display amenity icons in the UI  

---

### 3.5 Property-Amenity Relationship

**Attributes:**
- Property ID (Foreign Key to Property)
- Amenity ID (Foreign Key to Amenity)

**Functional Requirements:**
- Support multiple amenities per property  
- Enable amenity-based property filtering  

---

### 3.6 Booking Entity

**Attributes:**
- Booking ID (Primary Key)
- Property ID (Foreign Key to Property)
- Guest ID (Foreign Key to User)
- Check-in / Check-out Dates
- Number of Guests
- Booking Status (pending, confirmed, completed, cancelled, rejected)
- Total Price
- Creation/Update Dates
- Special Requests
- Cancellation Reason/Date

**Functional Requirements:**
- Support booking creation and management  
- Track booking status changes  
- Handle date-based availability  
- Calculate pricing  
- Support booking modifications and cancellations  

---

### 3.7 Payment Entity

**Attributes:**
- Payment ID (Primary Key)
- Booking ID (Foreign Key to Booking)
- Guest ID (Foreign Key to User)
- Amount, Currency
- Payment Method
- Payment Status
- Transaction ID
- Payment Date
- Refund Amount/Date/Reason (if applicable)

**Functional Requirements:**
- Support secure payment processing  
- Track payment status  
- Handle refunds  
- Generate receipts  
- Maintain payment history  

---

### 3.8 Review Entity

**Attributes:**
- Review ID (Primary Key)
- Booking ID (Foreign Key to Booking)
- Property ID (Foreign Key to Property)
- Guest ID (Foreign Key to User)
- Overall Rating (1–5 stars)
- Sub-ratings (Cleanliness, Communication, etc.)
- Comment
- Host Response + Date
- Submission Date
- Published Status

**Functional Requirements:**
- Support review submission post-stay  
- Calculate property averages  
- Allow host responses  
- Display reviews on listings  

---

### 3.9 Saved Property Entity

**Attributes:**
- Saved ID (Primary Key)
- User ID (Foreign Key to User)
- Property ID (Foreign Key to Property)
- Date Saved

**Functional Requirements:**
- Support saving/favoriting properties  
- Display saved properties  
- Track engagement  

---

### 3.10 Message Entity

**Attributes:**
- Message ID (Primary Key)
- Conversation ID
- Sender ID (FK to User)
- Recipient ID (FK to User)
- Related Booking ID (optional)
- Message Content
- Attachment URL (optional)
- Read Status
- Sent Date / Read Date

**Functional Requirements:**
- Support inquiries and booking communication  
- Track message status  
- Allow attachments  
- Maintain conversation history  

---

## 4. Relationships Between Entities

- **User – Property**: One-to-Many
- **Property – Image**: One-to-Many
- **Property – Amenity**: Many-to-Many
- **User – Booking (as Guest)**: One-to-Many
- **Property – Booking**: One-to-Many
- **Booking – Payment**: One-to-Many
- **Booking – Review**: One-to-One
- **User – Saved Properties**: One-to-Many
- **User – Message (sent and received)**: One-to-Many (both ways)

---

## 5. Data Integrity and Constraints

### 5.1 Entity Integrity
- Primary keys must be unique and not null  
- Soft deletion for preserving history  

### 5.2 Referential Integrity
- All FKs must match existing PKs  
- Rules for deleting related data:
  - Users: anonymize
  - Properties: keep related bookings/reviews
  - Bookings: only status updates

### 5.3 Domain Integrity
- Email: valid format  
- Passwords: meet security standards  
- Dates: valid ranges  
- Ratings: 1–5  
- Prices: positive  
- Status fields: predefined values only  

---

## 6. Business Rules

### 6.1 Booking
- No double-booking  
- Hosts approve/reject in 24 hours  
- Guests can cancel based on policy  
- Booking requires payment  

### 6.2 Payment
- Payment confirms booking  
- Refunds follow cancellation rules  
- Payment disputes alert support  
- Host paid after check-in  

### 6.3 Review
- Only guests who completed stays can review  
- Reviews allowed for 14 days post-checkout  
- Hosts can reply, not delete  
- Must follow content guidelines  

### 6.4 User
- Email verification mandatory  
- Hosts need identity verification  
- Repeated policy violations = suspension  
- Data is privacy-protected  

---

## 7. Reporting and Analytics Requirements

### 7.1 Host Analytics
- Occupancy rates  
- Avg. booking value  
- Review trends  
- Conversion rates  

### 7.2 Guest Analytics
- Booking history  
- Spending behavior  
- Search patterns  
- Saved properties  

### 7.3 Platform Analytics
- User growth  
- Revenue tracking  
- Popular properties/locations  
- Seasonal trends  
- Keyword analytics  

---

## 8. Performance Requirements

### 8.1 Query Performance
- Property search < 2s  
- Booking creation < 5s  
- Login < 1s  
- Dashboard load < 3s  

### 8.2 Scalability
- 100K+ users  
- 50K+ listings  
- 1K concurrent users  
- 10K bookings/month  

### 8.3 Availability
- 99.9% uptime  
- Backup & recovery  
- Database replication  

---

## 9. Security Requirements

### 9.1 Data Protection
- Encrypt personal info  
- Follow PCI-DSS for payments  
- Protect location data  

### 9.2 Access Control
- Role-based access  
- Log sensitive actions  
- Prevent SQL injection  

---

## 10. Future Considerations

- Multi-currency support  
- Calendar sync  
- Pro host management tools  
- Loyalty programs  
- Subscription listings  
- Instant booking  
- Smart pricing  
- Group booking support  

---

## 11. Conclusion

This requirements analysis provides a foundation for designing the StayEase database schema. The next step is to transform these requirements into an **Entity-Relationship Diagram (ERD)** and then into a **physical database schema**. The system must be **scalable, performant, and secure** to meet both current and future needs.

---
