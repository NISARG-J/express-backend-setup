# Express Backend Setup CLI — Technical Reference

## Overview

**Express Backend Setup CLI** is a professional-grade scaffolding tool designed to accelerate the development of production-ready Express.js applications. It automates the complex configuration of modular architectures, security middleware, database persistence layers, and external service integrations.

### Key Features

-  **Modular Architecture**: Automatically structures projects into controllers, routes, models, and services.
-  **Dual-Language Support**: Full support for both **TypeScript** and **JavaScript** with pre-configured build pipelines.
-  **Database Agnostic**: Native integration for MongoDB (Mongoose) and SQL (Sequelize supporting PostgreSQL, MySQL, and SQLite).
-  **Secure by Default**: Integrated protection using **Helmet**, **CORS**, and **Rate Limiting**.
-  **Advanced Cron Jobs**: Built-in orchestration via `cron-guardian` with retries and overlap prevention.
-  **Service Abstraction**: High-level interfaces for **Cloud Storage** (S3, Cloudinary) and **Email Delivery** (SendGrid, Mailgun).
-  **Rapid Prototyping**: Integrated CRUD generator to scaffold full API resources from existing models.

---

## Installation & Requirements

### Global Installation (Optional)
```bash
npm install -g express-backend-setup
```

### Direct Execution (Recommended)
```bash
npx express-backend-setup
```

### Requirements
- **Node.js**: ≥ 18.0.0
- **NPM**: ≥ 9.0.0
- **TypeScript**: ≥ 4.5.0 (if using TypeScript)

---

## Quick Start

1.  **Initialize Project**: Run the command and follow the interactive prompts.
    ```bash
    npx express-backend-setup my-backend-api
    ```
2.  **Navigate and Start**:
    ```bash
    cd my-backend-api
    npm run dev
    ```
3.  **Health Check**: Visit `http://localhost:8080/` to verify the API is running.

---

## CLI Reference

### Command-Line Arguments

The CLI supports the following usage patterns:

- `npx express-backend-setup [projectName]`: Starts the interactive setup.
- `npx express-backend-setup --crud [ModelName]`: Triggers the CRUD generator for an existing resource.

### Interactive Configuration Options

| Option | Description | Recommended |
|---|---|---|
| **Project Language** | Select between TypeScript (`ts`) or JavaScript (`js`). | `ts` |
| **Database Provider** | Choose MongoDB, PostgreSQL, MySQL, SQLite, or None. | - |
| **Security Tools** | Enable Helmet, CORS, and Rate Limiting. | All |
| **Storage Provider** | Select Local, AWS S3, Cloudinary, or Firebase Storage. | - |
| **Email Provider** | Select SMTP (Nodemailer), SendGrid, Mailgun, or Firebase. | - |

---

## System Architecture

The generated project follows a clean, modular architecture that separates concerns and ensures maintainability.

### Directory Structure

```text
src/
├── config/             # Connection initializers (DB, Storage, Email)
├── controllers/        # Request handling and domain logic orchestration
├── middlewares/        # Security, validation, and logging middlewares
├── models/             # Database schemas and model definitions
├── routes/             # API endpoint definitions and mounting
├── services/           # Abstraction layers for external provider logic (S3, SMTP, etc.)
├── jobs/               # Cron job management (via cron-guardian)
├── utils/              # Shared helper functions and static constants
└── index.[js|ts]       # Application entry point and assembler
```

### Middleware Pipeline Execution

Requests are processed through a strictly ordered pipeline for maximum security and performance:

1.  **Request Logging**: Initial telemetry capture.
2.  **Helmet**: Standardizes secure HTTP headers.
3.  **CORS**: Manages cross-origin resource sharing.
4.  **Rate Limiting**: Rejects abusive IPs before heavy processing.
5.  **Payload Parsing**: Converts JSON and URL-encoded bodies.
6.  **Domain Routes**: Executes specific resource logic.
7.  **Global Error Handling**: Standardizes failure responses.

---

## Feature Deep Dives

### Advanced Cron Job Management

Integration with `cron-guardian` provides a robust orchestration layer in `src/jobs/index.js`:

- **Automatic Retries**: Failed jobs can be retried with configurable delays.
- **Overlap Prevention**: Prevents concurrent execution of long-running tasks.
- **Monitoring**: Built-in logging and status tracking for all scheduled jobs.

### Resource Generator (CRUD Engine)

The CRUD engine (`--crud`) handles the "Heavy Lifting" of API development:

1.  **Static Analysis**: Parses model files (`src/models/`) to extract schema metadata.
2.  **Validator Generation**: Produces Zod schemas for strict request validation.
3.  **Action Logic**: Generates controllers with built-in pagination, filtering, and error handling.
4.  **Route Mapping**: Automatically registers all RESTful endpoints.

---

## Best Practices & Optimization

### Security Recommendations
- **Environment Variables**: Never commit your `.env` file. Use the generated `.env.example` as a template for deployment.
- **CORS Configuration**: In production, restrict `origin` to your specific frontend domains within `src/index.js`.
- **Database Indexing**: Ensure all query fields detected by the CRUD generator are properly indexed in your database.

### Performance
- **Middleware Order**: Keep the `Rate Limiter` as the first middleware to protect against DDoS attacks.
- **Asynchronous Flow**: Utilize the generated service layer to offload heavy I/O tasks (file uploads, emails) from the main request thread.

---

## Troubleshooting

### Port Conflicts
**Error**: `EADDRINUSE: address already in use :::8080`
- **Solution**: Change the `PORT` variable in your `.env` file or provide a different port during the CLI setup.

### Database Connection Failures
- **Mongo**: Ensure the `MONGODB_URL` is correctly formatted and your IP is whitelisted in Atlas.
- **SQL**: Verify the `SQL_DATABASE_URL` matches your local/production credentials and the service is running.

---

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
