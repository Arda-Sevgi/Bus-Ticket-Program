# Nottingham Bus Ticket Booking System

A comprehensive command-line bus ticket booking system developed in Python, featuring user authentication, ticket management, and booking functionality for Nottingham's bus network.

## 📋 Table of Contents
- [Project Overview](#project-overview)
- [Features](#features)
- [System Architecture](#system-architecture)
- [Development Process](#development-process)
- [Installation & Setup](#installation--setup)
- [Usage Guide](#usage-guide)
- [Technical Implementation](#technical-implementation)
- [Known Limitations](#known-limitations)
- [Future Enhancements](#future-enhancements)
- [Acknowledgements](#acknowledgements)

## 🎯 Project Overview

This project was developed as part of an academic assessment to create a functional bus ticket booking system. The system manages user registration, authentication, ticket inventory, and booking operations through a text-based interface, with all data persisted in CSV files.

**Development Timeline**: December 2025 - January 2026 (Approximately 1 month)

## ✨ Features

### User Management
- **Dual User Types**: Separate functionality for administrators and customers
- **Customer Registration**: Complete registration system with validation
- **Secure Authentication**: Login system for both user types
- **Profile Validation**: Email, phone number, and username validation

### Booking System
- **Route Viewing**: Display all available bus routes with complete details
- **Ticket Booking**: Customer booking with quantity selection
- **Travel Date Selection**: Date validation with booking window restrictions
- **Booking History**: Customers can view their personal booking history
- **Real-time Seat Management**: Automatic seat availability updates

### Administrative Functions
- **Route Management**: Add new bus routes to the system
- **Route Updates**: Modify existing route information
- **Booking Overview**: View all customer bookings across the system
- **Inventory Control**: Manage ticket availability and pricing

### Data Validation & Restrictions
- **Username Validation**: Minimum 4 characters, uniqueness check
- **Password Security**: Minimum 6 characters with confirmation
- **Email Format**: Regex-based email validation
- **Phone Number**: UK phone number format validation (supports +44 and 0 formats)
- **Date Validation**: 
  - Prevents booking dates in the past
  - Maximum 90-day advance booking window
  - DD/MM/YYYY format enforcement
- **Time Validation**: HH:MM format for departure times
- **Numeric Validation**: Price and quantity input validation
- **Name Validation**: Alphabetic characters only for full names

## 🏗️ System Architecture

### Data Storage Structure
The system utilises three CSV files for data persistence:

1. **users.csv**
   - Stores user credentials and profile information
   - Fields: username, password, user_type, full_name, email, phone
   - Pre-configured with default admin account
   - Located in the programme's working directory

2. **tickets.csv**
   - Contains all available bus routes and ticket information
   - Fields: ticket_id, route_name, departure, destination, price, available_seats, departure_time
   - Initialised with 7 default Nottingham routes
   - Located in the programme's working directory

3. **bookings.csv**
   - Records all customer bookings
   - Fields: booking_id, username, ticket_id, route_name, booking_date, travel_date, quantity, total_price
   - Located in the programme's working directory

### File Initialisation
All CSV files are automatically created on first run if they don't exist, complete with headers and default data where applicable. Files are created in the same directory as the Python script.

## 🔨 Development Process

### Phase 1: Authentication System (Week 1)
The development began with implementing the user authentication framework:
- Created the dual-user system (admin/customer)
- Developed the login functionality with credential verification
- Established the admin account with predefined credentials
- Implemented CSV-based user storage

### Phase 2: Customer Registration (Week 1-2)
Built a comprehensive registration system with multiple validation layers:
- Username validation with uniqueness checking
- Password strength requirements and confirmation
- Email format validation using regex
- UK phone number format validation
- Full name validation excluding numeric characters

### Phase 3: Ticket Management (Week 2-3)
Developed the core ticketing functionality:
- Created ticket display system with formatted output
- Implemented ticket inventory management
- Built the booking system with seat allocation
- Developed automatic seat availability updates
- Added date and time validation for travel bookings

### Phase 4: Administrative Features (Week 3)
Added administrative capabilities:
- Route creation functionality
- Route modification system
- Comprehensive booking overview for admins
- Inventory management tools

### Phase 5: Refinement & Validation (Week 4)
Enhanced the system with robust validation and user experience improvements:
- Implemented the 90-day booking window restriction
- Added comprehensive input validation across all fields
- Created user-friendly error messages with guidance
- Implemented 'back' functionality for better navigation
- Added booking ID generation with timestamps

### Challenges Overcome

#### 1. CSV File Management and Data Consistency
**Problem**: When multiple users attempted to make bookings simultaneously, data inconsistencies and seat availability errors could occur in the CSV files.

**Solution**:
- Implemented a read-then-write approach for each booking operation
- Added error handling with try-except blocks for file operations
- Created a validation system to check seat availability before booking

**Known Limitations**:
- No file locking mechanism implemented (would require third-party libraries such as `fcntl` on Linux or `msvcrt` on Windows)
- Race conditions possible with concurrent access in multi-user scenarios
- System designed for single-user or low-traffic environments
- For production use, database migration with proper transaction support is recommended

#### 2. Date and Time Validation
**Problem**: Users entering past dates, invalid formats, or dates far in the future caused inconsistencies in the system.

**Solution**:
- Developed a comprehensive date handling system using the datetime module
- Implemented the 90-day booking window restriction
- Enforced DD/MM/YYYY format with checks to prevent different format attempts
- Compared against current date for past date validation
- Provided clear error messages and correct format examples to users (e.g., "15/03/2025")

#### 3. Balancing User Experience and Security
**Problem**: Strong validation rules could be confusing for users and might discourage them from using the system.

**Solution**:
- Wrote explanatory error messages for each validation rule
- Informed users of requirements in advance (e.g., "Password must be at least 6 characters")
- Provided the ability to go back at each stage with the 'back' command
- Showed example formats to users after failed inputs
- Designed a user-friendly interface whilst maintaining minimum security standards

#### 4. Comprehensive Input Validation
**Problem**: Different input types (email, phone, date, price, etc.) required separate validation logic, each with its own challenges.

**Solution**:
- Created dedicated validation functions for each input type
- Implemented email format validation using regex pattern matching
- Developed a flexible checking mechanism for UK phone number formats (supporting both +44 and 0 prefixes)
- Added positivity and data type checks for numeric values
- Validated username uniqueness by searching the CSV file
- Wrote specific and instructive messages for each validation error

#### 5. Booking ID Generation
**Problem**: A unique identifier needed to be generated for each booking, and these identifiers needed to be traceable.

**Solution**:
- Created unique booking IDs using timestamp-based generation
- Format: `B` + `YYYYMMDDHHMMSS` (e.g., `B20250111142530`)
- The 'B' prefix denotes 'Booking'
- Timestamp ensures uniqueness and provides chronological ordering
- Format enables easy identification of booking time

**Alternative Considered**: Including username in the ID (e.g., `username_timestamp`) was considered but not implemented to maintain consistent ID length and format.

#### 6. Efficient Data Access
**Problem**: Frequent CSV file reading could impact performance during validation operations.

**Solution**:
- Structured validation operations sequentially and efficiently
- Read CSV files only when necessary for data accuracy
- Implemented efficient loops for data searching
- Used Python's built-in CSV module for optimised file operations

**Trade-offs**:
- Prioritised data accuracy over caching (always reading fresh data from CSV)
- Suitable performance for small to medium-scale operations (hundreds of bookings)
- For large-scale deployments, database implementation would provide better performance

## 🚀 Installation & Setup

### Prerequisites
- Python 3.6 or higher
- No external dependencies required (uses standard library only)

### Installation Steps

1. Download or clone the project files
2. Ensure `bus_ticket_system.py` is in your desired directory
3. Run the programme:
```bash
python bus_ticket_system.py
```

The system will automatically create the necessary CSV files on first run in the same directory as the script.

## 📖 Usage Guide

### First Time Setup

When you first run the programme, it will create three CSV files with default data:
- An admin account (username: `admin`, password: `admin123`)
- Seven pre-configured bus routes covering major Nottingham locations
- An empty bookings file ready to record transactions

### For Customers

#### 1. Registration:
- Select option 1 from the main menu
- Provide username (minimum 4 characters)
- Create password (minimum 6 characters)
- Enter full name (alphabetic characters only)
- Provide email address (valid format required)
- Enter phone number (UK format: 0115-000-0000 or +447123456789)
- All fields include validation and helpful error messages

#### 2. Booking a Ticket:
- Login with your credentials
- View available routes with all details
- Enter the ticket ID of your chosen route
- Specify the number of tickets (quantity)
- Select travel date in DD/MM/YYYY format (within 90 days)
- Receive booking confirmation with unique booking ID

#### 3. Viewing Bookings:
- Access "View My Bookings" from the customer menu
- See all your booking history with complete details
- View booking IDs, routes, travel dates, quantities, and total prices

### For Administrators

Login with admin credentials:
- **Username**: `admin`
- **Password**: `admin123`

**Available Functions**:
- View all routes in the system with current availability
- Add new bus routes with complete details (route name, departure, destination, price, seats, time)
- Update existing route information (modify any field)
- View all customer bookings across the system for monitoring

### Navigation Tips
- Type `back` at most input prompts to return to the previous screen
- Press Enter to continue after viewing information
- Press Enter without typing to keep current values when updating routes
- The system provides clear error messages with guidance for corrections

## 🔧 Technical Implementation

### Key Technologies
- **Python 3**: Core programming language
- **CSV Module**: Data persistence and management
- **Datetime Module**: Date/time validation and booking window logic
- **Re Module**: Regular expression validation for email and phone numbers
- **Os Module**: File system operations and existence checking

### Design Patterns
- **Class-based Architecture**: Single `BusTicketSystem` class managing all functionality
- **State Management**: Current user and user type tracked throughout session
- **Validation Layer**: Dedicated methods for each validation type
- **Error Handling**: Try-catch blocks for file operations and user input

### Validation Methods
The system includes comprehensive validation functions:
- `validate_username()`: Length and availability checking
- `validate_date()`: Format, past dates, and booking window validation
- `validate_time()`: HH:MM format verification
- `validate_email()`: Regex-based email format checking
- `validate_phone()`: UK phone number format validation
- `validate_positive_number()`: Numeric value validation
- `validate_integer()`: Whole number validation with optional maximum

### Data Flow
1. User input → Validation → CSV read
2. Data processing → Update logic → CSV write
3. Confirmation message → Return to menu

### Constants and Configuration
```python
MAX_BOOKING_DAYS = 90        # Maximum advance booking window
MIN_USERNAME_LENGTH = 4      # Minimum username characters
MIN_PASSWORD_LENGTH = 6      # Minimum password characters
```

## ⚠️ Known Limitations

### Concurrency Issues
- **No File Locking**: The system lacks a file locking mechanism, making it vulnerable to race conditions when multiple users access simultaneously
- **Overselling Risk**: Two users booking the same ticket concurrently may cause overselling
- **Recommended Use**: Single-user or low-traffic scenarios only
- **Production Unsuitability**: Not suitable for high-traffic environments without database migration

### Security Concerns
- **Plain Text Passwords**: Passwords stored without encryption (acceptable for academic projects, not for production)
- **No Session Management**: No session timeout or forced logout functionality
- **No Audit Trail**: User actions are not logged for security monitoring
- **Admin Account**: Default admin credentials should be changed in production use

### Data Storage Limitations
- **CSV Constraints**: Lack of data integrity constraints and relational capabilities
- **No Backup System**: No automatic backup mechanism for data files
- **Corruption Risk**: File corruption possible with unexpected system crashes
- **Scalability**: Performance degrades with large numbers of bookings

### Validation Gaps
- **Same-Day Booking**: No time-of-day validation (can book 08:00 departure at 10:00 on same day)
- **Fixed Booking Window**: 90-day limit has no administrative override option
- **No Waiting Lists**: Cannot handle sold-out routes with waiting list functionality
- **No Booking Modifications**: Users cannot modify or cancel bookings after creation

### User Experience Limitations
- **Command-Line Only**: Text-based interface may not be intuitive for all users
- **No Digital Tickets**: No confirmation emails or printable/digital tickets
- **Limited Feedback**: No booking reminders or travel notifications
- **Navigation**: Requires manual typing; no mouse or graphical interaction

### Functional Limitations
- **No Payment Processing**: Booking completes without actual payment verification
- **No Refund System**: Cannot process cancellations or refunds
- **Fixed Routes**: No dynamic route planning or suggestions
- **No User Profiles**: Limited user information and no preference storage

## 🔮 Future Enhancements

### High Priority Improvements

#### Security & Data Management
- **Password Hashing**: Implement bcrypt or Argon2 for secure password storage
- **Database Migration**: Move from CSV to SQLite or PostgreSQL for:
  - Transaction support and ACID compliance
  - Better concurrency handling
  - Data integrity constraints
  - Improved query performance
- **Session Management**: Add proper session handling with timeout mechanisms
- **Rate Limiting**: Prevent brute force login attempts
- **Audit Logging**: Track all system operations for security and debugging

#### Core Functionality
- **Booking Cancellation**: Allow users to cancel bookings with configurable refund policies
- **Booking Modification**: Enable date/time changes with seat availability checks
- **Payment Integration**: Add Stripe or PayPal for actual payment processing
- **Email Notifications**: Send booking confirmations, reminders, and updates
- **Digital Tickets**: Generate QR codes or printable tickets

### Medium Priority Improvements

#### User Experience
- **Graphical User Interface**: Implement GUI using:
  - Tkinter for desktop application
  - PyQt for advanced desktop features
  - Flask/Django for web-based interface
- **Search & Filter**: Advanced route search with:
  - Departure/destination filtering
  - Price range filtering
  - Time slot preferences
  - Date range browsing
- **User Profiles**: Extended user information including:
  - Saved payment methods
  - Favourite routes
  - Travel history analytics

#### Administrative Features
- **Analytics Dashboard**: 
  - Revenue reports
  - Popular routes analysis
  - Peak time identification
  - Customer demographics
- **Bulk Operations**: Import/export routes and bookings in batch
- **Dynamic Pricing**: Implement surge pricing or discount schemes
- **Route Optimisation**: Suggest optimal routes based on demand

### Lower Priority Enhancements

#### System Enhancements
- **Multi-language Support**: Internationalisation capabilities (Welsh for local accessibility)
- **Mobile Application**: Dedicated iOS/Android apps
- **Real-time Updates**: WebSocket integration for live seat availability
- **SMS Notifications**: Text message confirmations and reminders
- **Accessibility Features**: Screen reader support, keyboard navigation
- **Dark Mode**: Visual theme options

#### Advanced Features
- **Loyalty Programme**: Points system for frequent travellers
- **Group Bookings**: Special handling for large parties
- **Season Tickets**: Recurring booking management
- **API Development**: REST API for third-party integrations
- **Waiting Lists**: Automatic notification when sold-out tickets become available

## 📝 Acknowledgements

This project was developed as part of an academic assessment, utilising knowledge gained from previous programming projects and coursework. The system demonstrates practical application of:
- Object-oriented programming principles
- File handling and data persistence
- Input validation and error handling
- User authentication systems


---

**Version**: 1.0  
**Last Updated**: 12 February 2026  
**Developed By**: Arda Sevgi  
