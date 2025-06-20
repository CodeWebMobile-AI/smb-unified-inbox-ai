# smb-unified-inbox-ai Architecture

## Overview
AI-powered unified inbox for SMBs. Consolidates customer inquiries, automates responses, and qualifies leads across multiple channels.

### Problem Statement
Small businesses struggle to efficiently manage and respond to customer inquiries across multiple platforms leading to missed leads, slow response times, and frustrated customers.

### Target Audience
Small business owners (restaurants, local shops, service providers) with limited staff and resources who need a streamlined way to manage customer communication and improve responsiveness.

### Core Entities


## Design System

### Typography
- **Font**: Inter
- **Font Stack**: `'Inter', sans-serif`
- **Google Fonts**: https://fonts.google.com/specimen/Inter
- **Rationale**: Inter is chosen for its excellent readability at various sizes and weights, making it suitable for both body text and headings. It's designed with a focus on digital displays.

### Color Palette
A complementary palette chosen for its balance and professional feel, aiming to create a modern and approachable user experience. Colors are chosen with WCAG AA accessibility in mind.

- **Indigo**: `#3F51B5` - Main brand color, used for headers and primary actions.
- **Teal**: `#009688` - Supporting color for secondary elements, such as buttons and accents.
- **Amber**: `#FFC107` - Used for call-to-action buttons and highlights, such as important notifications.
- **Dark Grey**: `#212121` - Primary text color for high contrast and readability.
- **Off-White**: `#FAFAFA` - Main background color for content areas.
- **Light Grey**: `#E0E0E0` - For card borders, dividers, and form inputs.
- **Green**: `#4CAF50` - For success messages and confirmation.
- **Orange**: `#FF9800` - For warnings and non-critical alerts.
- **Red**: `#F44336` - For error messages and destructive actions.

## System Architecture

### 1. Core Components & Rationale
The system comprises the following core components:

*   **Laravel Backend (API):** Handles authentication, authorization, data processing, database interactions, and real-time communication via Redis.
*   **React Frontend (UI):** Provides the user interface for managing inboxes, messages, and customer data.
*   **Database (MySQL/PostgreSQL):** Stores customer data, messages, tags, user accounts, and other persistent data. PostgreSQL is preferred for its advanced data types and robustness.
*   **Redis:** Used for real-time updates and caching to improve performance.
*   **Laravel Echo Server:** Facilitates WebSocket communication for real-time updates.
*   **IMAP/SMTP Server Integration:** Connects to external email servers to retrieve and send emails.  Consider using a dedicated library for easier integration (e.g., `Webklex/laravel-imap` for IMAP).
*   **AI Chatbot Engine:** A component for processing customer messages and generating automated responses (can start with simple template-based responses and evolve into a more sophisticated NLP model).
*   **Queue System:** Laravel's queue system handles asynchronous tasks, such as sending emails, processing large datasets, and running sentiment analysis.

Rationale:

*   Laravel provides a robust framework for building APIs and managing application logic.
*   React, combined with Inertia.js, offers a modern and efficient way to build dynamic user interfaces.
*   Redis enables real-time functionality and improves application performance through caching.
*   TypeScript enforces type safety and improves code maintainability.

### 2. Database Schema Design
The database schema includes the following tables:

*   **users:** Stores user information (id, name, email, password, etc.).
*   **customers:** Stores customer information (id, name, email, phone, etc.).
*   **messages:** Stores messages from various platforms (id, customer_id, platform, content, sent_at, received_at, etc.). Platform can be 'email', 'chat', 'sms', etc.
*   **tags:** Stores tags for categorizing customers and messages (id, name, color).
*   **customer_tags:** A pivot table linking customers and tags (customer_id, tag_id).
*   **message_tags:** A pivot table linking messages and tags (message_id, tag_id).
*   **chatbots:** Stores chatbot templates and rules (id, name, trigger_keywords, response_template).
*   **email_accounts:** Stores the information needed to connect to the IMAP/SMTP server (id, user_id, host, port, username, password, encryption).
*   **failed_jobs:** Stores information about failed queue jobs.

