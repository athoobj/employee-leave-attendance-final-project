# Employee Leave and Attendance System - Angular Front End

## Project Overview

This project is the Angular front end for the **Employee Leave and Attendance System** developed as part of the COOP Training Final Project.

The Angular application connects to the existing Spring Boot REST API and provides a complete user interface for authentication, role-based access, CRUD operations, leave request workflow, attendance operations, search, filtering, sorting, pagination, validation, and dashboard reporting.

The application uses the existing Spring Boot backend and the existing project entities. No mock services, static arrays, or hardcoded application data are used instead of API calls.

All application data is loaded from the Spring Boot backend using Angular `HttpClient` services.

---

# Assigned Topic

**Employee Leave and Attendance System**

## Main Module

- Leave Requests

## Additional Modules

- Attendance
- Employees
- Leave Types

## Main Leave Request Workflow

- Submit Leave Request
- Approve Leave Request
- Reject Leave Request
- Cancel Leave Request

## Attendance Workflow

- Check In
- Check Out
- Mark Absent

---

# Technologies Used

## Front End

- Angular
- TypeScript
- HTML
- CSS
- Bootstrap
- Angular Reactive Forms
- Angular Router
- HttpClient
- RxJS

## Back End

- Spring Boot
- Spring Security
- Spring Data JPA
- Hibernate
- MySQL
- Maven

## Container Runtime

- Podman CLI
- Spring Boot container
- MySQL container
- Podman network

---

# Application Architecture

The application follows the required architecture:

```text
Angular Component
        ↓
Angular Service (HttpClient)
        ↓
Spring Boot REST Controller
        ↓
Service Layer
        ↓
Repository
        ↓
MySQL (Podman)
```

Angular components do not call `HttpClient` directly.

All backend communication is performed through Angular services.

---

# Project Structure

The final project contains:

```text
Employee Leave and Attendance Final Project
│
├── employee-leave-attendance-system - Copy.zip
├── leave-attendance-ui - Copy.zip
└── README.md
```

The Angular front end is clearly separated from the Spring Boot backend.

---

# Spring Boot Backend Repository

Original Spring Boot backend repository:

https://github.com/athoobj/employee-leave-attendance-system.git

The Angular application uses this existing Spring Boot backend.

The Spring Boot application and MySQL database must be running before the Angular front end is started.

---

# Backend Runtime with Podman

The backend runs using two separate Podman containers:

1. Spring Boot application container
2. MySQL database container

Both containers run on the same Podman network.

The Spring Boot API is available at:

```text
http://localhost:8081
```

Before starting the Angular application, make sure that:

- Podman is running.
- The MySQL container is running.
- The Spring Boot application container is running.
- Both containers are connected to the same Podman network.
- Port `8081` is reachable from the host.
- The Spring Boot application can connect to MySQL.
- CORS allows requests from the Angular development server.

The backend must allow the Angular development server origin:

```text
http://localhost:4200
```

## Check Running Containers

The running containers can be checked using:

```bash
podman ps
```

The Spring Boot and MySQL containers must both appear as running.

---

# Running the Angular Front End

## 1. Requirements

Before running the front end, install:

- Node.js
- npm
- Angular CLI

The Spring Boot backend and MySQL database must also already be running.

---

## 2. Install Dependencies

Open a terminal inside the Angular project folder and run:

```bash
npm install
```

---

## 3. Start the Angular Development Server

Run:

```bash
ng serve
```

The Angular application will be available at:

```text
http://localhost:4200
```

---

## 4. Production Build

Before final submission, run:

```bash
ng build
```

The production build must complete without compilation errors.

The generated production files are created inside the Angular `dist` directory.

---

# Environment Configuration

The backend API URL is stored in the Angular environment configuration.

The API URL is not hardcoded inside components or services.

## API Base URL

```text
http://localhost:8081
```

Example environment configuration:

```typescript
export const environment = {
  apiBaseUrl: 'http://localhost:8081'
};
```

Angular services use:

```typescript
environment.apiBaseUrl
```

to communicate with the Spring Boot REST API.

---

# TypeScript Models

The Angular application uses typed TypeScript models matching the backend data.

Models are used for application entities and API responses.

The project includes typed models for data such as:

- Employee
- Leave Request
- Leave Type
- Attendance
- Login Request
- Login Response
- Dashboard summaries
- Reports
- Paged responses

Separate request models are used where create or update payloads differ from the returned response.

The application also defines the leave request workflow status values.

The application does not rely on `any` as the application data model.

