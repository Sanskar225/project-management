# Project Management Application

A full-stack project management application built with Next.js, Express, PostgreSQL, and AWS services. This application provides comprehensive project tracking, task management, team collaboration, and real-time updates.

## Table of Contents

- [Features](#features)
- [Tech Stack](#tech-stack)
- [Architecture](#architecture)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Configuration](#configuration)
- [Running the Application](#running-the-application)
- [Project Structure](#project-structure)
- [API Documentation](#api-documentation)
- [Database Schema](#database-schema)
- [AWS Services](#aws-services)
- [Authentication](#authentication)
- [Deployment](#deployment)
- [Contributing](#contributing)

## Features

### Core Functionality
- **Project Management**: Create, view, and manage multiple projects
- **Task Tracking**: Comprehensive task management with status, priority, and assignments
- **Team Collaboration**: Team-based project organization with roles
- **Multiple Views**: Board (Kanban), List, Table, and Timeline (Gantt) views
- **Search & Filter**: Advanced search across projects, tasks, and users
- **Priority Management**: Organize tasks by priority (Urgent, High, Medium, Low, Backlog)
- **User Management**: User profiles, authentication, and role-based access
- **File Attachments**: Upload and manage task attachments via AWS S3
- **Comments**: Task-level commenting for collaboration
- **Dark Mode**: Toggle between light and dark themes
- **Responsive Design**: Optimized for desktop, tablet, and mobile devices

### Advanced Features
- Real-time task status updates
- Drag-and-drop task management
- Timeline visualization with Gantt charts
- Data grid with export capabilities
- Custom task assignments and tracking
- Profile picture management via S3

## Tech Stack

### Frontend
- **Framework**: Next.js 14 (React 18)
- **Language**: TypeScript
- **State Management**: Redux Toolkit with Redux Persist
- **Styling**: Tailwind CSS
- **UI Components**:
  - Material UI (MUI) for data grids
  - Lucide React for icons
- **Data Visualization**: Recharts, Gantt Task React
- **Drag & Drop**: React DnD
- **HTTP Client**: Axios
- **Authentication**: AWS Amplify

### Backend
- **Framework**: Express.js
- **Language**: TypeScript
- **Database ORM**: Prisma
- **Database**: PostgreSQL
- **Authentication**: AWS Cognito
- **Process Manager**: PM2

### AWS Services
- **S3**: File storage for images and attachments
- **Cognito**: User authentication and authorization
- **EC2**: Application hosting
- **RDS**: PostgreSQL database (optional)

### Development Tools
- **Package Manager**: npm
- **Linting**: ESLint
- **Formatting**: Prettier
- **Build Tool**: Next.js build system

## Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    Client (Next.js)                     │
│  ┌──────────────────────────────────────────────────┐  │
│  │  Pages  │  Components  │  Redux Store  │  API     │  │
│  └──────────────────────────────────────────────────┘  │
└────────────────────┬────────────────────────────────────┘
                     │
                     │ HTTP/REST
                     │
┌────────────────────▼────────────────────────────────────┐
│                 Server (Express.js)                     │
│  ┌──────────────────────────────────────────────────┐  │
│  │  Routes  │  Controllers  │  Prisma ORM            │  │
│  └──────────────────────────────────────────────────┘  │
└────────────┬───────────────────────┬────────────────────┘
             │                       │
             │                       │
    ┌────────▼─────────┐    ┌───────▼──────────┐
    │   PostgreSQL     │    │    AWS Services   │
    │    Database      │    │  - S3             │
    └──────────────────┘    │  - Cognito        │
                            └───────────────────┘
```

## Prerequisites

- **Node.js**: v18.0.0 or higher
- **npm**: v9.0.0 or higher
- **PostgreSQL**: v14.0 or higher
- **AWS Account**: For S3 and Cognito services
- **Git**: For version control

## Installation

### 1. Clone the Repository

```bash
git clone <your-repository-url>
cd project-management
```

### 2. Install Server Dependencies

```bash
cd server
npm install
```

### 3. Install Client Dependencies

```bash
cd ../client
npm install
```

## Configuration

### Server Configuration

1. Create a `.env` file in the `server` directory:

```env
# Server Configuration
PORT=8000
NODE_ENV=development

# Database Configuration
DATABASE_URL="postgresql://username:password@localhost:5432/project_management?schema=public"

# AWS Configuration (Optional - for local S3 testing)
AWS_REGION=us-east-1
AWS_ACCESS_KEY_ID=your_access_key
AWS_SECRET_ACCESS_KEY=your_secret_key
```

2. Set up the database:

```bash
cd server

# Generate Prisma Client
npx prisma generate

# Run migrations
npx prisma migrate deploy

# Seed the database (optional)
npm run seed
```

### Client Configuration

Create a `.env.local` file in the `client` directory:

```env
# API Configuration
NEXT_PUBLIC_API_BASE_URL=http://localhost:8000

# AWS Cognito Configuration
NEXT_PUBLIC_COGNITO_USER_POOL_ID=your_user_pool_id
NEXT_PUBLIC_COGNITO_USER_POOL_CLIENT_ID=your_client_id

# AWS S3 Configuration (Optional)
NEXT_PUBLIC_S3_BUCKET_URL=https://your-bucket.s3.region.amazonaws.com
```

### AWS Setup

#### S3 Bucket Setup

1. Create an S3 bucket (e.g., `pm-s3-images-sanskar`)
2. Configure CORS policy:

```json
[
  {
    "AllowedHeaders": ["*"],
    "AllowedMethods": ["GET", "PUT", "POST", "DELETE"],
    "AllowedOrigins": ["*"],
    "ExposeHeaders": []
  }
]
```

3. Set bucket policy for public read access:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::your-bucket-name/*"
    }
  ]
}
```

#### Cognito Setup

1. Create a User Pool in AWS Cognito
2. Configure app client with the following:
   - Enable username and email sign-in
   - Set password requirements
   - Configure required attributes (username, email)
3. Note the User Pool ID and Client ID for `.env.local`

## Running the Application

### Development Mode

#### Start the Server

```bash
cd server
npm run dev
```

The server will start on `http://localhost:8000`

#### Start the Client

```bash
cd client
npm run dev
```

The client will start on `http://localhost:3000`

### Production Mode

#### Build and Start Server

```bash
cd server
npm run build
npm start
```

#### Build and Start Client

```bash
cd client
npm run build
npm start
```

## Project Structure

```
project-management/
├── client/                      # Next.js frontend application
│   ├── public/                  # Static assets
│   ├── src/
│   │   ├── app/                 # Next.js app directory
│   │   │   ├── home/            # Home page
│   │   │   ├── projects/        # Project management pages
│   │   │   ├── priority/        # Priority-based task views
│   │   │   ├── teams/           # Team management
│   │   │   ├── users/           # User management
│   │   │   ├── search/          # Search functionality
│   │   │   ├── settings/        # Settings page
│   │   │   ├── timeline/        # Timeline view
│   │   │   └── ...
│   │   ├── components/          # Reusable React components
│   │   │   ├── Header/
│   │   │   ├── Modal/
│   │   │   ├── Navbar/
│   │   │   ├── Sidebar/
│   │   │   ├── TaskCard/
│   │   │   └── ...
│   │   ├── lib/                 # Utility functions
│   │   └── state/               # Redux store and API
│   ├── .env.local               # Environment variables
│   └── package.json
│
├── server/                      # Express.js backend application
│   ├── prisma/
│   │   ├── migrations/          # Database migrations
│   │   ├── seedData/            # Seed data JSON files
│   │   ├── schema.prisma        # Prisma schema definition
│   │   └── seed.ts              # Database seeding script
│   ├── src/
│   │   ├── controllers/         # Route controllers
│   │   │   ├── projectController.ts
│   │   │   ├── taskController.ts
│   │   │   ├── userController.ts
│   │   │   ├── teamController.ts
│   │   │   └── searchController.ts
│   │   ├── routes/              # API routes
│   │   │   ├── projectRoutes.ts
│   │   │   ├── taskRoutes.ts
│   │   │   ├── userRoutes.ts
│   │   │   ├── teamRoutes.ts
│   │   │   └── searchRoutes.ts
│   │   └── index.ts             # Server entry point
│   ├── .env                     # Environment variables
│   ├── ecosystem.config.js      # PM2 configuration
│   └── package.json
│
└── README.md                    # This file
```

## API Documentation

### Base URL
```
http://localhost:8000
```

### Endpoints

#### Projects

- **GET /projects**
  - Get all projects
  - Response: Array of project objects

- **POST /projects**
  - Create a new project
  - Body: `{ name, description, startDate, endDate }`
  - Response: Created project object

#### Tasks

- **GET /tasks?projectId={id}**
  - Get tasks for a specific project
  - Response: Array of task objects with relations

- **POST /tasks**
  - Create a new task
  - Body: `{ title, description, status, priority, tags, startDate, dueDate, projectId, authorUserId, assignedUserId }`
  - Response: Created task object

- **PATCH /tasks/:taskId/status**
  - Update task status
  - Body: `{ status }`
  - Response: Updated task object

- **GET /tasks/user/:userId**
  - Get tasks for a specific user
  - Response: Array of task objects

#### Users

- **GET /users**
  - Get all users
  - Response: Array of user objects

- **GET /users/:cognitoId**
  - Get a specific user by Cognito ID
  - Response: User object

- **POST /users**
  - Create a new user
  - Body: `{ username, cognitoId, profilePictureUrl, teamId }`
  - Response: Created user object

#### Teams

- **GET /teams**
  - Get all teams
  - Response: Array of team objects with usernames

#### Search

- **GET /search?query={searchTerm}**
  - Search across tasks, projects, and users
  - Response: `{ tasks: [], projects: [], users: [] }`

## Database Schema

### Key Models

#### User
- userId (Primary Key)
- cognitoId (Unique)
- username (Unique)
- profilePictureUrl
- teamId (Foreign Key)

#### Team
- id (Primary Key)
- teamName
- productOwnerUserId
- projectManagerUserId

#### Project
- id (Primary Key)
- name
- description
- startDate
- endDate

#### Task
- id (Primary Key)
- title
- description
- status
- priority
- tags
- startDate
- dueDate
- points
- projectId (Foreign Key)
- authorUserId (Foreign Key)
- assignedUserId (Foreign Key)

#### Attachment
- id (Primary Key)
- fileURL
- fileName
- taskId (Foreign Key)
- uploadedById (Foreign Key)

#### Comment
- id (Primary Key)
- text
- taskId (Foreign Key)
- userId (Foreign Key)

### Relationships

- User → Team (Many-to-One)
- User → Task (One-to-Many as author/assignee)
- Project → Task (One-to-Many)
- Task → Attachment (One-to-Many)
- Task → Comment (One-to-Many)
- Team → Project (Many-to-Many via ProjectTeam)

## AWS Services

### S3 Configuration

The application uses AWS S3 for storing:
- User profile pictures
- Task attachments
- Project images

**Bucket Structure:**
```
pm-s3-images-sanskar/
├── profile-pictures/
│   ├── p1.jpeg
│   ├── p2.jpeg
│   └── ...
├── attachments/
│   ├── i1.jpg
│   ├── i2.jpg
│   └── ...
└── logos/
    └── logo.png
```

### Image URLs

All images are accessed via:
```
https://pm-s3-images-sanskar.s3.us-east-1.amazonaws.com/{filename}
```

## Authentication

The application uses AWS Cognito for authentication:

1. **Sign Up**: Users register with username, email, and password
2. **Sign In**: Users authenticate with email/password
3. **JWT Tokens**: Cognito issues access tokens for API authorization
4. **Protected Routes**: Client-side route protection with authentication checks
5. **API Authorization**: Server validates JWT tokens on protected endpoints

### Authentication Flow

```
1. User signs up/logs in via Amplify UI
2. Cognito issues JWT access token
3. Token stored in client-side session
4. API requests include token in Authorization header
5. Server validates token for protected endpoints
```

## Deployment

### EC2 Deployment

Follow the instructions in `server/aws-ec2-instructions.md` for detailed EC2 setup:

1. Connect to EC2 instance
2. Install Node.js via nvm
3. Install Git and clone repository
4. Install dependencies
5. Configure environment variables
6. Set up PM2 process manager
7. Start the application

### Production Checklist

- [ ] Set `NODE_ENV=production`
- [ ] Configure production database
- [ ] Set up HTTPS/SSL certificates
- [ ] Configure CORS for production domains
- [ ] Set up proper AWS IAM roles and policies
- [ ] Enable CloudWatch logging
- [ ] Configure backup strategies
- [ ] Set up monitoring and alerts
- [ ] Implement rate limiting
- [ ] Configure CDN for static assets

## Contributing

### Development Workflow

1. Create a feature branch from `main`
2. Make your changes
3. Test thoroughly (unit tests, integration tests)
4. Run linting and formatting
5. Submit a pull request
6. Wait for code review and approval

### Code Style

- Follow existing code patterns
- Use TypeScript for type safety
- Write meaningful commit messages
- Comment complex logic
- Keep functions small and focused

### Testing

```bash
# Run tests (when implemented)
npm test

# Run linting
npm run lint
```

## License

This project is licensed under the ISC License.

## Author

Sanskar Sinha

## Acknowledgments

- Next.js for the frontend framework
- Express.js for the backend framework
- Prisma for database ORM
- AWS for cloud services
- Material-UI for UI components

---

For questions or support, please open an issue in the repository.