Consider using Laravel's Eloquent ORM for database interactions, which provides a convenient and secure way to manage database records.  Use migrations and seeds to manage database schema changes and populate initial data. Foreign key constraints should be used to enforce data integrity.

### 3. API Design & Key Endpoints
The API follows a RESTful architecture and utilizes JSON for data exchange. Key endpoints include:

*   **Authentication:**
    *   `POST /api/register`: Registers a new user.
    *   `POST /api/login`: Logs in an existing user.
    *   `POST /api/logout`: Logs out the current user.
*   **Customers:**
    *   `GET /api/customers`: Retrieves a list of customers (paginated).
    *   `POST /api/customers`: Creates a new customer.
    *   `GET /api/customers/{customer}`: Retrieves a specific customer.
    *   `PUT /api/customers/{customer}`: Updates an existing customer.
    *   `DELETE /api/customers/{customer}`: Deletes a customer.
*   **Messages:**
    *   `GET /api/messages`: Retrieves a list of messages (paginated).
    *   `GET /api/messages/{message}`: Retrieves a specific message.
*   **Tags:**
    *   `GET /api/tags`: Retrieves a list of tags.
    *   `POST /api/tags`: Creates a new tag.
    *   `PUT /api/tags/{tag}`: Updates an existing tag.
    *   `DELETE /api/tags/{tag}`: Deletes a tag.
*   **Chatbots:**
    *   `GET /api/chatbots`: Retrieves a list of chatbots.
    *   `POST /api/chatbots`: Creates a new chatbot.
    *   `PUT /api/chatbots/{chatbot}`: Updates an existing chatbot.
    *   `DELETE /api/chatbots/{chatbot}`: Deletes a chatbot.
*   **Email Accounts:**
    *   `GET /api/email_accounts`: Retrieves a list of email accounts associated with a user.
    *   `POST /api/email_accounts`: Creates a new email account.
    *   `PUT /api/email_accounts/{email_account}`: Updates an existing email account.
    *   `DELETE /api/email_accounts/{email_account}`: Deletes an email account.

API endpoints will use proper HTTP status codes to indicate success or failure. Utilize Laravel's resource controllers for streamlined API development.  Implement API versioning to handle future API changes.

### 4. Frontend Structure (TypeScript)
The frontend is built using React and Inertia.js with TypeScript.

*   **Components:** Reusable UI elements (e.g., buttons, forms, tables).
*   **Pages:** Inertia.js pages that render components and handle data fetching.
*   **Layouts:** Define the overall structure of pages (e.g., header, sidebar, footer).
*   **Types:** TypeScript type definitions for data models and API responses.
*   **Services:** Handles API calls and data transformations.
*   **Hooks:** Custom React hooks for managing state and logic.
*   **Utils:** Utility functions for common tasks (e.g., date formatting, string manipulation).

Directory Structure (Example):

```
resources/
├── js/
│   ├── Components/
│   │   ├── Button.tsx
│   │   ├── Input.tsx
│   │   └── ...
│   ├── Pages/
│   │   ├── Customers/
│   │   │   ├── Index.tsx
│   │   │   ├── Create.tsx
│   │   │   └── ...
│   │   ├── Messages/
│   │   │   ├── Index.tsx
│   │   │   └── ...
│   │   └── ...
│   ├── Layouts/
│   │   ├── Authenticated.tsx
│   │   └── ...
│   ├── Types/
│   │   ├── Customer.ts
│   │   ├── Message.ts
│   │   └── ...
│   ├── Services/
│   │   ├── CustomerService.ts
│   │   ├── MessageService.ts
│   │   └── ...
│   ├── Hooks/
│   │   ├── useCustomer.ts
│   │   └── ...
│   ├── Utils/
│   │   ├── dateUtils.ts
│   │   └── ...
│   └── app.tsx
```

Using TypeScript ensures type safety and improves code maintainability, especially as the project grows.  Use functional components and React hooks for managing state and side effects.

