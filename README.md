## 1. Project Title

GoAero - Flight Booking Management System

## 2. Description

GoAero is a Java-based desktop flight booking and management system built with a rich Swing user interface. It supports three primary roles—passenger (user), administrator, and flight owner (airline company)—to cover end-to-end flight reservation workflows. The application manages airports, flights, airlines, and bookings with persistent storage in a MySQL database.

## 3. Key Features

- **User registration and authentication**
  - Passenger sign-up and login with basic password validation
  - Separate login tabs for passengers, administrators, and airline (flight owner) users
  - Session management via a centralized `SessionManager` model
- **Flight search and booking**
  - Search flights by departure and destination airports and date (`SearchFlights`)
  - Display of airline, route, schedule, price, and available seat count
  - Booking dialog to confirm flight selection, fare, and passenger details (`FlightBookingDialog`)
  - Automatic PNR (Passenger Name Record) and booking reference generation (`PNRGenerator`)
- **Passenger dashboard** (`UserDashboard`)
  - Central hub for logged-in users to:
    - Search and book flights
    - View booking history and booking details (`BookingHistory`, `BookingDetailsDialog`)
    - Manage personal profile information (`UserProfileDialog`)
- **Administrator dashboard** (`AdminDashboard`)
  - Tabbed management panels for:
    - **Users** – view and manage registered passengers (`UserManagementPanel`)
    - **Airports** – maintain airport codes, names, and cities (`AirportManagementPanel`)
    - **Flight owners (airlines)** – manage airline companies (`FlightOwnerManagementPanel`, `AdminFlightOwnerDialog`)
    - **Flights** – create and maintain flight schedules, capacity, and pricing (`FlightManagementPanel`, `AdminFlightDialog`)
    - **Bookings** – view and manage all system bookings (`BookingManagementPanel`)
    - **Reports** – aggregated statistics on users, airlines, flights, bookings, and revenue (`ReportsPanel`)
- **Flight owner (airline) capabilities** (`FlightOwnerDashboard`)
  - Airline-specific portal for logged-in flight owners
  - Manage their own flights and schedules (`OwnerFlightManagementPanel`, `OwnerFlightDialog`)
  - View booking statistics and analytics for their airline (`OwnerBookingStatsPanel`)
  - Manage airline profile information (`OwnerProfilePanel`)
- **Database integration for data persistence**
  - Relational persistence using **MySQL** via JDBC (`DBConnection`)
  - DAO layer for CRUD operations on core entities (users, admins, airports, flights, flight owners, bookings)
- **Utility and validation features**
  - Password utility with strength checking and verification (`PasswordUtil`)
  - PNR and booking reference generation and validation (`PNRGenerator`)
  - Common validation helpers (`ValidationUtil`)
- **Modern Swing-based UI**
  - Consistent color scheme, tabbed layouts, and responsive dialogs
  - Separate dashboards and workflows tailored to each role (passenger, admin, flight owner)

## 4. Technologies Used

- **Programming Language**: Java (standard Java SE; uses `java.awt`, `javax.swing`, and JDBC APIs). The project is compatible with **JDK 8 or later**.
- **User Interface**: Java Swing with custom-styled components and layouts (JFrame, JPanel, JTabbedPane, JDialog, JTable, etc.).
- **Database**: MySQL (accessed via JDBC).
- **Database Driver**: MySQL Connector/J
  - Included in the repository as `src/lib/mysql-connector-j-9.3.0.jar` (also copied under `out/production/mava/lib/`).
- **Persistence Pattern**: Data Access Object (DAO) pattern for all core domain entities.
- **Project Layout / IDE**: IntelliJ IDEA project (`GoAero.iml`), plain Java module (no Maven/Gradle build file).
- **Build & Execution Tools**:
  - Java compiler (`javac`) and runtime (`java`)
  - Any modern Java IDE (IntelliJ IDEA, Eclipse, NetBeans) or VS Code with Java extensions

## 5. Prerequisites

Before running the project, ensure the following software is installed and configured:

1. **Java Development Kit (JDK)**
   - Version: **JDK 8 or higher** (11+ recommended)
   - Ensure `java` and `javac` are available on your system `PATH`.
2. **MySQL Database Server**
   - Version: **MySQL 8.0 or later** (recommended to match the MySQL Connector/J 9.x driver)
   - Running locally on `localhost:3306` or adjust configuration accordingly.
3. **Database Client / Administration Tool** (optional but recommended)
   - MySQL Workbench, DBeaver, or any other SQL client.
4. **MySQL JDBC Driver (Connector/J)**
   - Already included in this repository as `src/lib/mysql-connector-j-9.3.0.jar`.
   - If running outside an IDE, ensure this JAR is on the runtime classpath.
5. **Development Environment (optional)**
   - IntelliJ IDEA (recommended), Eclipse, NetBeans, or VS Code with Java support.

## 6. How to Run

The application is a desktop Swing application with the main entry point at `com.GoAero.main.Main`. Below are two common ways to run it.

### Method 1 – Running a Runnable JAR File

This method assumes you (or your instructor) have already packaged the application into a runnable JAR file, for example `GoAero.jar`.

1. **Ensure the database is running**
   - Start your MySQL server.
   - Confirm that the `goAero` database and required tables exist (see **Configuration** below).
2. **Place required files**
   - `GoAero.jar` – the compiled application (created via IDE or `jar` command).
   - `mysql-connector-j-9.3.0.jar` – MySQL JDBC driver (if not bundled into the JAR).
