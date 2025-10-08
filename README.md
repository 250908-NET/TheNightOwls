# Fadebook - Barbershop Appointment Management System

A full-stack barbershop appointment management system built with .NET 9.0 Web API backend and Next.js frontend.

## 📋 Table of Contents

- [Team Members](#team-members)
- [Project Overview](#project-overview)
- [Technology Stack](#technology-stack)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [Database Setup with Docker](#database-setup-with-docker)
  - [Running the Backend API](#running-the-backend-api)
  - [Running the Frontend](#running-the-frontend)
- [API Documentation](#api-documentation)
- [Testing](#testing)
- [CI/CD Automation](#cicd-automation)
- [Project Requirements](#project-requirements)
- [Features](#features)

## 👥 Team Members

* Christian Brewer
* Victor Torres
* Charles Trangay
* Dean Gelbaum
* Jeremiah Ogembo
* [Muhiddin Kurbonov](https://github.com/muhiddinkurbonov)

## 📖 Project Overview

Fadebook is a comprehensive barbershop management system that allows customers to book appointments with barbers for various services. The system manages barbers, customers, services, and appointments through a RESTful API, with a modern frontend interface built using Next.js.

This project fulfills all requirements for Revature's P2 - Web Application project, including:
- Service-oriented architecture
- MS SQL Server Express in Docker
- 3rd normal form database with many-to-many relationships
- Entity Framework Code-First migrations
- Repository pattern implementation
- Comprehensive unit testing (50%+ coverage)
- REST API principles
- React-based frontend with Next.js

## 🛠️ Technology Stack

### Backend
- **Framework**: .NET 9.0
- **API**: ASP.NET Core Web API
- **Database**: SQL Server Express (Dockerized)
- **ORM**: Entity Framework Core 9.0
- **Authentication**: JWT Bearer (infrastructure in place, currently disabled)
- **Logging**: Serilog (console + file logging)
- **Mapping**: AutoMapper
- **Documentation**: Swagger/OpenAPI
- **Testing**: xUnit, Moq, FluentAssertions
- **Coverage**: Coverlet

### Frontend
- **Framework**: Next.js 15.5.4
- **Language**: TypeScript 5
- **UI Library**: React 19.1.0
- **Styling**: Tailwind CSS 4

### DevOps
- **Database**: Docker + Docker Compose
- **Containerization**: MSSQL Server Express 2022
- **CI/CD**: Custom bash automation scripts

## 📁 Project Structure

```
TheNightOwls/
├── api/                          # .NET Web API Backend
│   ├── Controllers/              # API endpoint controllers
│   ├── DTO/                      # Data Transfer Objects
│   ├── Models/                   # Database entity models
│   ├── Services/                 # Business logic layer
│   ├── Repositories/             # Data access layer (Repository pattern)
│   ├── DB/                       # DbContext and configurations
│   ├── Mapping/                  # AutoMapper profiles
│   ├── Middleware/               # Custom middleware (logging, etc.)
│   ├── Exceptions/               # Custom exceptions
│   ├── Migrations/               # EF Core migrations
│   └── Properties/               # Launch settings
│
├── Api.Tests/                    # Unit test project (xUnit)
│   ├── Repositories/             # Repository tests
│   ├── Services/                 # Service layer tests
│   └── TestUtilities/            # Test helpers and utilities
│
├── fadebook-njs/                 # Next.js Frontend
│   ├── src/
│   │   └── app/                  # Next.js app directory
│   ├── public/                   # Static assets
│   └── package.json              # Frontend dependencies
│
├── Diagrams/                     # Project documentation
│   ├── ERD-and-USER-STORIES.svg  # Entity Relationship Diagram
│   ├── Page Wire Diagram.svg     # UI wireframes
│   ├── Webpage Flow Diagram      # Application flow
│   └── Mockups/                  # UI mockups
│
├── TestResults/                  # Test coverage reports
├── docker-compose.yml            # Database container configuration
├── .env.example                  # Environment variables template
├── cicd.sh                       # CI/CD automation script
├── start-db.sh                   # Database startup script
├── api.sln                       # Backend solution file
└── TheNightOwls.sln              # Complete solution file
```

## 🚀 Getting Started

### Prerequisites

- **.NET 9.0 SDK** - [Download](https://dotnet.microsoft.com/download/dotnet/9.0)
- **Docker Desktop** - [Download](https://www.docker.com/products/docker-desktop)
- **Node.js 20+** - [Download](https://nodejs.org/) (for frontend)
- **Git** - [Download](https://git-scm.com/)

### Database Setup with Docker

The project uses **MSSQL Server Express 2022** running in a Docker container.

#### 1. Configure Environment Variables

Copy the example environment file:

```bash
cp .env.example .env
```

The `.env` file contains:
```env
CONNECTION_STRING="Server=127.0.0.1,1433;Database=NightOwlsDb;User Id=sa;Password=YourStrongPassword123!;TrustServerCertificate=True"
MSSQL_SA_PASSWORD="YourStrongPassword123!"
```

**Password Requirements:**
- Minimum 8 characters
- Contains uppercase letters
- Contains lowercase letters
- Contains numbers
- Contains special characters (!, @, #, $, etc.)

#### 2. Start the Database

**Option A: Using the automated script (Recommended)**

```bash
./start-db.sh
```

This script will:
- Validate environment variables
- Start the MSSQL container
- Wait for SQL Server to be ready
- Apply EF Core migrations automatically

**Option B: Manual setup**

```bash
# Start Docker container
docker-compose up -d

# Wait for SQL Server to start (about 10-15 seconds)
docker logs mssql-express

# Apply migrations
cd api
dotnet ef database update
```

#### 3. Verify Database

Check if the container is running:

```bash
docker ps
```

You should see `mssql-express` container running on port `1433`.

#### Docker Commands Reference

```bash
# Start database
docker-compose up -d

# Stop database
docker-compose down

# View logs
docker-compose logs sqlserver

# Stop and remove all data (WARNING: destructive)
docker-compose down -v

# Restart database
docker-compose restart sqlserver

# Access SQL Server CLI
docker exec -it mssql-express /opt/mssql-tools/bin/sqlcmd -S localhost -U sa -P 'YourPassword'
```

### Running the Backend API

#### Option A: Using the CI/CD script

```bash
# Build and run
./cicd.sh run

# Or run all tests first
./cicd.sh all
```

#### Option B: Manual dotnet commands

```bash
cd api
dotnet restore
dotnet build
dotnet run
```

The API will start on `https://localhost:5001` (HTTPS) or `http://localhost:5000` (HTTP).

**Access Swagger UI:** Navigate to `https://localhost:5001/swagger`

### Running the Frontend

```bash
cd fadebook-njs
npm install
npm run dev
```

The frontend will start on `http://localhost:3000`

### Database Seeding

The application automatically seeds the database with sample data on first run:

**Services:**
- Haircut - $20.00
- Haircut and Beard - $15.00
- Shampoo - $18.00
- The Works - $25.00

**Barbers:**
- Dean (username: `dean-the-machine`) - Specialty: Beards
- Victor (username: `v-for-victor`) - Specialty: Styled Hair Cuts
- Charles (username: `charles-xavier`) - Specialty: The Works

**Sample Customer:**
- Muhiddin (username: `m-kurbonov`)

## 📚 API Documentation

### Base URL
```
https://localhost:5001/api
```

### Endpoints

#### **Appointment Management** (`/api/appointment`)

| Method | Endpoint | Description | Request Body | Response |
|--------|----------|-------------|--------------|----------|
| POST | `/api/appointment` | Create a new appointment | `AppointmentDto` | 201 Created |
| GET | `/api/appointment/{id}` | Get appointment by ID | - | `AppointmentDto` |
| PUT | `/api/appointment/{id}` | Update an appointment | `AppointmentDto` | 200 OK |
| DELETE | `/api/appointment/{id}` | Delete an appointment | - | 204 No Content |
| GET | `/api/appointment/by-date?date={date}` | Get appointments by date | Query: `date` (YYYY-MM-DD) | `List<AppointmentDto>` |
| GET | `/api/appointment/by-username/{username}` | Get customer appointments | - | `List<AppointmentDto>` |

#### **Barber Management** (`/api/barber`)

| Method | Endpoint | Description | Request Body | Response |
|--------|----------|-------------|--------------|----------|
| GET | `/api/barber` | Get all barbers | - | `List<BarberDto>` |
| GET | `/api/barber/{id}` | Get barber by ID | - | `BarberDto` |
| POST | `/api/barber` | Create a new barber | `BarberDto` | 201 Created |
| PUT | `/api/barber/{id}` | Update a barber | `BarberDto` | 200 OK |
| DELETE | `/api/barber/{id}` | Delete a barber | - | 204 No Content |
| PUT | `/api/barber/{id}/services` | Update barber's services | `List<int>` (service IDs) | 204 No Content |

#### **Customer Account** (`/api/customeraccount`)

| Method | Endpoint | Description | Request Body | Response |
|--------|----------|-------------|--------------|----------|
| GET | `/api/customeraccount` | Check if username exists | `NameDTO` | `IsTakenDTO` |

#### **Customer Appointment** (`/api/customerappointment`)

| Method | Endpoint | Description | Request Body | Response |
|--------|----------|-------------|--------------|----------|
| GET | `/customer/{id}` | Get customer by ID | - | `CustomerDto` |
| GET | `/api/customerappointment/services` | Get all available services | - | `List<ServiceDto>` |
| GET | `/api/customerappointment/barbers-by-service/{serviceId}` | Get barbers by service | - | `List<BarberDto>` |
| POST | `/api/customerappointment/request-appointment` | Create appointment request | `AppointmentRequestDto` | `AppointmentDto` |

### Data Models

#### AppointmentDto
```json
{
  "appointmentId": 0,
  "status": "Pending",
  "customerId": 1,
  "serviceId": 1,
  "barberId": 1,
  "appointmentDate": "2025-10-07T10:00:00Z"
}
```

**Valid Status Values**: `Pending`, `Completed`, `Cancelled`, `Expired`

#### BarberDto
```json
{
  "barberId": 0,
  "username": "string",
  "name": "string",
  "specialty": "string",
  "contactInfo": "string"
}
```

#### CustomerDto
```json
{
  "customerId": 0,
  "username": "string",
  "name": "string",
  "contactInfo": "string"
}
```

#### ServiceDto
```json
{
  "serviceId": 0,
  "serviceName": "string",
  "servicePrice": 0.0
}
```

## 🧪 Testing

### Running Tests

The project includes comprehensive unit tests using **xUnit**, **Moq**, and **FluentAssertions**.

**Using the CI/CD script:**

```bash
# Run tests and generate HTML coverage report
./cicd.sh test

# Run full CI pipeline (clean, build, test, report)
./cicd.sh all

# Regenerate HTML report from existing coverage data
./cicd.sh report
```

**Manual testing:**

```bash
cd Api.Tests
dotnet test --configuration Release --logger "trx" --collect:"XPlat Code Coverage"
```

### Test Coverage

- **Current Coverage**: 50%+ (meeting project requirements)
- **Test Categories**:
  - Repository layer tests (14 test files)
  - Service layer tests
  - Integration tests
- **Coverage Reports**: Available in `TestResults/html/index.html`

### Test Technologies

- **xUnit** - Test framework
- **Moq** - Mocking framework
- **FluentAssertions** - Assertion library
- **Coverlet** - Code coverage tool
- **InMemory/SQLite** - Test databases

## 🔄 CI/CD Automation

The project includes a comprehensive bash automation script (`cicd.sh`) for build, test, and deployment automation.

### Available Commands

```bash
./cicd.sh {command}
```

| Command | Description |
|---------|-------------|
| `all` | Run clean, restore, build, and test with coverage report |
| `build` | Restore dependencies and build the solution (Release) |
| `test` | Run tests with Coverlet and generate HTML report |
| `test2` | Run tests with runsettings (alternative method) |
| `run` | Build and start the Web API application |
| `clean` | Clean project build artifacts |
| `setup` | Create solution file and link projects |
| `report` | Regenerate HTML coverage report from existing data |
| `help` | Show help message |

### Examples

```bash
# First-time setup
./cicd.sh setup

# Full CI pipeline
./cicd.sh all

# Run tests with coverage
./cicd.sh test

# Start the API
./cicd.sh run
```

### Features

- ✅ Cross-platform support (Windows/Linux/macOS)
- ✅ Automatic path conversion for Windows environments
- ✅ Test result consolidation
- ✅ HTML coverage report generation
- ✅ Automatic browser opening for reports
- ✅ Clean output directory management
- ✅ Colored console output
- ✅ Environment validation

## 📋 Project Requirements

This project fulfills all P2 requirements:

### Application Architecture ✅
- [x] Code pushed to cohort organization Git repository
- [x] Application builds and runs successfully
- [x] Service Oriented Architecture (Controllers → Services → Repositories)

### SQL Database ✅
- [x] MS SQL Server Express
- [x] Running in Docker container
- [x] 3rd Normal Form database design
- [x] Many-to-many relationships (Barber ↔ Service via BarberService)
- [x] Entity Framework Code-First migrations

### API ✅
- [x] Written in C# for .NET 9.0
- [x] ASP.NET Core framework
- [x] RESTful principles
- [x] Repository pattern with interfaces
- [x] Entity Framework Core for data persistence
- [x] Full CRUD operations
- [x] Exception handling
- [x] Serilog logging
- [x] Data validation with DataAnnotations
- [x] 50%+ unit test coverage

### Frontend ✅
- [x] Next.js framework (React-based)
- [x] TypeScript with HTML5 & CSS3
- [x] Interacts with API for data persistence
- [x] React components with hooks (useState, Context)
- [x] Component hierarchy (parent-child relationships)
- [x] Props implementation

### Documentation ✅
- [x] Project description
- [x] User stories (see `Diagrams/ERD-and-USER-STORIES.svg`)
- [x] UI wireframes (see `Diagrams/Mockups/`)
- [x] ERD diagram (see `Diagrams/ERD-and-USER-STORIES.svg`)
- [x] Unit test coverage reporting
- [x] API endpoint documentation (this README + Swagger)

## ✨ Features

### Backend Features
- RESTful API design following best practices
- Automatic database seeding with sample data
- Interactive Swagger/OpenAPI documentation
- Request/response logging middleware
- Comprehensive error handling with descriptive messages
- AutoMapper for DTO-Model mapping
- Repository pattern for clean data access
- Service layer for business logic separation
- Transaction support for data consistency
- JWT infrastructure (ready for authentication)

### Database Features
- Many-to-many relationship (Barber ↔ Service)
- Foreign key relationships (Appointment → Customer, Barber, Service)
- 3rd Normal Form normalization
- Code-First migrations with EF Core
- Persistent data storage with Docker volumes

### Testing Features
- xUnit test framework
- Moq for mocking dependencies
- FluentAssertions for readable test assertions
- InMemory and SQLite test databases
- Code coverage reporting (Coverlet)
- HTML coverage reports (ReportGenerator)
- Automated test execution via CI/CD script

### DevOps Features
- Dockerized MSSQL Server Express
- Automated database startup script
- Environment variable configuration
- CI/CD automation with bash scripts
- Cross-platform build support
- Test result consolidation and reporting

## 🔐 Environment Configuration

The project uses environment variables for configuration:

**Required variables in `.env`:**
```env
CONNECTION_STRING="Server=127.0.0.1,1433;Database=NightOwlsDb;User Id=sa;Password=YourPassword;TrustServerCertificate=True"
MSSQL_SA_PASSWORD="YourPassword"
```

**Configuration in `appsettings.json`:**
- Logging levels (Serilog)
- Log file paths
- Allowed hosts
- JWT settings (commented out, ready for implementation)

## 📊 Database Schema

The database includes the following tables:

- **Customer** - Customer information
- **Barber** - Barber profiles with specialties
- **Service** - Available services with pricing
- **BarberService** - Many-to-many junction table
- **Appointment** - Appointment bookings with status tracking

See `Diagrams/ERD-and-USER-STORIES.svg` for the complete Entity Relationship Diagram.

## 🎨 UI Mockups

UI mockups are available in `Diagrams/Mockups/`:
- Welcome Page
- Sign In Page
- Create Account
- Appointment Manager
- Create Appointment Page
- Barber Manager
- Confirmation Page

## 🐛 Troubleshooting

### Database Connection Issues

1. Ensure Docker is running:
   ```bash
   docker ps
   ```

2. Check SQL Server logs:
   ```bash
   docker logs mssql-express
   ```

3. Verify environment variables:
   ```bash
   cat .env
   ```

### Migration Issues

```bash
# Remove all migrations
rm -rf api/Migrations

# Create new initial migration
cd api
dotnet ef migrations add InitialCreate

# Apply migration
dotnet ef database update
```

### Test Coverage Issues

```bash
# Install ReportGenerator globally
dotnet tool install -g dotnet-reportgenerator-globaltool

# Regenerate report
./cicd.sh report
```

## 📝 License

This project is part of Revature's training program.

## 🙏 Acknowledgments

- **[Richard Hawkins](https://github.com/HawkinsR)** - Our Revature instructor and trainer who oversees this project and provides invaluable guidance throughout our learning journey
- **Team members** - For collaborative development and dedication to building this project
- **Open source community** - For the amazing tools and frameworks that made this possible

---

**For more information, see:**
- Frontend README: `fadebook-njs/README.md`
- Project Requirements: `Project2.md`
- API Error Documentation: `api/errors.md`