### 5. Real-time & Events Architecture
Real-time functionality is implemented using Redis and Laravel Echo Server.

*   **Laravel Events:** Define events that occur within the application (e.g., `MessageReceived`, `CustomerUpdated`).
*   **Redis:** Used as the message broker for broadcasting events to connected clients.
*   **Laravel Echo Server:** A Node.js server that acts as a WebSocket server, subscribing to Redis channels and broadcasting events to clients.
*   **Frontend (React):** Uses the `laravel-echo` library to connect to the Laravel Echo Server and listen for events.

Workflow:

1.  An event occurs in the Laravel backend (e.g., a new message is received).
2.  The Laravel backend dispatches the event.
3.  The event is broadcast to a Redis channel.
4.  Laravel Echo Server subscribes to the Redis channel and receives the event.
5.  Laravel Echo Server broadcasts the event to connected clients (React frontend).
6.  The React frontend receives the event and updates the UI accordingly.

Example (Laravel):

```php
// Event
namespace App\Events;
use Illuminate\Broadcasting\InteractsWithSockets;
use Illuminate\Broadcasting\PrivateChannel;
use Illuminate\Contracts\Broadcasting\ShouldBroadcast;
use Illuminate\Foundation\Events\Dispatchable;
use Illuminate\Queue\SerializesModels;

class MessageReceived implements ShouldBroadcast
{
    use Dispatchable, InteractsWithSockets, SerializesModels;

    public $message;

    public function __construct($message)
    {
        $this->message = $message;
    }

    public function broadcastOn()
    {
        return new PrivateChannel('customer.' . $this->message->customer_id);
    }
}

// Dispatching the event
event(new MessageReceived($message));
```

Example (React):

```typescript
import Echo from 'laravel-echo';

window.Echo = new Echo({
    broadcaster: 'socket.io',
    host: window.location.hostname + ':6001'
});

window.Echo.private(`customer.${customerId}`)
    .listen('MessageReceived', (e: any) => {
        console.log('New message:', e.message);
        // Update the UI with the new message
    });
```

Configure `laravel-echo-server` and `redis` correctly. Use private channels to ensure only authorized users can receive events.  Secure your WebSocket connection with SSL/TLS.

### 6. Authentication & Authorization Flow
Authentication and authorization are implemented using Laravel's built-in authentication features.

*   **Authentication:** Verifies the identity of a user.
*   **Authorization:** Determines what resources a user is allowed to access.

Workflow:

1.  User attempts to log in by providing their email and password.
2.  The Laravel backend authenticates the user against the database.
3.  If authentication is successful, the backend issues a JWT (JSON Web Token) or uses Laravel's session management.
4.  The JWT or session ID is stored in the browser (e.g., in local storage or a cookie).
5.  Subsequent requests from the frontend include the JWT or session ID in the `Authorization` header or as a cookie.
6.  The Laravel backend validates the JWT or session ID to authenticate the user and authorize access to resources.

Consider using Laravel Sanctum for API authentication, which provides a lightweight and secure way to authenticate users using tokens. Implement role-based access control (RBAC) to manage user permissions.  Protect sensitive endpoints with middleware to ensure only authorized users can access them.

### 7. Deployment & Scalability Plan
The application can be deployed to various platforms, such as AWS, Google Cloud, or DigitalOcean.

*   **Infrastructure:** Use a load balancer to distribute traffic across multiple application servers.  Use a database cluster for high availability and scalability. Consider using containerization (Docker) for consistent deployments.
*   **Deployment Process:** Automate the deployment process using CI/CD pipelines (e.g., GitHub Actions, GitLab CI).  Use zero-downtime deployment strategies to minimize downtime during deployments.
*   **Scalability:** Scale the application horizontally by adding more application servers and database replicas.  Use caching to reduce the load on the database.
*   **Monitoring:** Monitor the application's performance using tools such as New Relic, Datadog, or Prometheus.  Set up alerts to notify you of any issues.

Example Deployment Stack (AWS):

