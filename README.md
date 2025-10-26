🎟️ Event RSVP Chatbot and Admin Dashboard

A modern, full-stack event management solution featuring a dark-mode, conversational chatbot for guest RSVPs and a secure admin dashboard for real-time management. Built with HTML, CSS (Bootstrap), JavaScript, and Firebase (Authentication & Firestore).

🚀 Live Demo

You can test the application live here:

Component

URL

Credentials (For Admin)

Public Chatbot

https://lovely-sherbet-711ba3.netlify.app/

None (Anonymous Login)

Admin Dashboard

https://lovely-sherbet-711ba3.netlify.app/admin.html

Requires Firebase Email/Password Auth

✨ Key Features

Public Chatbot (chatbot.html)

Conversational RSVP: Guests can quickly respond to an event invitation via an engaging chat interface.

Persistent Identity: Uses Firebase Anonymous Authentication to track a user's session, even if they leave and return.

RSVP History Drawer: Users can view their past bookings in a smooth, animated side drawer.

Self-Service Management: Users can edit or delete their own existing RSVPs directly from the history drawer.

Modern UI: Features a sleek, dark-mode design with smooth animations and transitions.

Admin Dashboard (admin.html)

Secure Access: Protected by Firebase Email/Password Authentication to ensure only authorized personnel can access the data.

Event Management: Securely add new events to the Firestore database for the chatbot to pull.

Real-Time Live RSVPs: View all incoming RSVPs in a well-organized, smooth-loading list.

Full CRUD on RSVPs: Admins can Edit (update name, status, preferences) and Delete any guest's RSVP entry.

Professional UI: High-contrast, dark-mode interface designed for long-term use and easy data scanning.

🛠️ Setup and Installation

This project is a single-page application structure built to run entirely in the browser using the Firebase CDN.

1. Firebase Project Setup (Crucial)

To make this app work, you must set up your own Firebase project and update the configuration and security rules.

Create a Firebase Project: Go to the Firebase Console.

Enable Services: You must enable the following services:

Authentication: Enable the Email/Password provider (for the Admin login) and the Anonymous provider (for the Chatbot).

Firestore Database: Start in Production mode.

2. Update Security Rules (Mandatory)

The security of your data depends entirely on these rules. Go to Firestore Database > Rules and publish the following:

rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    
    // Admin (Email/Password) can write events; Guests (Authenticated) can only read them.
    match /artifacts/{appId}/public/data/events/{event} {
      allow read: if request.auth != null;
      allow write: if request.auth.token.firebase.sign_in_provider == 'password';
    }

    // Guest can create their own RSVP.
    match /artifacts/{appId}/public/data/rsvps/{rsvp} {
      allow create: if request.auth != null;

      // Guest can read/update/delete THEIR OWN RSVP.
      // Admin can read/update/delete ANY RSVP.
      allow read, update, delete: if request.auth != null && (
                                    resource.data.userId == request.auth.uid ||
                                    request.auth.token.firebase.sign_in_provider == 'password'
                                  );
    }
  }
}


3. Clone and Run Locally

Clone this repository:

git clone [https://github.com/Sajeelsahil1/Event-RSVP-Chatbot.git](https://github.com/Sajeelsahil1/Event-RSVP-Chatbot.git)
cd event-rsvp-chatbot


Open the files (index.html and admin.html) in your browser using a local web server (like the VS Code Live Server extension) to view and test.