3. **Run from the command line**
   - If the JAR includes a proper `Main-Class` manifest and also bundles dependencies:
     ```bash
     java -jar GoAero.jar
     ```
   - If the MySQL driver is separate, include it on the classpath (adjust paths as needed):
     ```bash
     java -cp "GoAero.jar;lib/mysql-connector-j-9.3.0.jar" com.GoAero.main.Main
     ```
4. **Use the application**
   - The **Landing Page** window should open.
   - From there you can navigate to login and other features.

### Method 2 – Running from Source

1. **Clone or download the repository**
   ```bash
   git clone <repository-url>
   cd mava
   ```
2. **Set up the MySQL database**
   - Create a database named `goAero`:
     ```sql
     CREATE DATABASE goAero CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
     ```
   - Create tables for administrators, flight owners, users, airports, flights, and bookings.
     - The expected fields can be inferred from the model classes in:
       - `src/com/GoAero/model/*.java`
       - `src/com/GoAero/dao/*.java`
     - If you have been given an SQL schema or ER diagram as part of the assignment, use that as the source of truth.
3. **Configure database connection settings** (see **Configuration** section for details)
   - Edit `src/com/GoAero/db/DBConnection.java` if your database host, port, schema name, username, or password differ.
4. **Compile the source code (command line option)**
   - From the project root, run:
     ```bash
     javac -cp "src;src/lib/mysql-connector-j-9.3.0.jar" -d out/production/mava $(find src -name "*.java")
     ```
5. **Run the application (command line option)**
   - After compilation, run:
     ```bash
     java -cp "out/production/mava;src/lib/mysql-connector-j-9.3.0.jar" com.GoAero.main.Main
     ```
6. **Alternative: Run from an IDE**
   - Open the project in IntelliJ IDEA (or your preferred IDE).
   - Ensure the module uses a valid JDK and that `src/lib/mysql-connector-j-9.3.0.jar` is added as a project/library dependency.
   - Set the run configuration main class to `com.GoAero.main.Main`.
   - Run the configuration; the Swing UI should launch.

## 7. Configuration

### Database Connection

The database connection logic is centralized in `src/com/GoAero/db/DBConnection.java`:

- Default configuration (as provided in the code):
  - **URL**: `jdbc:mysql://localhost:3306/goAero`
  - **Username**: `root`
  - **Password**: `QWERTY`

To adapt the application to your environment:

1. Open `DBConnection.java`.
2. Update the following constants to match your MySQL setup:
   - `DB_URL` – change host, port, or database name if needed.
   - `USER` – your MySQL username.
   - `PASS` – your MySQL password.
3. Rebuild and rerun the application.

> **Security note (for academic awareness):**
> The current implementation stores database credentials and user passwords in plain text. This is acceptable for coursework and local experimentation but **not** for production systems. In a real deployment, use environment variables or configuration files for credentials and hash passwords securely.

### Environment Variables (Optional Enhancement)

The project does **not** currently read configuration from environment variables. However, for more advanced setups, you may choose to:

- Store `DB_URL`, `USER`, and `PASS` in environment variables.
- Modify `DBConnection.getConnection()` to read from `System.getenv()` instead of hard-coded constants.

## 8. Project Structure

High-level layout of the main project directories:

- `src/com/GoAero/main/`
  - Entry point for the application (`Main.java`), which launches the Swing `LandingPage`.
- `src/com/GoAero/ui/`
  - All Swing user interface classes:
    - Landing page, login screen, and registration dialogs
    - Dashboards for passenger, admin, and flight owner roles
    - Management panels for users, airports, flights, owners, and bookings
    - Search, booking, history, reporting, and analytics screens
- `src/com/GoAero/model/`
  - Domain model classes (POJOs) for `User`, `Admin`, `Airport`, `Flight`, `FlightOwner`, `Booking`, and `SessionManager`.
- `src/com/GoAero/dao/`
  - Data Access Objects that encapsulate all database operations for the models.
  - `BaseDAO` provides shared JDBC helper logic; specific DAOs (e.g., `UserDAO`, `FlightDAO`, `BookingDAO`) implement entity-specific queries.
- `src/com/GoAero/db/`
  - Database connection utility (`DBConnection`) that provides a single method to obtain a JDBC `Connection`.
- `src/com/GoAero/util/`
  - Utility classes for password handling, validation, and PNR/booking reference generation.
- `src/lib/`
  - Third-party libraries required by the project (currently MySQL Connector/J).
- `out/production/mava/`
  - Compiled `.class` files generated by the IDE or `javac` command.

## 9. Screenshots (Optional)

You may include screenshots here to illustrate the user interface. For example:

- Landing page screenshot
- Login screen showing the three roles (Passenger, Admin, Airline)
- Admin dashboard with management tabs
- Passenger dashboard with search and booking views
- Flight owner analytics dashboard

Add images to a `docs/` or `screenshots/` folder and reference them using Markdown, for example:

```markdown
![Admin Dashboard](docs/admin-dashboard.png)
```

## 10. Project Status

This project is **completed for academic submission** as a fully functional prototype of a flight booking management system. While it demonstrates key concepts such as multi-role access, Swing UI design, DAO-based persistence, and basic analytics, it is **not hardened for production use** (e.g., credentials and passwords are not secured, and no automated test suite is provided).
