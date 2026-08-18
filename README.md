# Employee Leave and Attendance System - Angular Front End

## Project Overview

This project is the Angular front end for the **Employee Leave and Attendance System** developed as part of the COOP Training Final Project.

The Angular application connects to the existing Spring Boot REST API and provides a complete user interface for:

- Authentication
- Role-based access
- Leave Request CRUD operations
- Leave Request workflow
- Attendance operations
- Employees
- Leave Types
- Search
- Filtering
- Sorting
- Pagination
- Reactive Forms
- Validation
- Error handling
- Dashboard summaries
- Dashboard reports
- Responsive user interface

The Angular application uses the same topic, entities, business rules, and backend developed in the Spring Boot COOP Final Project.

No mock services, static arrays, or replacement backend are used for application data.

All application data is loaded from the Spring Boot REST API using Angular `HttpClient` services.

---

# Assigned Topic

**Employee Leave and Attendance System**

## Main List Module

- Leave Requests

## Additional Modules

- Attendance
- Employees
- Leave Types

## Leave Request Workflow

The application operates the real Leave Request workflow implemented by the backend:

- Submit Leave Request
- Approve Leave Request
- Reject Leave Request
- Cancel Leave Request

## Attendance Workflow

The application also supports the attendance operations required for the assigned topic:

- Check In
- Check Out
- Mark Absent for permitted privileged users

The exact workflow statuses and role names used by the Angular application match the values implemented in the Spring Boot backend.

---

# Technologies Used

## Front End

- Angular
- TypeScript
- HTML
- CSS
- Bootstrap
- Angular Router
- Angular Reactive Forms
- Angular HttpClient
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
Spring Boot Service Layer
        ↓
Spring Data Repository
        ↓