---

# Authentication

Authentication is validated against the Spring Boot backend and database.

The login page sends the username and password to the backend authentication endpoint.

After successful authentication, the application stores the authenticated user's information and role through `AuthService`.

Authentication functionality includes:

- Backend login validation
- Authentication state
- Username storage
- Role storage
- Session persistence after browser refresh
- Logout
- Route protection
- Role-based access
- Authorization header through the HTTP interceptor

A failed login displays the message returned by the backend API.

---

# Demo Users

The following demo accounts can be used to test the application:

| Username | Password | Role | Access |
|---|---|---|---|
| `admin` | `admin123` | ADMIN | Full administrative access |
| `hr` | `hr123` | HR | HR and leave/attendance management |
| `employee` | `employee123` | EMPLOYEE | Personal leave and attendance operations |

---

# Role-Based Access

The application uses role-based access to control routes, navigation links, buttons, and available actions.

The Spring Boot backend remains responsible for the actual authorization and security rules.

## ADMIN

ADMIN can:

- Access the full dashboard
- View system summaries
- View reports
- View employees
- Manage employees
- View leave types
- Manage leave types
- View all leave requests
- Create leave requests when permitted
- Edit leave requests when permitted
- Perform workflow management actions
- View attendance records
- Mark employees absent
- Perform permitted delete operations
- Access administrative functionality

---

## HR

HR can:

- Access the full dashboard
- View system summaries
- View reports
- View employees
- Manage employees
- View leave types
- Manage leave types
- View all leave requests
- Create and edit permitted leave requests
- Approve leave requests
- Reject leave requests
- View attendance records
- Mark employees absent
- Perform HR management operations permitted by the backend

---

## EMPLOYEE

EMPLOYEE can:

- Access the personal dashboard
- View personal leave requests
- Submit a leave request for their own employee account
- View leave request details
- Cancel permitted leave requests
- View personal attendance records
- Check in
- Check out
- View available leave types

Standard employees see only the personal records permitted by the backend.

---

# Header

The application header displays:

- Application navigation
- Login status
- Current username
- Current user role
- Logout button

Navigation options are displayed according to the authenticated user's permissions.

---

# Application Routes

| Route | Component / Page | Guard / Access |
|---|---|---|
| `/` | Redirect to Dashboard | Redirect |
| `/login` | Login | Public |
| `/dashboard` | Dashboard | Auth Guard |
| `/leave-requests` | Leave Requests List | Auth Guard |
| `/leave-requests/search/:keyword` | Leave Requests Search | Auth Guard |
| `/leave-requests/filter/:value` | Leave Requests Filter | Auth Guard |
| `/leave-requests/detail/:id` | Leave Request Detail | Auth Guard |
| `/leave-requests/add` | Leave Request Form | Auth Guard |
| `/leave-requests/edit/:id` | Leave Request Form | Auth Guard + Role Guard |
| `/leave-requests/:id/workflow` | Leave Request Workflow | Auth Guard |
| `/attendance` | Attendance | Auth Guard |
| `/employees` | Employees | Auth Guard + Role Guard |
| `/leave-types` | Leave Types | Auth Guard |
| `/access-denied` | Access Denied | Public |
| `**` | Page Not Found | Wildcard |

The wildcard route is placed last.

Navigation uses Angular Router and `routerLink`.

Route parameters are read using `ActivatedRoute`.

---

# Route Guards

## Auth Guard

The Auth Guard checks whether the user is authenticated.

If the user is not logged in, the application redirects to:

```text
/login
```

---

## Role Guard

The Role Guard checks whether the authenticated user's role is permitted to access the requested route.

If the user is logged in but does not have the required permission, the application redirects to:

```text
/access-denied
```

Buttons and navigation links that the current user cannot use are also hidden.

Front-end guards and hidden buttons are used for the user interface only.

The Spring Boot backend remains responsible for enforcing authorization.

---

# Implemented Modules

The application contains the required main module and additional supporting modules.

## 1. Leave Requests

Leave Requests is the main module of the application.

Implemented functionality includes:

- Display leave requests
- Load records from the backend
- View leave request details
- Create leave requests
- Update leave requests
- Delete leave requests according to permissions
- Search leave requests
- Filter leave requests
- Filter by status
- Filter by employee for privileged users
- Sort records
- Pagination
- Page size selection
- Display total record count
- Display current page information
- Status badges
- Empty result messages
- Workflow operations
- Reactive form validation
- Backend validation messages
- Role-based actions

---

# Leave Request Detail Page