*   **Load Balancer:** AWS Elastic Load Balancer (ELB).
*   **Application Servers:** AWS EC2 instances with Docker.
*   **Database:** AWS RDS (PostgreSQL).
*   **Cache:** AWS ElastiCache (Redis).
*   **CI/CD:** AWS CodePipeline.

Use a staging environment for testing changes before deploying to production.  Regularly review and optimize the application's performance.

### 8. Testing Strategy
A comprehensive testing strategy is crucial for ensuring the quality and reliability of the application.

*   **Backend (Laravel):**
    *   **Unit Tests (Pest):** Test individual units of code (e.g., models, controllers, services).
    *   **Feature Tests (Pest):** Test the application's features from an end-user perspective.
    *   **Integration Tests (Pest):** Test the interaction between different components of the application.
*   **Frontend (React):**
    *   **Unit Tests (Vitest, RTL):** Test individual components in isolation.
    *   **Integration Tests (Vitest, RTL):** Test the interaction between different components.
    *   **End-to-End Tests (Cypress/Playwright):** Test the entire application flow from the user's perspective.

Example (Pest):

```php
// tests/Feature/CustomerTest.php
use App\Models\Customer;

it('can create a customer', function () {
    $data = [
        'name' => 'John Doe',
        'email' => 'john.doe@example.com',
    ];

    $response = $this->postJson('/api/customers', $data);

    $response->assertStatus(201);
    $this->assertDatabaseHas('customers', $data);
});
```

Example (Vitest):

```typescript
// src/components/Button.test.tsx
import { render, screen } from '@testing-library/react';
import Button from './Button';

test('renders a button with the correct text', () => {
    render(<Button>Click me</Button>);
    const buttonElement = screen.getByText('Click me');
    expect(buttonElement).toBeInTheDocument();
});
```

Write tests for all critical features and components.  Use code coverage tools to ensure that all code is adequately tested.  Automate the testing process using CI/CD pipelines.

### 9. Security & Hardening Plan
A security-first approach is mandatory to protect the application and its users from vulnerabilities, addressing OWASP Top 10.

*   **Input Validation:** Validate all user inputs to prevent injection attacks (e.g., SQL injection, XSS). Use Laravel's built-in validation features.
*   **Output Encoding:** Encode all output data to prevent XSS attacks. Use Laravel's templating engine, which automatically escapes output.
*   **Authentication and Authorization:** Implement strong authentication and authorization mechanisms. Use Laravel Sanctum for API authentication and role-based access control (RBAC) to manage user permissions.  Use MFA (Multi-factor Authentication) for enhanced security.
*   **Cross-Site Request Forgery (CSRF) Protection:** Protect against CSRF attacks by using Laravel's built-in CSRF protection.  Ensure all forms and AJAX requests include a CSRF token.
*   **SQL Injection Prevention:** Use Laravel's Eloquent ORM, which automatically prevents SQL injection attacks.
*   **Session Management:** Use secure session management techniques.  Use HTTP-only cookies to prevent XSS attacks.
*   **Dependency Management:** Keep all dependencies up to date to prevent vulnerabilities. Use Composer to manage backend dependencies and npm/yarn to manage frontend dependencies.  Regularly scan dependencies for vulnerabilities using tools like `composer audit` and `npm audit`.
*   **Security Headers:** Configure security headers (e.g., Content-Security-Policy, X-Frame-Options, Strict-Transport-Security) to protect against various attacks.
*   **Regular Security Audits:** Conduct regular security audits to identify and fix vulnerabilities. Use penetration testing tools to simulate real-world attacks.
*   **Rate Limiting:** Implement rate limiting to prevent brute-force attacks.
*   **Data Encryption:** Encrypt sensitive data at rest and in transit.  Use HTTPS for all communication.

Regularly review and update the security hardening plan to address new vulnerabilities.

### 10. Logging & Observability
Effective logging and monitoring are crucial for identifying and resolving issues in production.

