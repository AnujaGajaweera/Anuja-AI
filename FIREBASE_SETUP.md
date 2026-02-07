# Firebase Setup Guide for Anuja AI

This guide will walk you through setting up Firebase for the Anuja AI project.

## Step 1: Create a Firebase Project

1. Go to the [Firebase Console](https://console.firebase.google.com/)
2. Click "Add Project" or "Create a Project"
3. Enter your project name (e.g., "Anuja AI")
4. Accept the terms and click "Continue"
5. Choose whether to enable Google Analytics (optional)
6. Click "Create Project"

## Step 2: Register Your Web App

1. In your Firebase project, click the **Web icon** (`</>`) to add a web app
2. Enter an app nickname (e.g., "Anuja AI Web")
3. Check "Also set up Firebase Hosting" (optional)
4. Click "Register app"
5. Copy your Firebase configuration object

## Step 3: Configure Environment Variables

1. Create a `.env` file in your project root
2. Add your Firebase configuration:

```env
VITE_FIREBASE_API_KEY=AIzaSyXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
VITE_FIREBASE_AUTH_DOMAIN=your-project.firebaseapp.com
VITE_FIREBASE_PROJECT_ID=your-project-id
VITE_FIREBASE_STORAGE_BUCKET=your-project.appspot.com
VITE_FIREBASE_MESSAGING_SENDER_ID=123456789012
VITE_FIREBASE_APP_ID=1:123456789012:web:abcdef123456
```

3. Add `.env` to your `.gitignore` file (already configured)

## Step 4: Enable Authentication

1. In Firebase Console, go to **Authentication**
2. Click "Get Started"
3. Enable **Email/Password**:
   - Click on "Email/Password" provider
   - Toggle "Enable"
   - Click "Save"

4. Enable **Google Sign-In**:
   - Click on "Google" provider
   - Toggle "Enable"
   - Enter your project support email
   - Click "Save"

## Step 5: Create Firestore Database

1. In Firebase Console, go to **Firestore Database**
2. Click "Create Database"
3. Choose a location (select closest to your users)
4. Start in **Production Mode**
5. Click "Enable"

### Set Up Firestore Security Rules

1. Go to **Firestore Database** > **Rules** tab
2. Replace the default rules with:

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

3. Click "Publish"

### Create Firestore Collections

The collections will be created automatically when users start using the app:
- `chats` - Stores chat messages
- `images` - Stores generated image metadata

## Step 6: Enable Firebase Storage

1. In Firebase Console, go to **Storage**
2. Click "Get Started"
3. Start in **Production Mode**
4. Choose a location (same as Firestore)
5. Click "Done"

### Set Up Storage Security Rules

1. Go to **Storage** > **Rules** tab
2. Replace the default rules with:

```javascript
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /images/{userId}/{allPaths=**} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }

    match /{allPaths=**} {
      allow read: if request.auth != null;
    }
  }
}
```

3. Click "Publish"

## Step 7: (Optional) Set Up Firebase Hosting

If you want to deploy your app to Firebase Hosting:

1. Install Firebase CLI:
```bash
npm install -g firebase-tools
```

2. Login to Firebase:
```bash
firebase login
```

3. Initialize Firebase in your project:
```bash
firebase init
```

4. Select **Hosting** using arrow keys and spacebar
5. Choose your existing project
6. Set public directory to: `dist`
7. Configure as a single-page app: `Yes`
8. Set up automatic builds with GitHub: `No` (or Yes if you want CI/CD)

### Deploy to Firebase Hosting

```bash
npm run build
firebase deploy
```

Your app will be live at: `https://your-project-id.web.app`

## Step 8: Testing Your Setup

1. Start the development server:
```bash
npm run dev
```

2. Test the following features:
   - **Sign up** with email and password
   - **Sign in** with Google
   - **Send chat messages** (they should save to Firestore)
   - **Generate an image** (it should appear and save)
   - **View gallery** (your saved images should appear)
   - **Delete an image** from gallery
   - **Toggle dark/light mode**
   - **Logout and login** again

## Firestore Data Structure

### Chats Collection

```javascript
{
  userId: "user_uid",
  text: "Hello, AI!",
  sender: "user" | "ai",
  timestamp: Firestore.Timestamp
}
```

### Images Collection

```javascript
{
  userId: "user_uid",
  url: "https://image.pollinations.ai/...",
  prompt: "A futuristic city",
  timestamp: Firestore.Timestamp
}
```

## Security Best Practices

1. **Never commit `.env` file** to version control
2. **Use Firebase Security Rules** to protect data
3. **Enable App Check** (optional but recommended):
   - Go to **App Check** in Firebase Console
   - Register your app
   - Add reCAPTCHA v3 for web

4. **Set up Firebase Analytics** (optional):
   - Helps track user engagement
   - Monitor app performance

## Troubleshooting

### Authentication Not Working

- Check that your `.env` file has the correct values
- Ensure Email/Password and Google providers are enabled
- Check browser console for error messages

### Firestore Permission Denied

- Verify security rules are published
- Check that user is authenticated
- Ensure `userId` field matches authenticated user

### Images Not Saving

- Check Firestore security rules
- Verify Storage is enabled
- Check browser console for errors
- Ensure user is authenticated

### Firebase Initialization Error

- Double-check all environment variables
- Restart the development server after changing `.env`
- Clear browser cache and cookies

## Additional Resources

- [Firebase Documentation](https://firebase.google.com/docs)
- [Firestore Security Rules](https://firebase.google.com/docs/firestore/security/get-started)
- [Firebase Authentication](https://firebase.google.com/docs/auth)
- [Firebase Storage](https://firebase.google.com/docs/storage)

## Cost Considerations

Firebase has a **generous free tier**:
- **Firestore**: 50K reads, 20K writes, 20K deletes per day
- **Authentication**: Unlimited
- **Storage**: 5GB storage, 1GB/day downloads
- **Hosting**: 10GB storage, 360MB/day bandwidth

For most small to medium projects, you'll stay within the free tier.

---

Need help? Check the Firebase Console for detailed error messages and logs.