The Leave Request Detail page loads a single leave request using its route ID.

It displays the request information and related data.

The page provides actions according to the current user's role and the current request status.

Available actions can include:

- Edit
- Workflow
- Delete

The page also provides a Back to List navigation option.

---

# Leave Request Form

The Leave Request form uses Angular Reactive Forms.

The same form component is used for:

- Create
- Update

In edit mode, the existing record is loaded from the backend and its values are inserted into the form.

The form contains fields such as:

- Employee
- Leave Type
- Start Date
- End Date
- Reason

Drop-down values are loaded dynamically from the backend.

Leave types are not hardcoded inside the Angular component.

---

# Reactive Form Validation

The Leave Request form contains front-end validation matching the domain requirements.

Validation includes:

- Required fields
- White-space validation where required
- Maximum reason length
- Date validation
- Date range validation
- Backend validation errors

A custom cross-field validator verifies that:

```text
End Date >= Start Date
```

Validation messages are displayed to the user.

The submit button remains disabled while:

- The form is invalid
- A request is being submitted

Front-end validation does not replace backend validation.

---

# Leave Request Workflow

The Angular application operates the real leave request workflow implemented by the Spring Boot backend.

The required workflow includes:

```text
Submit
   ↓
Approve / Reject
   ↓
Cancel when permitted
```

Workflow operations include:

- Submit Leave Request
- Approve Leave Request
- Reject Leave Request
- Cancel Leave Request

The workflow page displays:

- Current leave request
- Current status
- Available actions
- Role-permitted actions

Only valid actions for the current request state are shown.

The application requests confirmation before executing workflow actions.

After a successful workflow transition:

1. The backend updates the request.
2. Angular reloads the request.
3. The new status is displayed.

If the backend rejects an invalid transition, the application displays the message returned by the backend.

Workflow business rules remain implemented in Spring Boot.

---

# 2. Attendance

The Attendance module communicates with the backend attendance API.

Implemented functionality includes:

- Display attendance records
- View attendance by employee
- View attendance by date
- Check In
- Check Out
- Mark employee absent
- Delete attendance according to permissions
- Role-based attendance access

## Employee Attendance

EMPLOYEE users can access their permitted personal attendance information.

Employees can perform:

```text
Check In
Check Out
```

according to backend business rules.

## HR / ADMIN Attendance

Privileged users can:

- View attendance records
- View attendance by employee
- View attendance by date
- Mark an employee absent
- Perform other permitted attendance management actions

---

# 3. Employees

The Employees module loads employee information from the backend.

Implemented functionality includes:

- Display employees
- View employee data
- Employee management for privileged users
- Role-based access

The Employees page is restricted to privileged roles.

---

# 4. Leave Types

The Leave Types module loads leave type information from the backend.

Implemented functionality includes:

- Display leave types
- Load leave types from the REST API
- Use leave types in the Leave Request form
- Manage leave types according to role permissions

Leave types are loaded dynamically from the backend and are not hardcoded in the Leave Request form.

---

# Search, Filtering, Sorting and Pagination

The Leave Requests page uses the backend paged endpoint.

The page supports:

## Search

- Keyword search

## Filters

- Leave request status
- Employee for privileged users

## Sorting

The list supports backend sorting on multiple columns.

Selecting the same column changes the sorting direction between:

```text
ASC
DESC
```

## Pagination

The page supports:

- Current page
- Previous page
- Next page
- Total pages
- Total records
- Page size

Page size options include:

```text
5
10
20
```

The page resets to the first page when:

- A new keyword is entered
- A filter changes
- The page size changes

A clear empty-state message is displayed when no records match the current search or filters.

---

# HTTP Services

The Angular application uses one service for each application module.

Services use Angular `HttpClient` through dependency injection.

Service methods return typed `Observable` values.

API query parameters are created using `HttpParams`.

Angular components do not call `HttpClient` directly.

The service layer handles communication between Angular components and the Spring Boot REST API.

---

# HTTP Interceptor

The application includes an HTTP interceptor.

The interceptor automatically attaches the authorization header to API requests.

This keeps authorization logic outside individual Angular components.

The interceptor is registered in the Angular application configuration.

---

# Error Handling

The application handles errors returned by the Spring Boot backend.

## 400 - Bad Request

Backend field validation messages are displayed to the user.

Validation errors are displayed with the corresponding form fields where applicable.

## 401 - Unauthorized

The authentication state is cleared and the user is redirected to:

```text
/login
```

## 403 - Forbidden

