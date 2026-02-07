# Anuja AI

A fully functional, visually stunning AI assistant website featuring intelligent chat conversations and AI-powered image generation using Pollinations AI.

## Features

- **AI Chat Interface**: Engage in intelligent conversations with an AI assistant
- **AI Image Generator**: Create stunning images from text prompts using Pollinations AI
- **User Authentication**: Secure email/password and Google OAuth login via Firebase
- **Personal Gallery**: Save and manage your AI-generated images
- **Dark/Light Mode**: Beautiful themes with smooth transitions
- **Real-time Updates**: Live chat and gallery updates using Firebase Firestore
- **Responsive Design**: Fully optimized for all devices
- **Smooth Animations**: Enhanced UX with Framer Motion

## Tech Stack

- **Frontend**: React 18 + TypeScript + Vite
- **Styling**: Tailwind CSS
- **Animations**: Framer Motion
- **Backend**: Firebase (Auth, Firestore, Storage)
- **AI Integration**: Pollinations AI
- **Icons**: Lucide React
- **Routing**: React Router v6

## Setup Instructions

### 1. Clone and Install

```bash
npm install
```

### 2. Firebase Configuration

1. Create a Firebase project at [Firebase Console](https://console.firebase.google.com/)
2. Enable **Authentication** (Email/Password and Google)
3. Create a **Firestore Database** (start in production mode)
4. Enable **Storage**
5. Copy your Firebase config

### 3. Environment Variables

Create a `.env` file in the root directory:

```env
VITE_FIREBASE_API_KEY=your_api_key
VITE_FIREBASE_AUTH_DOMAIN=your_project.firebaseapp.com
VITE_FIREBASE_PROJECT_ID=your_project_id
VITE_FIREBASE_STORAGE_BUCKET=your_project.appspot.com
VITE_FIREBASE_MESSAGING_SENDER_ID=your_sender_id
VITE_FIREBASE_APP_ID=your_app_id
```

### 4. Firestore Security Rules

Add these rules to your Firestore:

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /chats/{chatId} {
      allow read, write: if request.auth != null && request.auth.uid == resource.data.userId;
      allow create: if request.auth != null && request.auth.uid == request.resource.data.userId;
    }

    match /images/{imageId} {
      allow read, write: if request.auth != null && request.auth.uid == resource.data.userId;
      allow create: if request.auth != null && request.auth.uid == request.resource.data.userId;
    }
  }
}
```

### 5. Storage Security Rules

Add these rules to your Firebase Storage:

```javascript
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /images/{userId}/{allPaths=**} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
  }
}
```

### 6. Run Development Server

```bash
npm run dev
```

### 7. Build for Production

```bash
npm run build
```

## Firebase Hosting Deployment

1. Install Firebase CLI:
```bash
npm install -g firebase-tools
```

2. Login to Firebase:
```bash
firebase login
```

3. Initialize Firebase Hosting:
```bash
firebase init hosting
```

4. Build and deploy:
```bash
npm run build
firebase deploy
```

## Features Overview

### Authentication
- Email/password registration and login
- Google OAuth integration
- Protected routes for authenticated users
- Persistent sessions

### AI Chat
- Real-time conversation interface
- Message history stored in Firestore
- Smooth animations and loading states
- User-friendly chat bubbles

### Image Generator
- Text-to-image using Pollinations AI
- High-quality 1024x1024 images
- Download functionality
- Save to personal gallery

### Dashboard
- Tabbed interface (Chat, Generate, Gallery)
- View all saved images
- Delete images from gallery
- Real-time updates

### Theme System
- Dark and light modes
- Persistent theme preference
- Smooth transitions
- Beautiful gradients and colors

## Project Structure

```
src/
├── components/
│   ├── AIChat.tsx          # AI chat interface
│   ├── Dashboard.tsx       # Main dashboard
│   ├── Home.tsx           # Landing page
│   ├── ImageGenerator.tsx # Image generation
│   ├── Login.tsx          # Login page
│   ├── Navigation.tsx     # Navigation bar
│   ├── ProtectedRoute.tsx # Route protection
│   └── Register.tsx       # Registration page
├── contexts/
│   ├── AuthContext.tsx    # Authentication state
│   └── ThemeContext.tsx   # Theme management
├── config/
│   └── firebase.ts        # Firebase configuration
├── App.tsx               # Main app component
├── main.tsx             # App entry point
└── index.css            # Global styles
```

## Performance Optimizations

- Lazy loading for images
- Optimized bundle with Vite
- Firebase caching
- Efficient re-renders with React
- Smooth animations without layout shifts

## Security Features

- Firebase Authentication
- Firestore security rules
- Storage security rules
- Protected API endpoints
- Sanitized user inputs
- HTTPS enforced

## Browser Support

- Chrome (latest)
- Firefox (latest)
- Safari (latest)
- Edge (latest)

## License

MIT License - feel free to use this project for learning or production.

## Credits

- **AI Images**: [Pollinations AI](https://pollinations.ai/)
- **Backend**: [Firebase](https://firebase.google.com/)
- **Icons**: [Lucide React](https://lucide.dev/)
- **Animations**: [Framer Motion](https://www.framer.com/motion/)

## Support

For issues or questions, please open an issue on GitHub or contact the development team.

---

Built with love using React, TypeScript, Firebase, and AI technology.