MySQL
```

Angular components do not call `HttpClient` directly.

All REST API communication is handled through Angular services.

Business rules, authorization, workflow rules, and database validation remain in the Spring Boot backend.

---

# Project Repositories

## Final Project Repository

```text
https://github.com/athoobj/employee-leave-attendance-final-project
```

## Spring Boot Backend Repository

```text
https://github.com/athoobj/employee-leave-attendance-system.git
```

The Angular application uses the existing Spring Boot backend.

The backend and MySQL database must be running before the Angular application is used.

---

# Backend Runtime with Podman

The backend runs using two separate Podman containers:

1. MySQL database container
2. Spring Boot application container

Both containers must run on the same Podman network.

The Spring Boot backend is exposed to the host on:

```text
http://localhost:8081
```

The Angular development server runs on:

```text
http://localhost:4200
```

The Spring Boot backend must allow CORS requests from:

```text
http://localhost:4200
```

---

# Starting the Backend Containers

First, make sure Podman is running.

To view the available containers:

```bash
podman ps -a
```

Start the existing MySQL and Spring Boot containers using their configured container names:

```bash
podman start <mysql-container-name>
podman start <spring-boot-container-name>
```

Then verify that both containers are running:

```bash
podman ps
```

Both the MySQL container and the Spring Boot application container should appear in the running container list.

They must be connected to the same Podman network.

> The exact container names depend on the existing Spring Boot backend configuration.

After the containers are running, verify that the backend is reachable on:

```text
http://localhost:8081
```

---

# Running the Angular Front End

## 1. Requirements

Before running the Angular application, make sure the following are available:

- Node.js
- npm
- Angular CLI
- Running Spring Boot backend
- Running MySQL database

---

## 2. Install Dependencies

Open a terminal inside the Angular project folder and run:

```bash
npm install
```

The `node_modules` directory is generated locally and is not included in the submitted repository.

---

## 3. Start Angular

Run:

```bash
ng serve
```

Open the application in a browser:

```text
http://localhost:4200
```

---

## 4. Production Build

To verify the production build, run:

```bash
ng build
```

The final checked version successfully completes the Angular production build without compilation errors.

The generated production output is created inside:

```text
dist/
```

A bundle budget warning may appear, but it does not prevent the application from compiling successfully.

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

to access the Spring Boot REST API.

---

# TypeScript Models

The Angular application uses typed TypeScript interfaces/types for backend entities, request payloads, and API responses.

Models include:

- Employee
- Leave Request
- Leave Request Create
- Leave Request Update
- Leave Type
- Attendance
- Login Request
- Login Response
- System Summary
- Personal Summary
- Leave Status Report
- Attendance by Employee Report
- Generic paged response

A generic page type is used for paginated API responses.

Leave Request workflow statuses are represented using typed status values.

Separate create/update request types are used where request payloads differ from response objects.

Application data is typed instead of using `any` as the main data model.

---

# Authentication

Authentication is validated against the Spring Boot backend and database.

The Login page sends the entered username and password to the backend authentication endpoint.

After successful authentication, `AuthService` keeps the authenticated user information.

Authentication functionality includes:

- Backend-validated login
- Authentication state
- Username storage
- Role storage
- Employee ID where applicable
- Session persistence after browser refresh
- Logout
- Authenticated route protection
- Role-based route protection
- Authorization header through the HTTP interceptor

A failed authentication request displays the error returned by the backend.

---

# Demo Users

The following demo accounts are configured in the backend and can be used to demonstrate role-based functionality:

| Username | Password | Role | Access |
|---|---|---|---|
| `admin` | `admin123` | `ADMIN` | Full privileged and administrative access |
| `hr` | `hr123` | `HR` | HR, employee, leave, attendance, workflow and reporting functionality |
| `employee` | `employee123` | `EMPLOYEE` | Personal leave, attendance and dashboard functionality |

These users are authenticated against the Spring Boot backend/database.

---

# Role-Based Access

The application uses the actual role names implemented by the Spring Boot backend:

```text
ADMIN
HR
EMPLOYEE
```

The backend remains the final authority for security and authorization.

## ADMIN

ADMIN has privileged access and can perform permitted administrative functionality such as:

- View the full system dashboard
- View reports
- View employees
- Manage employees
- View leave types
- Manage leave types
- View all Leave Requests
- Create and edit permitted Leave Requests
- Perform permitted workflow operations
- View attendance records
- Perform privileged attendance operations
- Mark employees absent
- Perform permitted delete operations

---

## HR

HR can perform the permitted HR functionality defined by the backend, including:

- View the full dashboard
- View reports
- View employees
- Manage permitted employee functionality
- View leave types
- Manage permitted leave type functionality
- View all Leave Requests
- Create and edit permitted Leave Requests
- Approve Leave Requests
- Reject Leave Requests
- View attendance records
- Mark employees absent

Exact authorization remains enforced by the Spring Boot backend.

---

## EMPLOYEE

EMPLOYEE users can access permitted personal functionality such as:

- View the personal dashboard
- View personal Leave Requests
- Submit a Leave Request for their own employee account
- View Leave Request details
- Cancel permitted Leave Requests
- View personal attendance records
- Check In
- Check Out
- View available Leave Types

Personal-data ownership is enforced by the backend.

---

# Application Structure

The Angular application contains the following major components/pages:

- App
- Header
- Login
- Dashboard
- Leave Requests List
- Leave Request Detail
- Leave Request Form
- Leave Request Workflow
- Attendance
- Employees
- Leave Types
- Search
- Filter Menu
- Access Denied
- Page Not Found

The application contains one complete main CRUD module and multiple supporting modules.

---

# Header and Navigation

The shared Header displays the application title and navigation links.

Available navigation links include:

- Dashboard
- Leave Requests
- Attendance
- Leave Types
- Employees for privileged users

For authenticated users, the Header also displays:

- Current username
- Current role
- Logout button

The Header is only displayed when the user is authenticated.

Navigation options that are not available to the current role are hidden where appropriate.

---

# Application Routes

| Route | Component / Page | Guard / Access |
|---|---|---|
| `/` | Redirect to Dashboard | Redirect |
| `/login` | Login | Public |
| `/dashboard` | Dashboard | Auth Guard |
| `/leave-requests` | Leave Requests | Auth Guard |
| `/leave-requests/search/:keyword` | Leave Requests Search | Auth Guard |
| `/leave-requests/filter/:value` | Leave Requests Filter | Auth Guard |
| `/leave-requests/detail/:id` | Leave Request Detail | Auth Guard |
| `/leave-requests/add` | Leave Request Form | Auth Guard |
| `/leave-requests/edit/:id` | Leave Request Form | Auth Guard + Role Guard (`HR`, `ADMIN`) |
| `/leave-requests/:id/workflow` | Leave Request Workflow | Auth Guard |
| `/attendance` | Attendance | Auth Guard |
| `/employees` | Employees | Auth Guard + Role Guard (`HR`, `ADMIN`) |
| `/leave-types` | Leave Types | Auth Guard |
| `/access-denied` | Access Denied | Public |
| `**` | Page Not Found | Wildcard route |

The wildcard route is placed last.

Angular Router and `routerLink` are used for navigation.

Route parameters are read using `ActivatedRoute`.

---

# Route Guards

## Auth Guard

The Auth Guard checks whether the user is authenticated.

If the user is not logged in, the route redirects to:

```text
/login
```

---

## Role Guard

The Role Guard reads the roles configured in route data.

If the current role is allowed, navigation continues.

If an authenticated user does not have the required role, the application redirects to:

```text
/access-denied
```

The front end also hides actions that are not available to the current role.

The Spring Boot backend still enforces the actual authorization rules.

---

# Main Module - Leave Requests

Leave Requests is the main CRUD module.

The Leave Requests page loads its data from the real Spring Boot API.

Implemented functionality includes:

- Display Leave Requests
- Paged backend results
- View Leave Request details
- Create Leave Requests
- Update Leave Requests
- Delete Leave Requests according to permissions
- Keyword search
- Status filter
- Employee filter for privileged users
- Sorting
- Pagination
- Page size selection
- Total record count
- Current page information
- Status badges
- Empty result message
- Workflow operations
- Role-based buttons/actions
- Loading states
- Backend error messages

---

# Leave Request Main List

The Leave Requests list uses the paged response returned by the backend.

The page displays:

- Records
- Current page
- Total pages
- Total records
- Page size

Available page sizes:

```text
5
10
20
```

The page supports sorting on multiple columns, including:

- ID
- Start Date
- End Date

Sorting direction can switch between:

```text
ASC
DESC
```

The page supports:

- Keyword search
- Status filtering
- Employee filtering for privileged users

The current page resets when appropriate after a new search, filter, or page-size selection.

When no records match the search/filter criteria, the application displays an empty-state message.

Leave Request statuses are displayed as coloured badges using class binding.

---

# Leave Request Detail Page

The detail page loads one Leave Request using the route ID.

It displays the Leave Request information together with related domain data such as:

- Employee
- Leave Type
- Dates
- Reason
- Status

Actions are displayed according to the authenticated role.

Possible actions include:

- Edit
- Workflow
- Delete

The detail page also includes navigation back to the Leave Requests list.

---

# Leave Request Create and Update Form

The Leave Request form uses **Angular Reactive Forms**.

The same component is used for:

- Create
- Update

In edit mode:

1. The request ID is read from the route.
2. The existing request is loaded from the backend.
3. Existing values are inserted into the form.
4. The updated request is sent to the backend when submitted.

The form contains:

- Employee
- Leave Type
- Start Date
- End Date
- Reason

Employee and Leave Type options are loaded dynamically from backend services.

Values are not replaced with static mock arrays.

---

# Form Validation

The Leave Request form contains front-end validation aligned with the backend/domain rules.

Validation includes:

- Required Employee
- Required Leave Type
- Required Start Date
- Required End Date
- Maximum Reason length
- Date range validation
- Backend validation errors

A custom cross-field validator verifies:

```text
End Date >= Start Date
```

Validation messages are displayed near the corresponding fields.

The submit button remains disabled while:

- The form is invalid
- A submission is already in progress

Validation errors returned by the backend are also displayed.

Front-end validation does not replace backend validation.

---

# Leave Request Workflow

The application contains a dedicated Leave Request workflow screen.

The workflow operates the real status transitions implemented by Spring Boot.

Supported Leave Request operations include:

```text
Submit
Approve
Reject
Cancel
```

The workflow screen displays:

- Current request
- Current status
- Available workflow actions
- Actions permitted for the authenticated role

Only appropriate transitions are offered to the user.

Confirmation is requested before workflow operations where appropriate.

After a successful operation:

1. The request is updated by Spring Boot.
2. Angular refreshes the request.
3. The new status is displayed.

If the backend rejects a transition, Angular displays the message returned by the backend.

Workflow business rules remain server-side.

---

# Attendance Module

The Attendance module communicates with the Spring Boot Attendance API.

Implemented functionality includes:

- Display attendance records
- View attendance by employee
- View attendance by date
- Check In
- Check Out
- Mark Absent
- Delete attendance according to permissions
- Role-aware attendance access

## EMPLOYEE Attendance

EMPLOYEE users can access their permitted personal attendance data.

Permitted employee workflow actions include:

```text
Check In
Check Out
```

## HR / ADMIN Attendance

Privileged users can perform permitted attendance management actions such as:

- View attendance records
- View records by employee
- View records by date
- Mark an employee absent
- Perform permitted management/delete operations

Authorization remains enforced by the backend.

---

# Employees Module

The Employees module loads employee information from the Spring Boot REST API.

Implemented functionality includes:

- Display employees
- Display employee number
- Display employee full name
- Display employee email
- Display department
- Display active status
- Role-based employee management

The Employees route is restricted to:

```text
HR
ADMIN
```

---

# Leave Types Module

The Leave Types module loads Leave Type records from the backend.

Implemented functionality includes:

- Display Leave Types
- Display Leave Type description
- Display maximum days
- Display active status
- Use Leave Types in the Leave Request form
- Role-based management where permitted

Leave Types are loaded dynamically from the API and are not hardcoded into the Leave Request form.

---

# Search Component

The application includes a reusable Search component.

It is used to perform keyword search against the backend list endpoint.

Search criteria are sent through the Angular service layer.

---

# Filter Menu Component

The application includes a reusable Filter Menu.

The Leave Requests list supports at least two filter/search criteria:

- Leave Request Status
- Employee

The Employee filter is displayed to privileged users.

---

# Service Layer and HttpClient

The Angular application uses a service layer for backend communication.

Services include functionality for modules such as:

- Authentication
- Leave Requests
- Attendance
- Employees
- Leave Types
- Dashboard/Reports

Services use `HttpClient` through dependency injection.

Methods return typed `Observable` values.

Query parameters for search, filtering, sorting, and pagination are built using Angular HTTP parameter handling.

Components do not call `HttpClient` directly.

---

# HTTP Interceptor

The application contains an HTTP interceptor.

The interceptor:

- Retrieves the stored authentication token/information
- Adds the authorization header to API requests
- Handles authentication/authorization HTTP responses

## 401 - Unauthorized

If the backend returns `401`:

- Authentication state is cleared.
- The user is redirected to:

```text
/login
```

## 403 - Forbidden

If the backend returns `403`:

- The user is redirected to:

```text
/access-denied
```

---

# Error Handling

Backend errors are displayed to the user where appropriate.

## 400 - Bad Request

Validation errors returned by the backend can be displayed under their corresponding form fields.

## 401 - Unauthorized

The user is logged out and redirected to Login.

## 403 - Forbidden

The user is redirected to Access Denied.

## 404 - Not Found

The record-not-found message returned by the backend is displayed.

## 409 - Conflict

Business rule conflict messages returned by the backend are displayed.

Examples can include:

- Invalid workflow transition
- Duplicate data
- Referenced records
- Other backend business rule conflicts

Backend messages are preferred over replacing them with generic front-end business logic.

---

# Loading States

The application displays loading states while backend requests are in progress.

Examples include:

- Dashboard loading
- Leave Request list loading
- Leave Request detail loading
- Form loading/submission

The form submit button is disabled during submission.

---

# Dashboard

The Dashboard is role-aware.

Aggregate totals are loaded from backend endpoints.

Angular does not calculate backend report totals itself.

---

# HR / ADMIN Dashboard

Privileged users can view:

- Total Employees
- Total Leave Requests
- Total Attendance Records
- Pending Leave Requests
- Approved Leave Requests
- Rejected Leave Requests
- Cancelled Leave Requests

The privileged dashboard also displays detailed report data including:

- Leave Status Report
- Attendance by Employee Report

---

# EMPLOYEE Dashboard

EMPLOYEE users receive a personal summary including:

- My Leave Requests
- My Pending Requests
- My Approved Requests
- My Rejected Requests
- My Cancelled Requests
- My Attendance Records

---

# Dashboard and Report Endpoints

The Angular `DashboardService` consumes the following real Spring Boot endpoints.

## Full System Summary

```text
GET http://localhost:8081/summary
```

Used for the privileged system dashboard.

---

## Personal Summary

```text
GET http://localhost:8081/dashboard/personal
```

Used for the authenticated employee's personal dashboard.

---

## Leave Status Report

```text
GET http://localhost:8081/summary/leave-status
```

Returns Leave Request totals grouped by status.

---

## Attendance by Employee Report

```text
GET http://localhost:8081/summary/attendance-by-employee
```

Returns Attendance record counts grouped by employee.

---

The application therefore consumes more than the required minimum of two dashboard/report endpoints.

All report values are calculated by the Spring Boot backend using MySQL data.

---

# Angular Templates

The Angular templates use conditional and loop rendering to display:

- Lists
- Reports
- Role-based content
- Workflow actions
- Validation messages
- Loading states
- Empty results
- Status badges

---

# Built-In Pipes

The application uses Angular built-in pipes with parameters.

Examples include the Date pipe:

```html
{{ request.startDate | date:'mediumDate' }}
```

and the Number pipe:

```html
{{ systemSummary.totalEmployees | number:'1.0-0' }}
```

Therefore, the application uses at least two built-in pipes with parameters.

---

# Custom Pipe

The application includes:

```text
StatusLabelPipe
```

The custom pipe is used in the Leave Requests interface to display workflow status values in a user-friendly format.

---

# Custom Attribute Directive

The application includes the custom attribute directive:

```text
appHighlight
```

It is used in the Leave Requests list to visually highlight records that meet a domain condition.

For example, pending Leave Requests can be highlighted.

---

# Class Binding

Class binding is used to change styles according to application data.

Examples include status badges:

- Pending
- Approved
- Rejected
- Cancelled

Different CSS classes are applied according to Leave Request status.

---

# Attribute Binding

The application uses Angular attribute binding on native HTML attributes.

Examples include dynamic:

- `aria-label`
- `title`

This satisfies the native attribute-binding requirement.

---

# Bootstrap and Custom CSS

Bootstrap CSS is installed and registered in the application.

The application also contains custom CSS for project-specific styling.

The interface uses:

- Bootstrap containers
- Bootstrap grid
- Cards
- Tables
- Forms
- Buttons
- Badges
- Responsive utilities
- Custom CSS rules

---

# Responsive Design

The application is designed to remain usable on smaller screens.

Responsive functionality includes:

- Bootstrap responsive grid
- Responsive cards
- Responsive tables
- Responsive navigation/layout
- Custom responsive styling

The application was tested in the browser using a responsive viewport of approximately:

```text
423 x 645
```

The Dashboard and navigation remained usable at the smaller screen size.

---

# Delete and Deactivation Behaviour

Delete actions are displayed according to role permissions.

Delete functionality is available where permitted by the backend.

Delete actions are available from the required Leave Request locations, including:

- Main list row
- Detail page

Before deleting a record, the application asks for confirmation.

After a successful operation, the displayed data is refreshed or navigation returns to the updated list.

If the backend prevents deletion because a record is referenced or because of another business rule, the backend conflict/error message is displayed.

The backend remains responsible for deciding whether deletion or deactivation is permitted.

---

# Security

Front-end role checks are used to improve the user interface but are not considered the security layer.

The Spring Boot backend remains responsible for:

- Authentication
- Authorization
- Role validation
- Record ownership
- Workflow validation
- Business rules
- Database validation

A hidden button in Angular does not grant or remove backend permission.

Unauthorized API operations are still rejected by Spring Boot.

---

# Data Source

The application uses real data from:

```text
Angular
   ↓
