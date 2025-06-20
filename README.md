```markdown
# smb-unified-inbox-ai: AI-Powered Unified Inbox for SMBs

## 1. Project Title and Description

**smb-unified-inbox-ai** is an AI-powered unified inbox designed to help small businesses efficiently manage customer inquiries, automate responses, and qualify leads across multiple communication channels. This project consolidates customer interactions from email, social media, SMS, and live chat into a single, streamlined interface.

## 2. Problem Statement

Small businesses struggle to efficiently manage and respond to customer inquiries across multiple platforms, leading to missed leads, slow response times, and frustrated customers.  Juggling various inboxes is time-consuming and error-prone, hindering growth and damaging customer relationships.

## 3. Target Audience and Value Proposition

This project is specifically tailored for **small business owners** (restaurants, local shops, service providers) with limited staff and resources who need a streamlined way to manage customer communication and improve responsiveness.

**Value Proposition:**

*   **Centralized Communication:** Manage all customer interactions from a single inbox, eliminating the need to switch between multiple platforms.
*   **Increased Efficiency:** Automate responses to common inquiries, freeing up staff to focus on more complex tasks.
*   **Improved Customer Satisfaction:** Respond to inquiries faster and more consistently, leading to happier customers.
*   **Enhanced Lead Generation:** Automatically qualify leads based on keywords and sentiment analysis, ensuring that sales teams focus on the most promising prospects.
*   **Actionable Insights:** Monitor inbox activity and performance metrics to identify areas for improvement.

## 4. Features

This project aims to implement the following initial features:

1.  **Email Integration:** Support for connecting to email accounts using IMAP/SMTP protocols.
2.  **Basic AI Chatbot:** An AI chatbot capable of providing automated responses based on predefined templates.
3.  **Lead Qualification System:**  Automatic lead scoring based on keywords and sentiment analysis of customer messages.
4.  **Centralized Inbox UI:** A unified inbox interface to view and manage messages from all integrated platforms.
5.  **Basic CRM Functionality:** Tagging and note-taking capabilities for organizing and managing customer interactions.

## 5. Tech Stack

*   **Backend Framework:** Laravel (PHP) - A robust framework for building APIs and managing application logic.
*   **Frontend Framework:** React (JavaScript) with Inertia.js - A modern and efficient way to build dynamic user interfaces.  TypeScript is used for type safety and improved code maintainability.
*   **Database:** PostgreSQL - Preferred for its advanced data types, robustness, and scalability.
*   **Real-time Communication:** Redis and Laravel Echo Server - Enables real-time updates for inbox messages and notifications.
*   **AI Engine:** OpenAI or Azure AI - For natural language processing, sentiment analysis, and chatbot capabilities.
*   **SMS Integration:** Twilio or a similar SMS gateway provider.
*   **IMAP/SMTP:** Webklex/laravel-imap (or similar library) for simplified IMAP integration.

## 6. Architecture Overview

The architecture follows a full-stack approach, utilizing the following key components:

*   **Laravel Backend (API):**  Handles authentication, authorization, data processing, database interactions, and real-time communication via Redis.
*   **React Frontend (UI):** Provides the user interface for managing inboxes, messages, and customer data, enhanced by Inertia.js for seamless integration.
*   **Database (PostgreSQL):** Stores customer data, messages, tags, user accounts, and other persistent data.
*   **Redis:** Used for real-time updates, caching, and queue management.
*   **Laravel Echo Server:** Facilitates WebSocket communication for real-time updates.
*   **IMAP/SMTP Server Integration:** Connects to external email servers to retrieve and send emails.
*   **AI Chatbot Engine:** Processes customer messages and generates automated responses using defined templates or a more sophisticated NLP model.
*   **Queue System:**  Laravel's queue system manages asynchronous tasks such as sending emails, processing large datasets, and running sentiment analysis.

### Design System

*   **Font:** Inter - chosen for readability.  Font Stack: `'Inter', sans-serif`.  Google Font Link: [https://fonts.google.com/specimen/Inter](https://fonts.google.com/specimen/Inter)
*   **Color Palette:**

    *   **Primary:** Indigo (#3F51B5) - Headers, primary actions.
    *   **Secondary:** Teal (#009688) - Secondary elements, accents.
    *   **Accent:** Amber (#FFC107) - Call-to-action buttons, highlights.
    *   **Neutral Text:** Dark Grey (#212121) - Primary text.
    *   **Neutral Background:** Off-White (#FAFAFA) - Main background.
    *   **Neutral Border:** Light Grey (#E0E0E0) - Card borders, dividers, form inputs.
    *   **Success:** Green (#4CAF50) - Success messages.
    *   **Warning:** Orange (#FF9800) - Warnings.
    *   **Danger:** Red (#F44336) - Error messages.

## 7. Prerequisites

Before you begin, ensure you have the following installed:

*   PHP (version >= 8.1)
*   Composer
*   Node.js (version >= 16)
*   npm or yarn
*   PostgreSQL
*   Redis
*   Laravel Echo Server (globally installed: `npm install -g laravel-echo-server`)
*   A code editor (e.g., VS Code, Sublime Text)

## 8. Installation Steps

1.  **Clone the repository:**

    ```bash
    git clone <repository_url>
    cd smb-unified-inbox-ai
    ```

2.  **Install Composer dependencies:**

    ```bash
    composer install
    ```

3.  **Install npm dependencies:**

    ```bash
    npm install
    # or
    yarn install
    ```

4.  **Copy the `.env.example` file to `.env` and configure your environment variables:**

    ```bash
    cp .env.example .env
    ```

5.  **Generate an application key:**

    ```bash
    php artisan key:generate
    ```

6.  **Configure your database settings in the `.env` file:**

    ```
    DB_CONNECTION=pgsql
    DB_HOST=127.0.0.1
    DB_PORT=5432
    DB_DATABASE=your_database_name
    DB_USERNAME=your_database_username
    DB_PASSWORD=your_database_password
    ```

7.  **Run database migrations:**

    ```bash
    php artisan migrate
    ```

8.  **Seed the database (optional):**

    ```bash
    php artisan db:seed
    ```

9.  **Configure Laravel Echo Server:**

    ```bash
    laravel-echo-server init
    ```

    Answer the questions during initialization, configuring the server to use Redis.

10. **Start the Laravel development server:**

    ```bash
    php artisan serve
    ```

11. **Start the Vite development server (for React):**

    ```bash
    npm run dev
    # or
    yarn dev
    ```

12. **Start Laravel Echo Server:**

    ```bash
    laravel-echo-server start
    ```

## 9. Environment Setup

Configure the following environment variables in your `.env` file:

*   `APP_NAME`: Your application name (e.g., `smb-unified-inbox-ai`)
*   `APP_URL`: Your application URL (e.g., `http://localhost:8000`)
*   `DB_*`: PostgreSQL database connection details.
*   `REDIS_*`: Redis connection details.
*   `BROADCAST_DRIVER`: `redis`
*   `CACHE_DRIVER`: `redis`
*   `QUEUE_CONNECTION`: `redis`
*   `SESSION_DRIVER`: `database`
*   `SANCTUM_STATEFUL_DOMAINS`: `localhost:3000` (Frontend domain)
*   `OPENAI_API_KEY`: API key for OpenAI (or equivalent for Azure AI).
*   `TWILIO_ACCOUNT_SID`: Account SID for Twilio.
*   `TWILIO_AUTH_TOKEN`: Auth token for Twilio.
*   `EMAIL_IMAP_HOST`: IMAP host for email integration.
*   `EMAIL_IMAP_PORT`: IMAP port for email integration.
*   `EMAIL_SMTP_HOST`: SMTP host for email integration.
*   `EMAIL_SMTP_PORT`: SMTP port for email integration.
*   `EMAIL_USERNAME`: Email username.
*   `EMAIL_PASSWORD`: Email password.