The user is redirected to:

```text
/access-denied
```

## 404 - Not Found

The record-not-found message returned by the backend is displayed.

## 409 - Conflict

Business rule conflict messages returned by the backend are displayed.

Examples include:

- Invalid workflow transition
- Duplicate data
- Referenced records
- Business rule violations

Generic messages are used only when the backend does not provide a more specific message.

---

# Loading States

The application displays loading states while API requests are being processed.

The form submit button is disabled while a submission is in progress.

This prevents duplicate requests and provides feedback to the user.

---

# Dashboard

The application includes a role-aware Dashboard.

Dashboard values are loaded from backend report endpoints.

Angular does not calculate the aggregate totals.

---

## HR / ADMIN Dashboard

Privileged users can view system-level summary information including:

- Total Employees
- Total Leave Requests
- Total Attendance Records
- Pending Leave Requests
- Approved Leave Requests
- Rejected Leave Requests
- Cancelled Leave Requests

The dashboard also includes detailed reports such as:

- Leave Status Report
- Attendance by Employee Report

---

## EMPLOYEE Dashboard

Employees receive a personal dashboard summary.

The personal summary includes information such as:

- My Leave Requests
- My Pending Requests
- My Approved Requests
- My Rejected Requests
- My Cancelled Requests
- My Attendance Records

---

# Dashboard and Report Endpoints

The Angular Dashboard Service consumes backend dashboard/report endpoints for:

- Full System Summary
- Personal Summary
- Leave Status Report
- Attendance by Employee Report

The dashboard therefore consumes more than the minimum two required report endpoints.

All aggregate values are calculated by the Spring Boot backend using real MySQL data.

---

# Templates

Angular templates are used to:

- Render lists
- Display conditional content
- Display empty states
- Show role-based actions
- Show workflow actions
- Display status badges
- Display validation messages
- Display loading states

---

# Built-In Pipes

The application uses Angular built-in pipes with parameters.

Examples include:

```html
{{ request.startDate | date:'mediumDate' }}
```

and:

```html
{{ systemSummary.totalEmployees | number:'1.0-0' }}
```

This satisfies the requirement to use at least two built-in pipes with parameters.

---

# Custom Pipe

The application includes a custom status label pipe:

```text
StatusLabelPipe
```

The custom pipe is used to present workflow statuses in a user-friendly format.

It is used in the Leave Requests user interface.

---

# Custom Attribute Directive

The application includes a custom attribute directive:

```text
appHighlight
```

The directive highlights Leave Request records based on a domain condition.

For example, pending leave requests can be visually highlighted in the main Leave Requests list.

---

# Class Binding

Class binding is used to change the appearance of elements according to application data.

Examples include:

- Leave request status badges
- Workflow status presentation
- Conditional record styling

---

# Attribute Binding

Angular attribute binding is used on native HTML attributes where required by the interface.

This allows native HTML element attributes to respond dynamically to component data.

---

# Styling and Responsive Design

The user interface uses:

- Bootstrap CSS
- Custom CSS
- Responsive Bootstrap grid
- Responsive tables
- Cards
- Status badges
- Form styling
- Class binding
- Attribute binding

Responsive styling is included so that the application remains usable on smaller screens.

---

# Delete and Deactivation

Delete or management actions are displayed only when the current role is permitted to perform them.

Before a delete operation, the application asks the user for confirmation.

Example:

```text
Are you sure you want to delete this record?
```

After a successful delete operation, the displayed data is refreshed.

If the backend rejects deletion because the record is referenced by other data, the application displays the conflict message returned by the backend.

Backend authorization remains responsible for deciding whether the delete operation is permitted.

---

# Security

Front-end guards and role-based visibility improve the user interface but are not the security layer.

The Spring Boot backend remains responsible for:

- Authentication
- Authorization
- Role validation
- Ownership validation
- Workflow validation
- Business rules
- Database validation

Unauthorized operations are rejected by the backend even if a user attempts to call an API endpoint directly.

---

# Data Source

The application does not use mock data as a replacement for the backend.

Application data is retrieved from the existing Spring Boot REST API and MySQL database.

The Angular application does not create a new backend, new project topic, or replacement entities.

---

# Assumptions

The application assumes that:

- Podman is installed and running.
- The Spring Boot container is running.
- The MySQL container is running.
- Both backend containers are on the correct Podman network.
- The backend application is reachable on port `8081`.
- The Angular development server runs on port `4200`.
- CORS is configured to allow `http://localhost:4200`.
- The required database users exist.
- The demo users have the correct roles.
- The backend contains the required API endpoints.
- MySQL contains the data required for dashboard and application testing.