Spring Boot REST API
   ↓
MySQL
```

The application does not replace backend data with:

- Static arrays
- Mock services
- Hardcoded application records

The Angular project does not introduce a new project topic, new backend, or replacement business entities.

---

# Assumptions

The application assumes:

- Podman is installed and running.
- The MySQL container is configured.
- The Spring Boot application container is configured.
- Both containers are running.
- Both containers are on the required Podman network.
- Spring Boot can connect to MySQL.
- The backend is reachable through port `8081`.
- Angular runs on port `4200`.
- CORS permits requests from `http://localhost:4200`.
- The required database records exist.
- The demo users exist with the documented roles.
- Backend workflow and authorization endpoints are available.

---

# Limitations

- The Angular application requires the Spring Boot backend to be running.
- The Spring Boot backend requires the MySQL database.
- Dashboard values depend on the records currently stored in MySQL.
- Available buttons/actions depend on the logged-in user's role.
- Workflow actions depend on the current Leave Request status.
- The application expects the backend API at `http://localhost:8081`.
- The application is intended to operate with the existing Employee Leave and Attendance System backend.

---

# Known Issues

There are no known blocking compilation errors in the final checked Angular version.

The production build completes successfully.

The following Angular build warning may appear:

```text
bundle initial exceeded maximum budget
```