## 10. Development Workflow

1.  **Branching:** Use feature branches for new features and bug fixes.
2.  **Coding Style:** Follow the PSR-12 coding standard for PHP and use ESLint and Prettier for JavaScript/TypeScript.
3.  **Committing:** Write clear and concise commit messages.
4.  **Pull Requests:** Submit pull requests for review before merging code into the `main` branch.
5.  **Code Reviews:**  Perform thorough code reviews to ensure code quality and maintainability.
6.  **Testing:** Write unit, feature, and integration tests to ensure the application's functionality.

## 11. Testing

The project includes unit tests, feature tests, and end-to-end tests.

*   **Backend (Laravel):**  Use Pest for unit, feature, and integration testing.
*   **Frontend (React):**  Use Vitest and React Testing Library (RTL) for unit and integration testing. Cypress or Playwright can be used for end-to-end testing.

To run the tests:

*   **Backend Tests:**

    ```bash
    php artisan test
    ```

*   **Frontend Tests:**

    ```bash
    npm run test
    # or
    yarn test
    ```

## 12. Deployment

The application can be deployed to various platforms, such as:

*   AWS (Elastic Beanstalk, EC2, ECS)
*   Google Cloud Platform (App Engine, Compute Engine, Kubernetes Engine)
*   DigitalOcean
*   Heroku

