# ShapeFitness - Personalized Workout Generator

A Next.js-based web application that helps users create personalized workout routines based on their available equipment and target muscle groups. The app features an interactive muscle selection interface, intelligent exercise filtering, and progress tracking capabilities.

## Table of Contents

- [Features](#features)
- [Tech Stack](#tech-stack)
- [Project Architecture](#project-architecture)
- [Project Structure](#project-structure)
- [Prerequisites](#prerequisites)
- [Setup Instructions](#setup-instructions)
- [Running the Application](#running-the-application)
- [Environment Variables](#environment-variables)
- [Key Features & Workflows](#key-features--workflows)
- [API Endpoints](#api-endpoints)
- [Component Documentation](#component-documentation)
- [Database Schema](#database-schema)
- [Development Scripts](#development-scripts)
- [Docker Deployment](#docker-deployment)
- [Troubleshooting](#troubleshooting)

## Features

- **Equipment Selection**: Choose from bodyweight, dumbbells, barbells, kettlebells, bands, plates, pull-up bars, and benches
- **Interactive Muscle Targeting**: Visual muscle diagram to select target muscle groups (2-3 recommended)
- **Intelligent Exercise Generation**: Automatically filters and distributes exercises based on selected equipment and muscles
- **Workout Execution**: Step-by-step workout interface with video demonstrations
- **Progress Tracking**: Saves workout history and tracks previous set counts for progressive overload
- **User Profiles**: Public profiles with workout history and sharing capabilities
- **Offline Support**: Works offline with localStorage fallback for unauthenticated users
- **PWA Support**: Progressive Web App capabilities for mobile installation

## Tech Stack

### Frontend
- **Next.js 13.4.1** - React framework with SSR/SSG
- **React 18.2.0** - UI library
- **Mantine 6.0.10** - UI component library
- **SWR 2.1.5** - Data fetching and caching
- **react-beautiful-dnd** - Drag and drop functionality
- **react-hot-toast** - Toast notifications

### Backend
- **Next.js API Routes** - Serverless API endpoints
- **MongoDB 5.4.0** - NoSQL database
- **NextAuth.js 4.22.1** - Authentication (Credentials, Google, Twitter OAuth)

### Additional
- **next-pwa 5.6.0** - Progressive Web App support
- **party-js** - Celebration animations
- **crypto-js** - Password hashing

## Project Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    Client Browser                        │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐ │
│  │   Next.js    │  │   Mantine    │  │     SWR      │ │
│  │   Pages &    │  │   Components │  │   Data Fetch │ │
│  │  Components  │  │              │  │              │ │
│  └──────────────┘  └──────────────┘  └──────────────┘ │
└─────────────────────────────────────────────────────────┘
                        │
                        ▼
┌─────────────────────────────────────────────────────────┐
│              Next.js API Routes (Server)                 │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐ │
│  │   NextAuth   │  │   Exercise   │  │     User     │ │
│  │   Auth API   │  │     API      │  │     API      │ │
│  └──────────────┘  └──────────────┘  └──────────────┘ │
└─────────────────────────────────────────────────────────┘
                        │
                        ▼
┌─────────────────────────────────────────────────────────┐
│              Database Layer (lib/db-helper.js)           │
│  ┌──────────────┐  ┌──────────────┐                    │
│  │   MongoDB    │  │   Connection │                    │
│  │   Client     │  │   Pooling    │                    │
│  └──────────────┘  └──────────────┘                    │
└─────────────────────────────────────────────────────────┘
                        │
                        ▼
┌─────────────────────────────────────────────────────────┐
│                    MongoDB Database                      │
│  ┌──────────────┐  ┌──────────────┐                    │
│  │   Users      │  │  Exercises   │                    │
│  │  Collection  │  │  Collection  │                    │
│  └──────────────┘  └──────────────┘                    │
└─────────────────────────────────────────────────────────┘
```

### State Management

- **SWR**: Server-side data fetching with caching and revalidation
- **localStorage**: Offline support for unauthenticated users
- **React State**: Component-level state management
- **NextAuth Session**: User authentication state

## Project Structure

```
ShapeFitness/
├── components/              # React components
│   ├── Calendar/           # Calendar component for workout history
│   ├── Equipment.js        # Equipment selection interface
│   ├── Exercises/          # Exercise selection and customization
│   │   ├── Exercises.js
│   │   ├── SelectModal.js
│   │   └── utils.js
│   ├── Footer/            # Footer component
│   ├── Header/            # Header with navigation
│   ├── Layout/            # Main layout wrapper
│   ├── Muscles/           # Interactive muscle selection
│   │   ├── Muscles.js
│   │   └── Illustration.js
│   ├── Workout.js         # Active workout execution
│   └── WorkoutTable/      # Workout history display
├── constants/             # Static data
│   ├── quotes.json        # Motivational quotes
│   └── seo.js            # SEO metadata
├── containers/            # Container components
│   └── user/             # User profile components
├── docker/               # Docker configuration
│   ├── docker-compose.yml
│   ├── dockerfile
│   └── dockerfile-mongo-seed
├── lib/                  # Database and utilities
│   ├── db-helper.js      # MongoDB connection and queries
│   └── dump/            # Database dumps
├── pages/                # Next.js pages and API routes
│   ├── api/             # API endpoints
│   │   ├── auth/        # NextAuth configuration
│   │   ├── exercises.js # Exercise filtering API
│   │   ├── muscles.js   # Muscle group data API
│   │   ├── user/        # User CRUD operations
│   │   └── workout.js   # Workout sharing API
│   ├── _app.js          # App wrapper with providers
│   ├── _document.js     # Custom document
│   ├── index.js         # Main workout generator page
│   ├── profile.js       # User profile page
│   └── u/              # Public user profiles
├── public/              # Static assets
│   ├── equipment/      # Equipment images
│   └── icons/          # App icons
├── scripts/            # Utility scripts
│   ├── initDb.js      # Database initialization
│   └── scrape.js      # Data scraping (if applicable)
├── utils/             # Custom hooks and utilities
│   ├── useAccount.js  # User account management hook
│   ├── useWorkout.js  # Workout data fetching hook
│   └── ...
├── next.config.js     # Next.js configuration
├── package.json      # Dependencies and scripts
└── README.md         # This file
```

## Prerequisites

Before you begin, ensure you have the following installed:

- **Node.js** 16.x or higher
- **npm** or **yarn** package manager
- **MongoDB** 5.0+ (local installation or MongoDB Atlas account)
- **Git** (for cloning the repository)

## Setup Instructions

### 1. Clone the Repository

```bash
git clone <repository-url>
cd ShapeFitness
```

### 2. Install Dependencies

```bash
npm install
# or
yarn install
```

### 3. Set Up Environment Variables

Create a `.env.local` file in the root directory with the following variables:

```env
# MongoDB Connection
MONGODB_URI=mongodb://localhost:27017/shapefitness
# or for MongoDB Atlas:
# MONGODB_URI=mongodb+srv://username:password@cluster.mongodb.net/database

# NextAuth Configuration
NEXTAUTH_URL=http://localhost:3000
NEXTAUTH_SECRET=your-secret-key-here-generate-with-openssl-rand-base64-32

# Password Hashing
PASSWORD_HASH_SECRET=your-password-hash-secret-here

# Google OAuth (Optional)
GOOGLE_CLIENT_ID=your-google-client-id
GOOGLE_CLIENT_SECRET=your-google-client-secret

# Twitter OAuth (Optional)
TWITTER_CLIENT_ID=your-twitter-client-id
TWITTER_CLIENT_SECRET=your-twitter-client-secret
```

**Generating Secrets:**

```bash
# Generate NEXTAUTH_SECRET
openssl rand -base64 32

# Generate PASSWORD_HASH_SECRET
openssl rand -base64 32
```

### 4. Initialize Database

If you need to seed the database with exercises:

1. Ensure MongoDB is running
2. Prepare your exercise data file (JSON format)
3. Run the initialization script:

```bash
node scripts/initDb.js
```

**Note:** The script expects a `data.json` file in the root directory with exercise data.

### 5. Start Development Server

```bash
npm run dev
```

The application will be available at `http://localhost:3000`

## Running the Application

### Development Mode

```bash
npm run dev
```

Starts the Next.js development server with hot-reloading enabled.

### Production Build

```bash
# Build the application
npm run build

# Start the production server
npm start
```

### Docker Deployment

The project includes Docker configuration for containerized deployment:

```bash
cd docker
docker-compose up
```

This will:
- Build the Next.js application
- Start MongoDB container
- Seed the database (if configured)
- Expose the app on port 3000

## Environment Variables

| Variable | Required | Description |
|----------|----------|-------------|
| `MONGODB_URI` | Yes | MongoDB connection string |
| `NEXTAUTH_URL` | Yes | Base URL of your application |
| `NEXTAUTH_SECRET` | Yes | Secret key for NextAuth session encryption |
| `PASSWORD_HASH_SECRET` | Yes | Secret key for password hashing |
| `GOOGLE_CLIENT_ID` | No | Google OAuth client ID |
| `GOOGLE_CLIENT_SECRET` | No | Google OAuth client secret |
| `TWITTER_CLIENT_ID` | No | Twitter OAuth client ID |
| `TWITTER_CLIENT_SECRET` | No | Twitter OAuth client secret |

## Key Features & Workflows

### 1. Equipment Selection

Users start by selecting available equipment from a visual grid:
- Bodyweight (none)
- Dumbbell
- Barbell
- Kettlebell
- Band
- Plate
- Pull-up bar
- Bench

**Component:** `components/Equipment.js`

The selected equipment is stored in the user's profile and used to filter available exercises.

### 2. Muscle Targeting

Users interact with an anatomical muscle diagram to select target muscle groups:
- Visual feedback shows exercise count per muscle group
- Filters exercises based on selected equipment
- Recommends 2-3 muscle groups for balanced workouts

**Component:** `components/Muscles/Muscles.js`

### 3. Exercise Generation

The app intelligently generates a workout:
- Filters exercises matching selected equipment and muscles
- Distributes exercises evenly across selected muscle groups
- Defaults to ~6 exercises total (adjusted based on muscle count)
- Allows customization: shuffle, replace, or add exercises

**Component:** `components/Exercises/Exercises.js`

**Algorithm:**
1. Query exercises matching equipment and muscle filters
2. Shuffle available exercises
3. Distribute evenly across muscle groups (defaultCount = 6 / muscleCount)
4. Sort for high distribution across muscles

### 4. Workout Execution

Step-by-step workout interface:
- Video demonstrations for each exercise
- Set and rep tracking
- Previous workout data shown for progressive overload
- Completion tracking with celebration animations
- Save progress automatically

**Component:** `components/Workout.js`

### 5. Progress Tracking

- Workout history saved to user profile
- Previous set counts displayed during workouts
- Calendar view of workout history
- Public profile pages for sharing

**Components:** `components/WorkoutTable`, `components/Calendar`

### 6. User Authentication

Multiple authentication methods:
- **Credentials**: Email/password with SHA-256 hashing
- **Google OAuth**: Social login
- **Twitter OAuth**: Social login
- **Offline Mode**: localStorage for unauthenticated users

**Implementation:** `pages/api/auth/[...nextauth].js`

## API Endpoints

### Authentication

**`GET/POST /api/auth/[...nextauth]`**
- NextAuth.js catch-all route
- Handles sign-in, sign-out, session management
- Supports Credentials, Google, and Twitter providers

### Exercises

**`GET /api/exercises`**
- Query parameters:
  - `equipment` (comma-separated): Filter by equipment
  - `muscles` (comma-separated): Filter by muscle groups
  - `difficulty` (comma-separated): Filter by difficulty level
- Returns: Array of exercise objects matching filters

**`POST /api/exercises`**
- Body: `{ ids: string[] }`
- Returns: Array of exercise objects by IDs

### Muscles

**`GET /api/muscles`**
- Query parameters:
  - `equipment` (comma-separated): Filter by equipment
- Returns: Muscle groups with exercise counts per difficulty level

### User

**`GET /api/user`**
- Requires: Authenticated session
- Returns: Current user data (sanitized, no password)

**`PUT /api/user`**
- Requires: Authenticated session
- Body: User update object
- Returns: Updated user data

**`GET /api/user/[slug]`**
- Public endpoint
- Returns: Public user profile by slug

### Workout

**`GET /api/workout?id=<workout-id>`**
- Returns: Shared workout by ID
- Error: 404 if workout not found

## Component Documentation

### Equipment Component

**File:** `components/Equipment.js`

Toggleable equipment selection buttons with images. Each button:
- Shows equipment image from `/public/equipment/{id}.png`
- Toggles between selected (light) and unselected (outline) states
- Updates user's equipment array via `updateEquipment` callback

**Props:**
- `equipment`: Array of selected equipment IDs
- `updateEquipment`: Callback to update equipment array

### Muscles Component

**File:** `components/Muscles/Muscles.js`

Interactive muscle selection with anatomical illustration:
- Fetches available muscles based on equipment
- Shows exercise count per muscle group
- Filters by difficulty level (optional)
- Visual feedback on selection

**Props:**
- `muscles`: Array of selected muscle IDs
- `setMuscles`: Callback to update muscles
- `equipment`: Selected equipment array
- `setDifficulties`: Callback for difficulty filter
- `difficulties`: Selected difficulty levels

### Exercises Component

**File:** `components/Exercises/Exercises.js`

Exercise list and customization interface:
- Auto-generates workout on first load
- Allows shuffling individual exercises
- Opens modal to select alternative exercises
- Drag-and-drop reordering (via react-beautiful-dnd)

**Props:**
- `equipment`: Selected equipment
- `muscles`: Selected muscles
- `workout`: Current workout array
- `setWorkout`: Callback to update workout
- `difficulties`: Selected difficulty levels

### Workout Component

**File:** `components/Workout.js`

Active workout execution interface:
- Step-by-step exercise display
- Video demonstrations
- Set and rep input tracking
- Previous workout data display
- Completion tracking with animations
- Progress saving

**Props:**
- `workout`: Array of exercises
- `updateProgress`: Callback to save progress
- `user`: User object with workout history

### WorkoutTable Component

**File:** `components/WorkoutTable/WorkoutTable.js`

Displays workout history in a table format:
- Lists all saved workouts
- Shows workout date and exercise count
- Allows repeating or sharing workouts

## Database Schema

### Users Collection

```javascript
{
  _id: ObjectId,
  email: string,           // Unique user email
  password: string,        // SHA-256 hashed password (credentials only)
  slug: string,            // Unique URL-friendly identifier
  provider: string,        // 'credentials', 'google', or 'twitter'
  equipment: string[],     // Array of equipment IDs
  workouts: [              // Array of workout objects
    {
      id: string,          // UUID
      created_at: string,   // ISO date string
      exercises: [
        {
          id: string,      // Exercise _id reference
          completed: boolean,
          sets: string[]   // Array of set/rep strings
        }
      ]
    }
  ]
}
```

### Exercises Collection

```javascript
{
  _id: ObjectId,
  title: string,           // Exercise name
  targets: string[],       // Target muscle groups
  equipment: string[],     // Required equipment
  difficulty: string,      // 'Beginner', 'Intermediate', or 'Advanced'
  category: string,        // Exercise category
  videos: string[],        // Array of video URLs
  // ... other exercise metadata
}
```

## Development Scripts

| Command | Description |
|---------|-------------|
| `npm run dev` | Start development server on `http://localhost:3000` |
| `npm run build` | Create optimized production build |
| `npm start` | Start production server (requires build first) |
| `npm run lint` | Run ESLint to check code quality |
| `npm run lint:check` | Check linting without fixing |
| `npm run lint:fix` | Fix linting errors automatically |
| `npm run format` | Format code with Prettier |

## Docker Deployment

### Docker Compose

The project includes a `docker-compose.yml` file for easy deployment:

```yaml
services:
  shapefitness:      # Next.js application
  mongodb:         # MongoDB database
  mongoseed:        # Database seeding service
```

### Running with Docker

```bash
cd docker
docker-compose up -d
```

### Environment Variables in Docker

Set environment variables in `docker-compose.yml` or use a `.env` file:

```env
MONGODB_URI=mongodb://mongodb:27017/shapefitness
NEXTAUTH_URL=http://localhost:3000
NEXTAUTH_SECRET=your-secret
PASSWORD_HASH_SECRET=your-secret
```

## Troubleshooting

### MongoDB Connection Issues

**Error:** `MongoServerError: connection refused`

**Solutions:**
1. Ensure MongoDB is running: `mongod` or check MongoDB service
2. Verify `MONGODB_URI` in `.env.local` is correct
3. For MongoDB Atlas, check IP whitelist and credentials
4. Test connection: `mongosh <your-connection-string>`

### NextAuth Session Issues

**Error:** `NEXTAUTH_SECRET is not set`

**Solution:** Ensure `NEXTAUTH_SECRET` is set in `.env.local` and restart the server.

### Exercise Data Not Loading

**Error:** No exercises appear in the app

**Solutions:**
1. Check if exercises collection exists in MongoDB
2. Verify exercise data was seeded: `db.exercises.countDocuments()`
3. Check API endpoint: `curl http://localhost:3000/api/exercises?equipment=none&muscles=chest`
4. Review browser console for API errors

### Build Errors

**Error:** `Module not found` or build failures

**Solutions:**
1. Delete `node_modules` and `.next` directories
2. Clear npm cache: `npm cache clean --force`
3. Reinstall: `npm install`
4. Check Node.js version: `node --version` (should be 16+)

### OAuth Provider Issues

**Error:** OAuth login fails

**Solutions:**
1. Verify OAuth credentials in provider dashboard
2. Check redirect URIs match `NEXTAUTH_URL`
3. Ensure environment variables are set correctly
4. Review NextAuth logs for detailed error messages

### Port Already in Use

**Error:** `Port 3000 is already in use`

**Solutions:**
1. Kill process on port 3000: `npx kill-port 3000` (Windows: `netstat -ano | findstr :3000`)
2. Use different port: `PORT=3001 npm run dev`
3. Update `NEXTAUTH_URL` if using different port

## Contributing

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/your-feature`
3. Make your changes
4. Run linting: `npm run lint:fix`
5. Format code: `npm run format`
6. Commit changes: `git commit -m 'Add feature'`
7. Push to branch: `git push origin feature/your-feature`
8. Open a Pull Request

## License

See [LICENSE](LICENSE) file for details.

## Support

For issues, questions, or contributions, please open an issue on the repository.

---

**Built with ❤️ using Next.js and MongoDB**