This is a build-size warning only.

It does not prevent successful compilation or application execution.

---

# Git and Source Control

The Angular project uses Git source control.

The submitted source should include:

- Complete Angular source code
- README.md
- Valid `.gitignore`
- Meaningful commits

Generated/dependency folders are excluded.

The Angular source must not include:

```text
node_modules/
```

The following generated Angular folders are also excluded:

```text
dist/
.angular/
```

IDE-generated folders such as:

```text
.idea/
```

are excluded from the submitted Angular source.

For Spring Boot, the generated build directory:

```text
target/
```

is also excluded.

---

# Production Build Verification

The final Angular application was verified with:

```bash
ng build
```

The production build completed successfully without compilation errors.

The generated output location is under:

```text
dist/
```

The bundle-budget warning does not prevent a successful build.

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

# Important Testing Order

To run the complete system:

```text
1. Start Podman
        ↓
2. Start the MySQL container
        ↓
3. Start the Spring Boot container
        ↓
4. Verify the backend on http://localhost:8081
        ↓
5. Open the Angular project
        ↓
6. Run npm install if dependencies are not already installed
        ↓
7. Run ng serve
        ↓
8. Open http://localhost:4200
        ↓
9. Login using a demo account
        ↓
10. Test features according to the selected role
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

## Spring Boot Repository

```text
https://github.com/athoobj/employee-leave-attendance-system.git
```

## Final Project Repository

```text
https://github.com/athoobj/employee-leave-attendance-final-project
```

## Full System Summary

```text
http://localhost:8081/summary
```

## Personal Dashboard Summary

```text
http://localhost:8081/dashboard/personal
```

## Leave Status Report

```text
http://localhost:8081/summary/leave-status
```

## Attendance by Employee Report

```text
http://localhost:8081/summary/attendance-by-employee
```

---

# Final Submission Requirements

Before submission:

1. Make sure the final Angular source code is saved.
2. Make sure the final Spring Boot backend is available.
3. Make sure MySQL and Spring Boot run using Podman.
4. Confirm CORS allows the Angular origin.
5. Confirm `environment.apiBaseUrl` points to `http://localhost:8081`.
6. Confirm `node_modules` is not submitted.
7. Confirm generated Angular build/cache directories are excluded.
8. Confirm Spring Boot `target` is excluded.
9. Run:

