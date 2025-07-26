# Project Plan: Bus Timetable Information System (BTIS) Design and Setup

**Project Name:** Bus Timetable Information System (BTIS)
**Prepared For:** [Kindhouse]
**Date:** July 26, 2025
**Version:** 1.0

---

### 1. Executive Summary

This project plan outlines the phased approach to design, develop, and deploy a Bus Timetable Information System (BTIS). The system aims to modernize bus station operations by providing real-time bus schedule information to passengers via a website and mobile app, integrate with bus GPS for live updates, and offer a robust web-based admin interface for management. The plan details key phases, deliverables, resource requirements, and a high-level timeline.

### 2. Project Goals & Objectives

**Overall Goal:** To develop and implement a modern, efficient, and user-friendly Bus Timetable Information System (BTIS) that enhances commuter experience and operational efficiency for transportation authorities.

**Specific Objectives:**
* Provide real-time bus schedule information to passengers.
* Have a ticking system that links to the seats in the bus
* Manage Passenger disembarking and embarking
* Reduce passenger waiting times and improve journey planning.
* Integrate with existing/new GPS systems on buses.
* Reduce inquiries to station staff.
* Ensure scalability for future expansions.

### 3. Project Scope

**In-Scope:**
* **Web Portal:** Passenger-facing website for timetable information.
* **Mobile Application:** User-friendly app (iOS & Android) with real-time updates, route planning, notifications, favorite routes, and alerts.
* **Admin Interface:** Web-based portal for managing schedules, routes, bus allocation, and monitoring performance (punctuality, usage patterns).
* **GPS Integration:** API development and integration for real-time bus location and ETA updates.
* **Core Database System:** Design and implementation of a database managing Persons (Users, Drivers), Tickets, Buses, Routes, and Checkpoints (implied under Routes).

**Out-of-Scope (Based on the proposal, may require future phases):**
* Physical digital display screen installation at bus stations (design & software deployment are in scope, hardware acquisition & physical installation are assumed external or a separate project).
* Payment gateway integration for ticket purchases (ticketing management is mentioned, but full e-commerce is not detailed).
* Integration with external payment systems.
* Deep integration with legacy non-GPS transportation systems (unless specified during requirements).
* Advanced predictive analytics beyond basic reporting mentioned in the proposal.
* Hardware procurement for buses (GPS devices).

### 4. Core Entities & Data Model (Conceptual)

This section provides a conceptual overview based on the requirements. Detailed database schema design will occur in Phase 2.

**Tables:**
1.  **Person:** ID (PK), FirstName, LastName, Birthdate, Age, Email (Unique), Password (Hashed), MobileNumber
2.  **User:** ID (FK to Person.ID, PK), ParentID (FK to User.ID, NULLABLE - for guardian/child relationship or group booking leader)
3.  **Driver:** ID (FK to Person.ID, PK), DrivingLicenseType
4.  **Ticket:** ID (PK), Price, Status, DepartureTime, ExpiryDate, ActivationDate, Type (ENUM: 'Economy', 'VIP'), Discount (Decimal, NULLABLE), Meals (Boolean, NULLABLE)
5.  **Bus:** ID (PK), BusNumber (Unique), DriverID (FK to Driver.ID), RouteID (FK to Route.ID)
6.  **Route:** ID (PK), PickupPoint, ArrivalPoint, RouteName (e.g., "CBD to Airport")
7.  **Checkpoint:** ID (PK), RouteID (FK to Route.ID), CheckpointName, OrderInRoute, EstimatedArrivalTime (based on route/bus)

**Relationships:**
* `Person` (1) -- (0/1) `User`
* `Person` (1) -- (0/1) `Driver`
* `Bus` (1) -- (1) `Driver` (a bus has one assigned driver at a time)
* `Bus` (1) -- (1) `Route` (a bus follows one specific route at a time)
* `Route` (1) -- (M) `Checkpoint`
* `Ticket` (M) -- (1) `User` (a ticket belongs to a user) - *Implicit, needs to be added to Ticket table*
* `Ticket` (M) -- (1) `Bus` (a ticket is for a specific bus journey) - *Implicit, needs to be added to Ticket table*

### 5. Technology Stack (Proposed)

* **Backend & API:**
    * **Language:** Python (Django/Flask) or Node.js (Express) or Java (Spring Boot) - *Decision during Phase 1 based on team expertise & scalability needs.*
    * **Database:** PostgreSQL (for robust relational data, scalability, and integrity) or MongoDB (for flexibility if data model evolves rapidly, but may require more effort for relationships). *PostgreSQL preferred for this structured data model.*
    * **API Standard:** RESTful API for communication between frontends, admin, and GPS.