*   **Logging:** Log all significant events, such as errors, warnings, and user actions. Use Laravel's built-in logging features and configure the `stderr` channel for structured logging to standard error. Utilize Monolog formatters to standardize log output (e.g., JSON format) for easier processing by log aggregation tools.
*   **Monitoring:** Monitor key metrics, such as CPU usage, memory usage, database performance, and API response times. Use tools such as New Relic, Datadog, or Prometheus.
*   **Alerting:** Set up alerts to notify you of any issues, such as high error rates or slow response times.
*   **Log Aggregation:** Aggregate logs from all servers into a central location for easier analysis. Use tools such as ELK Stack (Elasticsearch, Logstash, Kibana) or Graylog.

Example (Laravel): To configure the `stderr` channel in `config/logging.php`
```php
'channels' => [
    'stderr' => [
        'driver' => 'monolog',
        'handler' => SyslogUdpHandler::class,
        'handler_with' => [
            'host' => env('LOG_UDP_HOST', '127.0.0.1'),
            'port' => env('LOG_UDP_PORT', 514),
            'formatter' => Monolog\Formatter\JsonFormatter::class,
        ],
    ],

    // Other channels
],
```

Implement centralized logging and monitoring from day one.  Regularly review logs and metrics to identify and resolve issues. Use distributed tracing to track requests across different services.

## Feature Implementation Roadmap

### 1. 1. Implement email integration with IMAP/SMTP support
Integrate with IMAP/SMTP servers to retrieve and send emails, allowing users to manage their email communications within the unified inbox.

**Database Changes:**
- Create 'email_accounts' table (id, user_id, host, port, username, password, encryption)
- Add 'email_account_id' to 'messages' table to associate messages with email accounts

**Backend Components:**
- EmailAccount model
- EmailAccountController
- Message model
- Queue worker for fetching emails
- Service class to handle IMAP/SMTP connections (e.g., using `Webklex/laravel-imap`)

**Frontend Components:**
- EmailAccounts/Index.tsx (list email accounts)
- EmailAccounts/Create.tsx (add email account)
- EmailAccounts/Edit.tsx (edit email account)
- MessageList.tsx (display emails in the inbox)
- ComposeEmail.tsx (compose and send emails)

**API Endpoints:**
- GET /api/email_accounts
- POST /api/email_accounts
- GET /api/email_accounts/{email_account}
- PUT /api/email_accounts/{email_account}
- DELETE /api/email_accounts/{email_account}
- POST /api/email_accounts/{email_account}/sync

**Real-time Events:**
- NewEmailReceived (broadcast when a new email is fetched)

**Testing Requirements:**
- EmailAccount model unit tests
- EmailAccountController feature tests
- IMAP/SMTP connection integration tests
- Email fetching queue worker tests
- Frontend component tests for email account management


### 2. 2. Develop a basic AI chatbot for automated responses based on pre-defined templates
Create a chatbot that can automatically respond to common customer inquiries based on pre-defined templates and trigger keywords.

**Database Changes:**
- Create 'chatbots' table (id, name, trigger_keywords, response_template)

**Backend Components:**
- Chatbot model
- ChatbotController
- ChatbotService (handles chatbot logic)
- Message processing queue worker (processes incoming messages and triggers chatbot responses)

**Frontend Components:**
- Chatbots/Index.tsx (list chatbots)
- Chatbots/Create.tsx (create chatbot)
- Chatbots/Edit.tsx (edit chatbot)
- MessageView.tsx (display chatbot responses alongside human responses)
- Configuration settings page to enable / disable and customize chatbot settings

**API Endpoints:**
- GET /api/chatbots
- POST /api/chatbots
- GET /api/chatbots/{chatbot}
- PUT /api/chatbots/{chatbot}
- DELETE /api/chatbots/{chatbot}

**Real-time Events:**
- ChatbotResponseSent (broadcast when a chatbot sends a response)

**Testing Requirements:**
- Chatbot model unit tests
- ChatbotController feature tests
- ChatbotService unit tests
- Message processing queue worker tests
- Frontend component tests for chatbot management


