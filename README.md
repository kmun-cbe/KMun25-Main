# K-MUN25 - Kumaraguru Model United Nations 2025

A comprehensive website for the Kumaraguru Model United Nations 2025 event, built with React, TypeScript, and Tailwind CSS for the frontend, and Node.js with Express for the backend.

## 📋 Project Structure

```
├── frontend/          # React frontend application
│   ├── src/          # Source code
│   ├── public/       # Static assets
│   └── package.json  # Frontend dependencies
├── backend/          # Node.js backend application
│   ├── src/          # Source code
│   ├── prisma/       # Database schema
│   ├── supabase/     # Supabase migrations
│   └── package.json  # Backend dependencies
└── README.md
```

## 🚀 Getting Started

### Prerequisites

- Node.js (v18 or higher)
- npm or yarn
- PostgreSQL database

### Frontend Setup

1. Navigate to the frontend directory:
   ```bash
   cd frontend
   ```

2. Install dependencies:
   ```bash
   npm install
   ```

3. Start the development server:
   ```bash
   npm run dev
   ```

The frontend will be available at `http://localhost:5173`

### Backend Setup

1. Navigate to the backend directory:
   ```bash
   cd backend
   ```

2. Install dependencies:
   ```bash
   npm install
   ```

3. Copy the environment file:
   ```bash
   cp .env.example .env
   ```

4. Update the `.env` file with your configuration

5. Generate Prisma client:
   ```bash
   npm run db:generate
   ```

6. Run database migrations:
   ```bash
   npm run db:migrate
   ```

7. Seed the database (optional):
   ```bash
   npm run db:seed
   ```

8. Start the development server:
   ```bash
   npm run dev
   ```

The backend will be available at `http://localhost:3001`

## 📋 Features

### Frontend Features
- ✅ **Responsive Design** - Works on desktop and mobile devices
- ✅ **User Authentication** - Login/register with role-based access
- ✅ **Committee Information** - Detailed committee pages
- ✅ **Registration System** - Multi-step registration form
- ✅ **Dashboard System** - Role-specific dashboards
- ✅ **Resource Center** - Download guides and documents
- ✅ **Gallery** - Committee and event galleries
- ✅ **Contact System** - Contact form with validation

### Backend Features
- ✅ **RESTful API** - Clean API endpoints
- ✅ **Authentication & Authorization** - JWT-based auth
- ✅ **Database Integration** - PostgreSQL with Prisma ORM
- ✅ **File Upload** - Document and image upload
- ✅ **Email Service** - Automated email notifications
- ✅ **Payment Integration** - Razorpay payment gateway
- ✅ **Admin Panel** - Administrative functions

## 🎨 Tech Stack

### Frontend
- **React 18** - UI library
- **TypeScript** - Type safety
- **Tailwind CSS** - Styling
- **Framer Motion** - Animations
- **React Router** - Navigation
- **React Hook Form** - Form handling
- **Lucide React** - Icons

### Backend
- **Node.js** - Runtime
- **Express.js** - Web framework
- **Prisma** - Database ORM
- **PostgreSQL** - Database
- **JWT** - Authentication
- **Multer** - File uploads
- **Nodemailer** - Email service
- **Razorpay** - Payment processing

## 📱 Available Scripts

### Frontend Scripts
- `npm run dev` - Start development server
- `npm run build` - Build for production
- `npm run preview` - Preview production build
- `npm run lint` - Run ESLint

### Backend Scripts
- `npm run dev` - Start development server
- `npm run start` - Start production server
- `npm run db:generate` - Generate Prisma client
- `npm run db:push` - Push schema to database
- `npm run db:migrate` - Run database migrations
- `npm run db:studio` - Open Prisma Studio
- `npm run db:seed` - Seed database with sample data

## 🔧 Configuration

### Environment Variables

Create a `.env` file in the backend directory with the following variables:

```env
DATABASE_URL="postgresql://username:password@localhost:5432/database_name"
JWT_SECRET="your-jwt-secret"
RAZORPAY_KEY_ID="your-razorpay-key"
RAZORPAY_KEY_SECRET="your-razorpay-secret"
SMTP_HOST="smtp.gmail.com"
SMTP_PORT=587
SMTP_USER="your-email@gmail.com"
SMTP_PASS="your-app-password"
NODE_ENV="development"
PORT=3001
```

## 🚀 Deployment

### Frontend Deployment
1. Build the frontend:
   ```bash
   cd frontend && npm run build
   ```

2. Deploy the `dist` folder to your hosting service

### Backend Deployment
1. Set up your production database
2. Update environment variables
3. Run migrations:
   ```bash
   npm run db:migrate
   ```
4. Start the server:
   ```bash
   npm start
   ```

## 📄 License

This project is licensed under the MIT License.

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Submit a pull request

## 📞 Support

For support, email support@kmun2025.com or create an issue in the repository.