# RailLink - Train Scheduling and Booking System

A complete Java Spring Boot application for a web-based train scheduling and booking system.

## Features

- **User Management**: Registration and authentication with role-based access control
- **Train Management**: CRUD operations for trains, stations, routes, and schedules
- **Booking System**: Seat booking with availability checking
- **Ticket Generation**: PDF ticket generation with booking details
- **Admin Panel**: Administrative interface for managing trains and schedules
- **REST API**: Complete RESTful API for all operations

## Technology Stack

- **Java 17**
- **Spring Boot 3.5.4**
- **Spring Security** for authentication and authorization
- **Spring Data JPA** for database operations
- **MySQL** as the database
- **Thymeleaf** for server-side templating
- **Maven** for build management

## Project Structure

```
src/main/java/com/raillink/
├── RailLinkApplication.java          # Main application class
├── config/
│   └── SecurityConfig.java          # Spring Security configuration
├── controller/
│   ├── AdminController.java         # Admin REST endpoints
│   ├── AuthController.java          # Authentication endpoints
│   ├── BookingController.java       # Booking REST endpoints
│   ├── HomeController.java          # Home page controller
│   └── ProfileController.java       # User profile endpoints
├── model/
│   ├── Booking.java                 # Booking entity
│   ├── Role.java                    # User roles enum
│   ├── Route.java                   # Route entity
│   ├── Schedule.java                # Schedule entity
│   ├── Station.java                 # Station entity
│   ├── Train.java                   # Train entity
│   └── User.java                    # User entity
├── repository/
│   ├── BookingRepository.java       # Booking data access
│   ├── RouteRepository.java         # Route data access
│   ├── ScheduleRepository.java      # Schedule data access
│   ├── StationRepository.java       # Station data access
│   ├── TrainRepository.java         # Train data access
│   └── UserRepository.java          # User data access
└── service/
    ├── BookingService.java          # Booking business logic
    ├── PdfService.java              # PDF generation service
    ├── RouteService.java            # Route business logic
    ├── ScheduleService.java         # Schedule business logic
    ├── StationService.java          # Station business logic
    ├── TrainService.java            # Train business logic
    ├── UserDetailsServiceImpl.java   # Spring Security user details
    └── UserService.java             # User business logic
```

## Database Schema

The application uses the following entities:

- **User**: Users with roles (PASSENGER, ADMIN, STAFF)
- **Train**: Train information with capacity and status
- **Station**: Station information with location
- **Route**: Route information with description
- **Schedule**: Train schedules with departure/arrival times
- **Booking**: User bookings with seat information

## API Endpoints

### Authentication
- `POST /register` - Register a new user
- `GET /login` - Login page
- `GET /register` - Registration page

### Admin Endpoints (ROLE_ADMIN required)
- `GET /api/admin/trains` - Get all trains
- `POST /api/admin/trains` - Create a new train
- `PUT /api/admin/trains/{id}` - Update a train
- `DELETE /api/admin/trains/{id}` - Delete a train
- `GET /api/admin/stations` - Get all stations
- `POST /api/admin/stations` - Create a new station
- `PUT /api/admin/stations/{id}` - Update a station
- `DELETE /api/admin/stations/{id}` - Delete a station
- `GET /api/admin/routes` - Get all routes
- `POST /api/admin/routes` - Create a new route
- `PUT /api/admin/routes/{id}` - Update a route
- `DELETE /api/admin/routes/{id}` - Delete a route
- `GET /api/admin/schedules` - Get all schedules
- `POST /api/admin/schedules` - Create a new schedule
- `PUT /api/admin/schedules/{id}` - Update a schedule
- `DELETE /api/admin/schedules/{id}` - Delete a schedule

### Booking Endpoints (ROLE_PASSENGER required)
- `GET /api/bookings/search` - Search for available schedules
- `POST /api/bookings/` - Create a new booking
- `DELETE /api/bookings/{id}` - Cancel a booking

### Profile Endpoints (Authenticated users)
- `GET /api/profile/my-bookings` - Get user's booking history
- `GET /api/profile/my-bookings/{id}/ticket` - Download ticket PDF

## Setup Instructions

1. **Prerequisites**
   - Java 17 or higher
   - MySQL 8.0 or higher
   - Maven 3.6 or higher

2. **Database Setup**
   - Create a MySQL database named `raillink_db`
   - Update the database password in `application.properties`

3. **Application Setup**
   ```bash
   # Clone the repository
   git clone <repository-url>
   cd raillink
   
   # Build the application
   mvn clean install
   
   # Run the application
   mvn spring-boot:run
   ```

4. **Access the Application**
   - Open your browser and navigate to `http://localhost:8080`
   - Register a new user or login with existing credentials

## Security Configuration

The application uses Spring Security with the following configuration:

- **Public Access**: `/`, `/css/**`, `/js/**`, `/register`, `/login`
- **Admin Access**: `/api/admin/**` (requires ROLE_ADMIN)
- **Passenger Access**: `/api/bookings/**` (requires ROLE_PASSENGER)
- **Authenticated Access**: All other endpoints require authentication

## Default Roles

- **ROLE_PASSENGER**: Can book tickets and view their bookings
- **ROLE_ADMIN**: Can manage trains, stations, routes, and schedules
- **ROLE_STAFF**: Staff role for future expansion

## Agile Sprint Summaries

- **Sprint 1 — Project bootstrap & authentication**
  - **Deliverables**: Spring Boot skeleton, `SecurityConfig`, role model (`PASSENGER`, `ADMIN`, `STAFF`), `AuthController`, login/register pages, DB config, `DataInitializationService`, `DbHealthController`.
  - **Activities**: Project setup, security wiring, seed data, health checks, basic docs.

- **Sprint 2 — Core domain and admin CRUD**
  - **Deliverables**: Entities (`Train`, `Station`, `Route`, `Schedule`, `User`), repositories/services, admin REST in `AdminController`, admin templates (trains/routes/stations/schedules/users), validation.
  - **Activities**: Schema modeling, CRUD endpoints, Thymeleaf forms, repo/service layering.

- **Sprint 3 — Search, booking, and tickets**
  - **Deliverables**: `BookingController`, search pages (`search-trains.html`, `search-results.html`), booking flow (`booking-form.html`), `my-bookings.html`, ticket PDF via `PdfService`, `ticket.html`.
  - **Activities**: Availability checks, transactional booking, PDF generation, UX polish.

- **Sprint 4 — Admin dashboard and user profile**
  - **Deliverables**: `AdminDashboardController` with `admin/dashboard.html`, `ProfileController` and `profile.html`.
  - **Activities**: Dashboard KPIs, profile management, navigation, access control refinements.

- **Sprint 5 — Real-time updates and staff tools**
  - **Deliverables**: `ScheduleSseController`, `ScheduleUpdateBroadcaster` (SSE), `StaffDashboardController` with `staff/dashboard.html`, `ReminderService`.
  - **Activities**: Event streaming, staff workflows, schedule update propagation, reminders.

- **Sprint 6 — Feedback, announcements, and password reset**
  - **Deliverables**: `FeedbackController` with `feedback.html`, `Announcement` domain plus admin pages (announcement form/list), `PasswordResetController` with `forgot-password.html` and `reset-password.html`, `help.html`.
  - **Activities**: User feedback loop, admin communications, secure token-based reset, documentation updates.

- **Ongoing hardening**
  - **Deliverables**: Test scaffolding (`RailLinkApplicationTests`), Maven build tasks, security tuning.
  - **Activities**: Smoke tests, dependency upgrades, minor bug fixes.

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests for new functionality
5. Submit a pull request

## License

This project is licensed under the MIT License. 