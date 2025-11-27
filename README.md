# ai-support-platform🤖 AI Support Platform (TypeScript + React + Firebase + Gemini RAG)

🌟 Project Overview

Yeh ek modern, real-time AI Customer Support Platform hai jise TypeScript aur React mein banaya gaya hai. Yeh application Firebase Firestore ko data persistence (chat history aur knowledge base) ke liye istemal karta hai, aur Google Gemini API ko Retrieval-Augmented Generation (RAG) ke liye use karta hai.

Is app mein do mukhy bhag hain:

Chat Interface: Jahaan user AI se sawal poochhte hain.

Admin Panel: Jahaan documents upload kiye jaate hain jo AI ke "Knowledge Base" ka hissa bante hain. AI sirf in documents ke aadhaar par hi jawab deta hai.

✨ Features

Real-time Chat: Firestore ke through instant message update.

Knowledge Base Upload: Admin panel se documents upload karein.

RAG Implementation: Gemini model in uploaded documents se information lekar user ke sawalon ka precise jawab deta hai.

TypeScript: Type safety aur robust code structure.

Responsive UI: Tailwind CSS ka upyog karke behtareen design.

⚙️ Architecture Diagram

Yeh application teen (3) core services ka upyog karta hai.

Frontend (React/TSX): User Interface aur user interaction.

Firestore (Firebase): Chat messages aur Knowledge Base documents ko store karta hai.

Private Data: Chat History (/users/{userId}/chat_history)

Public Data: Knowledge Base (/public/data/knowledge_base)

Gemini API: User ke prompt ko RAG context ke saath lekar, intelligence provide karta hai.

🛠️ Local Setup and Configuration

Is project ko local machine par run karne ke liye neeche diye gaye steps follow karein.

1. Prerequisites

Node.js (v16+) aur npm/yarn

Ek naya/existing React Project (Preferably Vite + TypeScript template)

Google Firebase Project: Jismein Firestore Database aur Authentication enable ho.

Google AI Studio / Gemini API Key: AI functionality ke liye.

2. Installation

Apne terminal mein project directory mein jaakar zaroori packages install karein:

npm install firebase tailwindcss postcss autoprefixer
# Tailwind CSS config files banane ke liye
npx tailwindcss init -p


tailwind.config.js mein content path set karein:

// tailwind.config.js
module.exports = {
  content: [
    "./index.html",
    "./src/**/*.{js,ts,jsx,tsx}",
  ],
  // ... rest of the config
}


src/index.css mein Tailwind directives daalen:

@tailwind base;
@tailwind components;
@tailwind utilities;


3. Key Configuration (Critical Step)

src/App.tsx file mein, top par diye gaye environment variables ko update karein:

A. Firebase Configuration

Apne Firebase project ki config se YOUR_... values ko replace karein:

const firebaseConfig: FirebaseConfig = {
    apiKey: "YOUR_FIREBASE_API_KEY", // <-- Replace this
    authDomain: "YOUR_PROJECT_ID.firebaseapp.com",
    projectId: "YOUR_PROJECT_ID",
    // ... baaki details
};


B. Gemini API Key

Apni Gemini API Key yahan daalen:

const GEMINI_API_KEY: string = "YOUR_GEMINI_API_KEY"; // <-- Replace this


⚠️ Note on Missing Keys: Agar aap keys khaali chhod dete hain, toh app chalega, lekin database se data sync nahi hoga, aur AI mock/placeholder jawab dega.

4. Firestore Security Rules (Mandatory)

Real-time data synchronization ke liye, aapko apne Firebase Console mein jaakar Firestore Rules ko update karna hoga. Agar rules set nahi kiye gaye toh app data read/write nahi kar payega.

Rules Tab mein yeh rules daalen:

rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    
    // Knowledge Base (Public Data: Sabhi authenticated users read/write kar sakte hain)
    match /artifacts/{appId}/public/data/knowledge_base/{docId} {
      allow read, write: if request.auth != null;
    }
    
    // Chat History (Private Data: Har user sirf apni history read/write kar sakta hai)
    match /artifacts/{appId}/users/{userId}/chat_history/{chatId} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
  }
}


5. Running the Application

Terminal mein project root directory se yeh command chalaayein:

npm run dev