* **Frontend (Web):** React, Angular, or Vue.js
* **Mobile Application:** React Native (for cross-platform iOS & Android) or native development (Swift for iOS, Kotlin/Java for Android) if higher performance/specific features are critical. *React Native preferred for efficiency.*
* **GPS Integration:** Requires an API from the GPS tracking provider. Standard protocols like MQTT or HTTP/S for data transfer.
* **Cloud Platform:** Microsoft Azure (due to prior mention of Microsoft products like Entra).
    * Azure App Services/VMs for backend.
    * Azure SQL Database/Cosmos DB for database.
    * Azure Blob Storage for file storage (e.g., bus images).
    * Azure DevOps for CI/CD.
    * Azure Maps for mapping integration (optional, but useful for displaying routes/buses).
* **Version Control:** Git (GitHub/Azure DevOps Repos).

### 6. Project Phases & Deliverables

Based on the provided implementation plan, expanded with technical detail.

**Phase 1: Requirements Gathering & Planning (Weeks 1-4)**

* **Activities:**
    * Kick-off Meeting: Introduce team, set expectations.
    * Detailed Stakeholder Meetings: Deep dive into all feature requirements with transportation authority, station staff, potential users. Refine user stories.
    * Technical Feasibility Study: Assess integration complexity with existing systems (especially GPS providers), identify potential challenges.
    * Technology Stack Finalization: Select specific frameworks, databases, and cloud services.
    * High-Level System Architecture Design: Conceptual layout of components, data flow.
    * Project Plan Refinement: Detailed task breakdown, resource allocation, risk assessment, precise timeline.
    * Resource Procurement: Identify and onboard specialized personnel if needed (e.g., mobile dev, UX/UI).
* **Deliverables:**
    * Detailed Requirements Document (Functional & Non-Functional)
    * Finalized Technology Stack Document
    * High-Level System Architecture Diagram
    * Project Schedule (Gantt Chart/Kanban Board)
    * Risk Register

**Phase 2: System Design & Development (Weeks 5-20)**

* **Activities:**
    * **Detailed Design:**
        * Database Schema Design (ERD).
        * API Specifications (endpoints, request/response formats).
        * UI/UX Design for Web, Mobile, and Admin interfaces (wireframes, mockups, prototypes).
        * Security Design (authentication, authorization, data encryption).
        * Scalability Design.
    * **Backend Development:**
        * Database setup and schema implementation.
        * Core API development (Persons, Users, Drivers, Tickets, Buses, Routes, Checkpoints).
        * Authentication and Authorization modules.
        * Admin API endpoints.
    * **Frontend Development (Web & Mobile):**
        * Development of passenger-facing website (timetables, route planning, real-time display).
        * Development of mobile application (timetables, real-time, notifications, favorites).
        * Integration with backend APIs.
    * **Admin Interface Development:**
        * Web-based portal development (schedule management, route updates, monitoring).
        * Analytical tools integration.
    * **GPS Integration Development:**
        * API client development for selected GPS provider(s).
        * Data ingestion and processing logic for real-time bus locations and ETAs.
        * Integration with backend for data persistence and exposure.
    * **Continuous Integration/Continuous Deployment (CI/CD) Setup:** Implement automated build, test, and deployment pipelines.
* **Deliverables:**
    * Detailed Database Schema (ERD)
    * API Documentation
    * UI/UX Design Mockups & Prototypes
    * Developed Backend APIs (tested)
    * Developed Web Frontend (tested)
    * Developed Mobile Application (tested)
    * Developed Admin Interface (tested)
    * GPS Integration Module (tested)
    * CI/CD Pipelines Configured

**Phase 3: Testing & Quality Assurance (Weeks 21-26)**

* **Activities:**
    * **Unit Testing:** (Ongoing during development)
    * **Integration Testing:** Verify communication between all system components (backend, frontends, GPS).
    * **System Testing:** End-to-end testing of all features against requirements.
    * **Performance Testing:** Load testing, stress testing to ensure scalability.
    * **Security Testing:** Vulnerability assessment, penetration testing.
    * **User Acceptance Testing (UAT):**
        * Pilot group (passengers and staff) tests the system in a controlled environment.
        * Gather feedback and iterate on minor adjustments.
    * **Bug Fixing & Refinement:** Address all identified issues.
* **Deliverables:**
    * Test Cases and Results Documentation
    * Performance Test Reports
    * Security Audit Report
    * UAT Feedback Log and Resolution Plan
    * Finalized, Tested System Build