A typical deployment process involves:

1.  Building the frontend assets: `npm run build` or `yarn build`.
2.  Configuring the web server (e.g., Nginx, Apache) to serve the application.
3.  Setting up a database connection.
4.  Configuring environment variables.
5.  Running database migrations.
6.  Starting the queue worker: `php artisan queue:work`.
7.  Deploying `laravel-echo-server` and ensuring it's running.
8.  Utilizing CI/CD pipelines (e.g., GitHub Actions, GitLab CI) for automated deployments.

Refer to the Laravel documentation and your chosen platform's documentation for detailed deployment instructions.  Consider using Docker for consistent deployments across different environments.

## 13. Monetization Strategy

The application will use a subscription-based pricing model with tiers based on the following:

*   **Number of Users:**  Different tiers for teams of varying sizes.
*   **Message Volume:** Limits on the number of messages processed per month.
*   **AI-Powered Features:** Higher tiers include access to advanced AI features such as sentiment analysis, lead scoring, and chatbot customization.

Charges apply 24/7 based on usage or the chosen subscription tier.

## 14. Contributing Guidelines

We welcome contributions to this project! Please follow these guidelines:

1.  Fork the repository.
2.  Create a feature branch for your changes.
3.  Write clear and concise commit messages.
4.  Submit a pull request with a detailed description of your changes.
5.  Ensure your code is well-tested and follows the coding style guidelines.

## 15. Custom Sections

### Database Schema

*   **businesses:**
    *   `id` (INT, primary key)
    *   `name` (VARCHAR)
    *   `email` (VARCHAR)
    *   `created_at` (TIMESTAMP)
    *   `updated_at` (TIMESTAMP)
*   **users:**
    *   `id` (INT, primary key)
    *   `business_id` (INT, foreign key)
    *   `name` (VARCHAR)
    *   `email` (VARCHAR)
    *   `password` (VARCHAR)
    *   `role` (ENUM('admin', 'agent'))
    *   `created_at` (TIMESTAMP)
    *   `updated_at` (TIMESTAMP)
*   **messages:**
    *   `id` (INT, primary key)
    *   `business_id` (INT, foreign key)
    *   `platform` (ENUM('email', 'facebook', 'sms', 'chat'))
    *   `sender` (VARCHAR)
    *   `recipient` (VARCHAR)
    *   `subject` (VARCHAR)
    *   `body` (TEXT)
    *   `created_at` (TIMESTAMP)
    *   `updated_at` (TIMESTAMP)
    *   `lead_score` (INT)
    *   `status` (ENUM('new','open','closed'))
*   **ai_templates:**
    *   `id` (INT, primary key)
    *   `business_id` (INT, foreign key)
    *   `name` (VARCHAR)
    *   `prompt` (TEXT)
    *   `created_at` (TIMESTAMP)
    *   `updated_at` (TIMESTAMP)

### Market Validation

Numerous SMB market research reports highlight the need for efficient customer communication management tools. Existing CRM systems are often too complex and expensive, while simpler solutions lack the necessary automation and integration capabilities. Reviews of unified inbox solutions show high user satisfaction, particularly for features like centralized message management and AI-powered responses.

### Customizations Roadmap
* Replace default user model with a business/user model structure for managing multiple users per business.
* Implement a multi-tenancy system to isolate data between different businesses.
* Develop platform integrations for email, social media (Facebook, Instagram, Twitter), SMS, and live chat.
* Integrate an AI engine (e.g., OpenAI, Azure AI) for automated responses and lead qualification.
* Design a dashboard for businesses to monitor inbox activity and performance metrics.

### Feature Implementation Roadmap
*   **Feature 1: Implement email integration with IMAP/SMTP support**
*   **Feature 2: Develop a basic AI chatbot for automated responses based on pre-defined templates**
*   **Feature 3: Build a lead qualification system based on keywords and sentiment analysis**
*   **Feature 4: Create a centralized inbox UI to view and manage messages from all platforms**
*   **Feature 5: Integrate with a basic CRM functionality (tagging and notes)**

(Detailed Roadmaps available in the project details)

```