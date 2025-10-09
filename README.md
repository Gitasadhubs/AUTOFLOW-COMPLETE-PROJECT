# AutoFlow CI/CD Backend

A full-featured GitHub webhook server for automated deployments with OAuth integration, repository management, and deployment logging.

## Features

- GitHub OAuth authentication
- Repository management (add, list, delete repos)
- Automated deployments on GitHub push
- Deployment logs and history
- REST API for frontend integration
- Prisma ORM with PostgreSQL

## Prerequisites

- Node.js & npm (recommended via [nvm](https://github.com/creationix/nvm))
- PostgreSQL database
- GitHub OAuth App (Client ID & Secret)

## Installation

1. Clone the repository:
```bash
git clone https://github.com/shov/github-webhook-autodeploy.git
cd github-webhook-autodeploy
```

2. Install dependencies:
```bash
npm install
```

3. Set up environment variables:
```bash
cp .env.example .env
# Edit .env with your values
```

4. Set up the database:
```bash
npx prisma generate
npx prisma migrate dev
```

5. Start the server:
```bash
npm run dev  # for development
# or
npm start    # for production
```

## Environment Variables

- `DATABASE_URL`: PostgreSQL connection string
- `GITHUB_CLIENT_ID`: GitHub OAuth Client ID
- `GITHUB_CLIENT_SECRET`: GitHub OAuth Client Secret
- `GITHUB_CALLBACK_URL`: OAuth callback URL
- `SESSION_SECRET`: Session secret for Passport
- `HOOK_KEY`: Webhook secret key
- `HOOK_REF`: Branch to trigger on (e.g., refs/heads/master)
- `PORT`: Server port (default 3000)

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

## Usage with PM2

1. Install PM2: `npm i pm2 -g`
2. Rename `ecosystem.json.example` to `ecosystem.json`
3. Edit `ecosystem.json` with your env vars
4. Run: `pm2 start ecosystem.json`

## GitHub Setup

- Create a GitHub OAuth App in your GitHub settings
- Set Authorization callback URL to `http://localhost:3000/auth/github/callback`
- For webhooks, set Payload URL to `http://yourdomain.com/webhook/secreturlpart`
- Content type: `application/json`
- Secret: (optional, but recommended)

## Development

- `npm run dev` - Start with nodemon
- `npm run db:generate` - Generate Prisma client
- `npm run db:migrate` - Run migrations
- `npm run db:push` - Push schema changes
