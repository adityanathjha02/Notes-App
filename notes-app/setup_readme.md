# Personal Notes App - Complete Setup Guide

A full-stack notes management application with Google OAuth authentication.

## Quick Start (5 minutes)

### 1. Prerequisites

- Node.js (v14 or higher)
- MongoDB (local installation or MongoDB Atlas)
- Google Cloud Console account

### 2. Google OAuth Setup

1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Create new project or select existing one
3. Enable "Google+ API"
4. Navigate to "Credentials" → "Create Credentials" → "OAuth 2.0 Client ID"
5. Configure OAuth consent screen if prompted
6. Set Authorized JavaScript origins:
   - `http://localhost:3000`
   - `http://localhost:5000`
7. Set Authorized redirect URIs:
   - `http://localhost:5000/auth/google/callback`
8. Copy your Client ID and Client Secret

### 3. Project Setup

#### Backend Setup

```bash
# Create project structure
mkdir notes-app && cd notes-app
mkdir backend && cd backend

# Initialize and install dependencies
npm init -y
npm install express mongoose cors dotenv passport passport-google-oauth20 jsonwebtoken bcryptjs nodemailer cookie-parser

# Create required folders
mkdir models routes middleware
```

#### Create Environment File

Create `backend/.env`:

```
PORT=5000
MONGODB_URI=mongodb://localhost:27017/notesapp
JWT_SECRET=your_super_secret_jwt_key_here
GOOGLE_CLIENT_ID=your_google_client_id_here
GOOGLE_CLIENT_SECRET=your_google_client_secret_here
```



#### Frontend Setup

```bash
# From project root
cd ..
mkdir frontend && cd frontend

# Create React app
npx create-react-app . --template minimal
npm install axios

# Create required folders
mkdir src/components src/styles
```

### 4. File Structure

Copy the provided files into this structure:

```
notes-app/
├── backend/
│   ├── .env
│   ├── package.json
│   ├── server.js
│   ├── models/
│   │   ├── User.js
│   │   └── Note.js
│   ├── routes/
│   │   └── notes.js
│   └── middleware/
│       └── auth.js
└── frontend/
    ├── package.json
    ├── public/
    │   └── index.html
    └── src/
        ├── index.js
        ├── App.js
        ├── components/
        │   ├── Auth.js
        │   ├── Dashboard.js
        │   └── NoteItem.js
        └── styles/
            └── App.css
```

### 5. MongoDB Setup

#### Option A: Local MongoDB

```bash
# Install MongoDB locally
# macOS
brew install mongodb/brew/mongodb-community

# Start MongoDB
brew services start mongodb/brew/mongodb-community
```

#### Option B: MongoDB Atlas (Cloud)

1. Sign up at [MongoDB Atlas](https://cloud.mongodb.com/)
2. Create a free cluster
3. Get connection string and replace `MONGODB_URI` in `.env`

### 6. Run the Application

#### Start Backend (Terminal 1)

```bash
cd backend
npm start
```

Server runs on http://localhost:5000

#### Start Frontend (Terminal 2)

```bash
cd frontend
npm start
```

App opens at http://localhost:3000

## Features

### Authentication

- Google OAuth 2.0 login
- JWT token-based sessions
- Automatic redirect after login

### Notes Management

- Create notes with title and content
- View all personal notes
- Delete notes
- Responsive grid layout
- Real-time updates

### Security

- Protected API endpoints
- User-specific note access
- JWT token validation
- CORS enabled

## API Endpoints

| Method | Endpoint                | Description           | Auth Required |
| ------ | ----------------------- | --------------------- | ------------- |
| GET    | `/auth/google`          | Initiate Google OAuth | No            |
| GET    | `/auth/google/callback` | OAuth callback        | No            |
| GET    | `/api/notes`            | Get user notes        | Yes           |
| POST   | `/api/notes`            | Create new note       | Yes           |
| DELETE | `/api/notes/:id`        | Delete note           | Yes           |

## Technical Decisions

### Frontend

- **React**: Simple component-based architecture
- **Axios**: HTTP client with interceptors for auth tokens
- **CSS**: Vanilla CSS for lightweight styling
- **State Management**: React hooks (useState, useEffect)

### Backend

- **Express.js**: Minimal web framework
- **Passport.js**: Google OAuth strategy
- **JWT**: Stateless authentication
- **MongoDB**: Document-based storage for notes
- **Mongoose**: ODM for schema validation

### Authentication Flow

#### Email/Password Flow

1. User registers with name, email, password
2. System generates 6-digit OTP, stores in database
3. OTP sent to email (or logged to console for demo)
4. User enters OTP to verify email
5. JWT token generated and set as HTTP-only cookie
6. User can login with email/password afterwards

#### Google OAuth Flow

1. User clicks "Login with Google"
2. Redirect to Google OAuth
3. Google redirects back with auth code
4. Backend exchanges code for user info
5. JWT token generated and set as HTTP-only cookie
6. User redirected to dashboard

### Security Features

- **Password Hashing**: bcrypt with 12 rounds
- **JWT Tokens**: HTTP-only cookies (no localStorage)
- **OTP Verification**: Time-limited email verification
- **Protected Routes**: All note operations require authentication
- **User Isolation**: Users can only access their own notes
- **CORS**: Configured for specific origins

## Troubleshooting

### Common Issues

#### "MongoDB connection error"

- Ensure MongoDB is running locally, or check Atlas connection string
- Verify network access in MongoDB Atlas

#### "OAuth error: redirect_uri_mismatch"

- Check Google Console redirect URIs match exactly
- Ensure no trailing slashes in URLs

#### "CORS error"

- Backend CORS is configured for localhost:3000
- Ensure frontend runs on port 3000

#### "Token invalid"

- Check JWT_SECRET is set in `.env`
- Clear localStorage and login again

### Environment Variables Checklist

- [ ] `MONGODB_URI` - Database connection
- [ ] `JWT_SECRET` - Random secure string
- [ ] `GOOGLE_CLIENT_ID` - From Google Console
- [ ] `GOOGLE_CLIENT_SECRET` - From Google Console

## Deployment Notes

### Frontend

- Build: `npm run build`
- Update OAuth origins for production domain
- Set production API URL

### Backend

- Set production environment variables
- Configure production MongoDB
- Update CORS origins
- Use process.env.PORT for hosting platforms

## Development Commands

```bash
# Backend development with auto-restart
cd backend && npm install -g nodemon && npm run dev

# Frontend development
cd frontend && npm start

# Build for production
cd frontend && npm run build
```

## Security Considerations

- JWT tokens expire (add refresh token for production)
- Environment variables never committed to git
- HTTPS required for production OAuth
- Input validation on all endpoints
- Rate limiting recommended for production

## Next Steps / Extensions

- [ ] Note editing functionality
- [ ] Note categories/tags
- [ ] Search functionality
- [ ] Rich text editor
- [ ] Multiple OAuth providers
- [ ] Email notifications
- [ ] Note sharing
- [ ] Mobile app

This setup provides a solid foundation for a full-stack notes application with modern authentication and security practices.