```bash
ng build
```

10. Confirm there are no compilation errors.
11. Test login with ADMIN.
12. Test login with HR.
13. Test login with EMPLOYEE.
14. Test session persistence after browser refresh.
15. Test Logout.
16. Test role guards.
17. Test Access Denied.
18. Test Leave Requests.
19. Test Create.
20. Test Read/Detail.
21. Test Update.
22. Test permitted Delete.
23. Test keyword Search.
24. Test Status filter.
25. Test Employee filter.
26. Test Sorting.
27. Test Pagination.
28. Test page-size options.
29. Test workflow Submit.
30. Test workflow Approve.
31. Test workflow Reject.
32. Test workflow Cancel.
33. Test Check In.
34. Test Check Out.
35. Test Attendance.
36. Test Employees.
37. Test Leave Types.
38. Test Dashboard as a privileged user.
39. Test Dashboard as EMPLOYEE.
40. Test responsive/small-screen layout.
41. Confirm README.md is included.
42. Confirm the Spring Boot repository link is included.
43. Commit the final version.
44. Tag the final submitted version as:

```text
v1.0.0
```

45. Verify the final GitHub repository before submitting its link.

---

# Final Version Tag

The final submitted Git version must be tagged:

```text
v1.0.0
```

The tag should be created only after the final source code and README have been committed.

Example commands:

```bash
git tag v1.0.0
git push origin v1.0.0
```

---

# Final Project Requirement Coverage

The final application covers:

- Existing assigned backend and topic
- Spring Boot and MySQL with Podman
- Angular routing
- Environment API configuration
- Typed TypeScript models
- Generic paged response
- Authentication
- Session persistence
- Logout
- Role-based access
- Route Guards
- HTTP Interceptor
- Error handling
- Main Leave Request CRUD
- Search
- At least two filters
- Sorting
- Pagination
- Reactive Forms
- Custom validation
- Backend validation messages
- Leave Request workflow
- Attendance Check In
- Attendance Check Out
- Dashboard summary endpoints
- Detailed report data
- Role-aware Dashboard
- Built-in pipes with parameters
- Custom pipe
- Custom directive
- Class binding
- Attribute binding
- Bootstrap
- Custom CSS
- Responsive design
- Access Denied
- Page Not Found
- Production build
- README documentation
- Spring Boot repository link

The final submitted repository must also contain the required `v1.0.0` tag.

---

# Author

**Athoob Aljofan**

COOP Training Final Project

**Employee Leave and Attendance System**
