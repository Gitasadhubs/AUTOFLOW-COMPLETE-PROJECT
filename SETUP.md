# AutoFlow CI/CD Application Setup Documentation

This document outlines the complete setup process for the AutoFlow CI/CD application, a full-featured GitHub webhook server for automated deployments with OAuth integration, repository management, and deployment logging.

## Project Overview

The application consists of:
- **Backend**: Node.js/Express server with Prisma ORM, SQLite database, GitHub OAuth, and webhook handling
- **Frontend**: Vue.js 3 application with routing and API integration

## Prerequisites

- Node.js & npm (recommended via nvm)
- Git

## Setup Steps Completed

### 1. Environment Configuration

Created `.env` file with the following variables:
```
DATABASE_URL="file:./backend/prisma/dev.db"
GITHUB_CLIENT_ID="dummy"
GITHUB_CLIENT_SECRET="dummy"
GITHUB_CALLBACK_URL="http://localhost:3000/auth/github/callback"
SESSION_SECRET="dummysecret"
HOOK_KEY="dummykey"
HOOK_REF="refs/heads/master"
PORT=3000
```

**Note**: For production, replace dummy values with actual GitHub OAuth credentials and PostgreSQL database URL.

### 2. Backend Setup

#### Dependencies Installation
- Installed all backend dependencies using `npm install`
- Resolved network connectivity issues during installation

#### Database Setup
- Generated Prisma client: `npx prisma generate`
- Pushed database schema to SQLite: `npx prisma db push`
- Database file created at `backend/prisma/dev.db`

#### Database Schema
The application uses the following models:
- **User**: GitHub user information (id, githubId, username, email, avatarUrl)
- **Repo**: Repository details (id, githubId, name, fullName, private, url, userId)
- **DeploymentLog**: Deployment records (id, repoId, status, message, timestamp)

### 3. Frontend Setup

#### Dependencies Installation
- Installed Vue.js dependencies in frontend directory
- Resolved ESLint configuration issues

#### ESLint Configuration Fix
- Removed deprecated `@babel/eslint-parser` from `.eslintrc.js`
- Simplified ESLint configuration to use default parsers

### 4. Application Startup

#### Backend Server
- Command: `npm run dev`
- Server runs on: http://localhost:3000
- Features nodemon for automatic restarts during development

#### Frontend Development Server
- Command: `cd frontend && npm run serve`
- Server runs on: http://localhost:8080
- Hot reload enabled for development

## API Endpoints

### Authentication
- `GET /auth/github` - Initiate GitHub OAuth
- `GET /auth/github/callback` - OAuth callback
- `GET /auth/user` - Get current user
- `GET /auth/logout` - Logout

### Repositories
- `GET /repos` - List user repos
- `POST /repos` - Add new repo
- `DELETE /repos/:id` - Remove repo

### Deployments
- `GET /deploys` - List deployment logs

### Webhook
- `POST /webhook/:key` - GitHub webhook endpoint

## Issues Resolved

1. **Network Connectivity**: Initial npm install failed due to network issues; resolved by retrying
2. **ESLint Parser**: Missing `@babel/eslint-parser` causing compilation errors; fixed by removing custom parser configuration
3. **Database Initialization**: Used `prisma db push` for non-interactive schema deployment

## Current Status

- ✅ Backend server running on port 3000
- ✅ Frontend development server running on port 8080
- ✅ Database schema deployed
- ✅ All dependencies installed
- ✅ ESLint configuration fixed

## Next Steps

1. Configure real GitHub OAuth application
2. Set up production database (PostgreSQL recommended)
3. Configure webhook secrets and deployment scripts
4. Test GitHub integration
5. Deploy to production environment

## Development Commands

```bash
# Backend
npm run dev              # Start development server
npm run db:generate      # Generate Prisma client
npm run db:push          # Push schema changes
npm run db:migrate       # Run migrations

# Frontend
cd frontend
npm run serve            # Start development server
npm run build            # Build for production
npm run lint             # Run ESLint
```

## File Structure

```
github-webhook-autodeploy-master/
├── backend/
│   ├── prisma/
│   │   ├── schema.prisma
│   │   ├── seed.js
│   │   └── dev.db
│   ├── routes/
│   │   ├── auth.js
│   │   ├── repos.js
│   │   ├── deploys.js
│   │   └── webhook.js
│   ├── services/
│   │   ├── githubService.js
│   │   ├── deployService.js
│   │   └── logService.js
│   ├── middleware/
│   │   └── auth.js
│   ├── utils/
│   │   └── helpers.js
│   └── server.js
├── frontend/
│   ├── src/
│   │   ├── components/
│   │   ├── views/
│   │   ├── router/
│   │   └── api/
│   ├── public/
│   └── package.json
├── .env
├── package.json
└── README.md
```

## Notes

- The application uses SQLite for development; switch to PostgreSQL for production
- GitHub OAuth credentials need to be configured for authentication to work
- Webhook endpoints require proper secret configuration for security
- The frontend assumes the backend is running on localhost:3000