---

# Limitations

- The Angular application requires the Spring Boot backend to be running.
- The application requires a working MySQL database connection.
- Dashboard values depend on the records currently stored in MySQL.
- Available actions depend on the authenticated user's backend role.
- Workflow actions depend on the current leave request status.
- The application expects the backend API at `http://localhost:8081`.
- The application is designed to work with the existing Employee Leave and Attendance System backend.

---

# Known Issues

There are no known blocking issues in the submitted version.

The Angular production build completes successfully.

A build budget warning may be displayed because the initial Angular bundle is larger than the configured warning budget. This warning does not prevent the application from compiling or running.

---

# Git and Submission

The final project submission contains the complete Angular source code and the Spring Boot backend.

The repository must contain:

- Angular source code
- Spring Boot backend
- README.md
- Valid `.gitignore`
- Meaningful Git commits

Generated or unnecessary dependency folders are excluded.

The Angular repository must not contain:

```text
node_modules/
```

The following generated Angular folders should also remain excluded by `.gitignore`:

```text
dist/
.angular/
```

The Spring Boot generated build directory should not be submitted:

```text
target/
```

IDE-generated folders such as `.idea` should also be excluded.

---

# Production Build Verification

Before final submission, the Angular application was checked using:

```bash
ng build
```

The production build completed successfully without compilation errors.

---

# Final Version Tag

The final submitted version should be tagged:

```text
v1.0.0
```

Example Git commands:

```bash
git tag v1.0.0
git push origin v1.0.0
```

---

# Final Project Checklist

- [x] Uses the existing Spring Boot backend and assigned Employee Leave and Attendance topic
- [x] Spring Boot and MySQL run as separate Podman containers
- [x] CORS supports the Angular development server
- [x] API base URL is stored in Angular environment files
- [x] TypeScript models are typed
- [x] Paged response type is implemented
- [x] Login is validated against the backend
- [x] Authentication session survives browser refresh
- [x] Logout is implemented
- [x] Auth Guard is implemented
- [x] Role Guard is implemented
- [x] HTTP interceptor attaches authorization information
- [x] HTTP error responses are handled
- [x] Leave Requests main module is connected to the backend
- [x] Create operation is implemented
- [x] Read operation is implemented
- [x] Update operation is implemented
- [x] Delete/deactivation behavior is handled according to permissions
- [x] Keyword search is implemented
- [x] At least two filters are implemented
- [x] Sorting is implemented
- [x] Pagination is implemented
- [x] Multiple page-size options are implemented
- [x] Reactive Forms are used for create/update
- [x] Custom form validation is implemented
- [x] Backend validation messages are displayed
- [x] Leave Request workflow is implemented
- [x] Invalid workflow transitions are handled by the backend
- [x] Attendance Check In is implemented
- [x] Attendance Check Out is implemented
- [x] Dashboard consumes backend report endpoints
- [x] Dashboard is role-aware
- [x] Custom pipe is implemented and used
- [x] Custom attribute directive is implemented and used
- [x] Built-in pipes with parameters are used
- [x] Class binding is used
- [x] Attribute binding is used
- [x] Bootstrap and custom CSS are used
- [x] Responsive design is implemented
- [x] Access Denied page is implemented
- [x] Page Not Found wildcard route is implemented
- [x] README.md is included
- [x] Demo users are documented
- [x] Application routes and guards are documented
- [x] Production build compiles without errors
- [x] `node_modules` is excluded from Git
- [x] Final version is prepared for `v1.0.0`

---

# Important Testing Order

To run the complete project:

```text
1. Start Podman
        ↓
2. Start MySQL container
        ↓
3. Start Spring Boot container
        ↓
4. Verify backend on http://localhost:8081
        ↓
5. Open Angular project
        ↓
6. Run npm install if dependencies are not installed
        ↓
7. Run ng serve
        ↓
8. Open http://localhost:4200
        ↓
9. Login using one of the demo accounts
```

---

# Demo Login Summary

```text
ADMIN
Username: admin
Password: admin123

HR
Username: hr
Password: hr123

EMPLOYEE
Username: employee
Password: employee123
```

---

# Project URLs

## Angular Front End

```text
http://localhost:4200
```

## Spring Boot Backend

```text
http://localhost:8081
```

---

# Author

**Athoob Aljofan**

COOP Training Final Project

**Employee Leave and Attendance System**