### 3. 3. Build a lead qualification system based on keywords and sentiment analysis
Implement a system that automatically qualifies leads based on keywords found in messages and the overall sentiment expressed.

**Database Changes:**
- Add 'lead_score' column to 'customers' table
- Create 'keywords' table (id, keyword, weight)
- Create 'customer_keywords' table (customer_id, keyword_id)

**Backend Components:**
- Customer model
- Keyword model
- LeadQualificationService (handles keyword matching and sentiment analysis)
- Message processing queue worker (updates lead scores based on new messages)

**Frontend Components:**
- CustomerList.tsx (display lead scores)
- CustomerView.tsx (display keywords and sentiment analysis results)
- Configuration settings page to configure keywords and sentiment thresholds

**API Endpoints:**
- GET /api/keywords
- POST /api/keywords
- PUT /api/keywords/{keyword}
- DELETE /api/keywords/{keyword}

**Real-time Events:**
- LeadScoreUpdated (broadcast when a lead score is updated)

**Testing Requirements:**
- Customer model unit tests
- Keyword model unit tests
- LeadQualificationService unit tests
- Message processing queue worker tests
- Frontend component tests for displaying lead scores


### 4. 4. Create a centralized inbox UI to view and manage messages from all platforms
Develop a user interface that displays messages from all integrated platforms in a single, unified inbox.

**Backend Components:**
- MessageController
- MessageService (handles message retrieval and display)

**Frontend Components:**
- MessageList.tsx (display messages from all platforms)
- MessageView.tsx (view individual messages)
- PlatformFilter.tsx (filter messages by platform)
- SearchBox.tsx (search messages)
- Pagination.tsx (handle pagination of messages)

**API Endpoints:**
- GET /api/messages (with platform and search filters)

**Real-time Events:**
- NewMessageReceived (broadcast when a new message is received from any platform)

**Testing Requirements:**
- MessageController feature tests
- MessageService unit tests
- Frontend component tests for displaying and filtering messages
- End-to-end tests to verify the entire inbox flow


### 5. 5. Integrate with a basic CRM functionality (tagging and notes)
Integrate basic CRM features like tagging and note-taking to help users organize and manage customer interactions.

**Database Changes:**
- Create 'customer_notes' table (id, customer_id, note, created_at)
- Create 'tags' table (id, name, color)
- Create 'customer_tags' table (customer_id, tag_id)
- Create 'message_tags' table (message_id, tag_id)

**Backend Components:**
- CustomerNote model
- CustomerNoteController
- Tag model
- TagController
- Customer model
- Message model

**Frontend Components:**
- CustomerView.tsx (display and add customer notes)
- CustomerList.tsx (display tags for each customer)
- TagManager.tsx (manage available tags)
- MessageView.tsx (display and add tags to messages)
- CustomerFilter.tsx (filter customers by tags)

**API Endpoints:**
- GET /api/customer_notes/{customer}
- POST /api/customer_notes/{customer}
- PUT /api/customer_notes/{note}
- DELETE /api/customer_notes/{note}
- GET /api/tags
- POST /api/tags
- PUT /api/tags/{tag}
- DELETE /api/tags/{tag}
- POST /api/customers/{customer}/tags/{tag} (assign a tag to a customer)
- DELETE /api/customers/{customer}/tags/{tag} (remove a tag from a customer)

**Real-time Events:**
- CustomerNoteAdded (broadcast when a new note is added to a customer)
- CustomerTagAdded (broadcast when a tag is added to a customer)

**Testing Requirements:**
- CustomerNote model unit tests
- CustomerNoteController feature tests
- Tag model unit tests
- TagController feature tests
- Frontend component tests for managing notes and tags


## Metadata
- **Generated**: 2025-06-20T17:46:11.235104+00:00
- **Project Type**: Laravel React Starter Kit
- **Architecture Version**: 1.0.0

---
*This architecture document is maintained by the CodeWebMobile AI system and should be the source of truth for all development decisions.*