**Phase 4: Deployment & Training (Weeks 27-30)**

* **Activities:**
    * **Production Environment Setup:** Provision cloud resources, configure networking, security groups.
    * **System Deployment:** Deploy Backend, Web Frontend, Admin Interface to production environment.
    * **Mobile App Submission:** Submit mobile application to Apple App Store and Google Play Store.
    * **Data Migration (if applicable):** Migrate any existing schedule data from legacy systems.
    * **Training Material Development:** Create user manuals, admin guides, FAQs.
    * **Training Sessions:** Conduct workshops for station staff and administrators on using the BTIS admin interface and understanding system capabilities.
* **Deliverables:**
    * Deployed BTIS Backend, Web, Admin on Production Environment
    * Mobile Apps Submitted to App Stores
    * Training Materials (User Manuals, Admin Guides)
    * Completed Training Sessions

**Phase 5: Go-Live & Support (Weeks 31 onwards)**

* **Activities:**
    * **Official Launch:** Announce system availability to the public.
    * **Monitoring:** Continuous monitoring of system performance, uptime, and user feedback.
    * **Post-Launch Support:** Dedicated support team for user inquiries and technical issues.
    * **Bug Fixes & Patches:** Address any critical issues post-launch.
    * **Feedback Collection:** Establish channels for ongoing user and staff feedback.
    * **Maintenance:** Regular system updates, security patches, database backups.
    * **Phase 2 Planning (Future Enhancements):** Begin planning for future features based on roadmap and user feedback.
* **Deliverables:**
    * Live BTIS Accessible to Public
    * System Monitoring Dashboards
    * Support Channels Established
    * Regular Maintenance Reports

### 7. High-Level Timeline (Approximate)

* **Phase 1: Requirements & Planning:** 4 Weeks
* **Phase 2: Design & Development:** 16 Weeks
* **Phase 3: Testing & QA:** 6 Weeks
* **Phase 4: Deployment & Training:** 4 Weeks
* **Phase 5: Go-Live & Support:** Ongoing

**Total Project Duration (Initial Setup): ~30-31 Weeks (approx. 7-8 months)**

### 8. Resource Requirements

* **Project Management:** 1 Project Manager
* **Business Analysis/Requirements:** 1 Business Analyst
* **UI/UX Design:** 1 UI/UX Designer
* **Backend Development:** 2-3 Backend Developers
* **Frontend Development (Web):** 1-2 Frontend Developers
* **Mobile Application Development:** 1-2 Mobile Developers (or 1-2 React Native Devs if cross-platform)
* **Database Administration/Engineering:** 1 Database Specialist (part-time/consultant)
* **QA/Testing:** 1-2 QA Engineers
* **DevOps/Cloud Engineering:** 1 DevOps Engineer
* **Technical Writing/Training:** 1 Technical Writer (part-time)

### 9. Risk Management (Initial Assessment)

* **Integration with GPS systems:** Complexity, API availability, data format consistency.
    * **Mitigation:** Early engagement with GPS providers, thorough API documentation review, robust error handling in integration module.
* **Data Accuracy & Real-time Updates:** Ensuring the accuracy and timeliness of bus location data.
    * **Mitigation:** Redundant data feeds, robust validation, clear data refresh rates.
* **User Adoption (Mobile App/Web):** Passengers not using the new system.
    * **Mitigation:** Intuitive UI/UX, marketing/awareness campaigns, clear benefits communication, feedback mechanisms.
* **Scalability Challenges:** System performance degradation with high user load.
    * **Mitigation:** Scalable cloud architecture (e.g., Azure App Services, serverless functions), performance testing, caching strategies.
* **Security Vulnerabilities:** Data breaches, unauthorized access.
    * **Mitigation:** Security by design, regular security audits, strong authentication, data encryption, input validation.
* **Scope Creep:** Additional features requested during development.
    * **Mitigation:** Strict change control process, clear scope definition in Phase 1.
* **Regulatory Compliance:** Ensuring adherence to transportation regulations, data privacy laws (e.g., POPIA in SA).
    * **Mitigation:** Early legal review, regular consultations with relevant authorities, data privacy policy built into design.

### 10. Conclusion

The Bus Timetable Information System (BTIS) represents a significant modernization effort that will greatly benefit both commuters and the transportation authority. This project plan provides a robust framework for its successful design and implementation. By adhering to the outlined phases, leveraging appropriate technologies, and managing risks effectively, we can deliver a reliable, user-friendly, and efficient system that transforms the public transportation experience.