# firebase_firestore


# Firebase Overview

**Firebase** is a platform developed by Google that provides a set of cloud-based tools to help developers build, improve, and grow apps. It offers backend services like authentication, databases, analytics, and cloud storage without managing servers.

---

## Core Firebase Features

| Feature                  | Description                                                            |
| ------------------------ | ---------------------------------------------------------------------- |
| Authentication           | Sign up/login with email, password, or third-party providers           |
| Cloud Firestore          | NoSQL cloud database that stores and syncs data in real-time           |
| Realtime Database        | Another NoSQL database with real-time syncing, suitable for small data |
| Firebase Hosting         | Fast and secure static hosting for web apps                            |
| Cloud Functions          | Serverless backend to run backend code in response to events           |
| Firebase Storage         | Store large files like images, videos, audio, PDFs                     |
| Crashlytics              | Real-time crash reporting and diagnostics                              |
| Firebase Analytics       | Free app analytics for user behavior tracking                          |
| Remote Config            | Change app behavior and appearance without publishing an update        |
| Firebase Cloud Messaging | Push notification service across platforms                             |

---

# Firebase Authentication

## Purpose

Authenticate users using multiple methods such as email/password, phone, Google, and more.

## Supported Authentication Methods

* Email/Password
* Google Sign-In
* Facebook, Apple, GitHub, Twitter
* Phone Number (OTP)
* Anonymous sign-in

## Example Code (Flutter)

### Adding dependencies

```yaml
dependencies:
  firebase_core: ^2.30.0
  firebase_auth: ^4.19.0
  google_sign_in: ^6.2.1
```

### Basic Usage

```dart
import 'package:firebase_auth/firebase_auth.dart';

final FirebaseAuth _auth = FirebaseAuth.instance;

// Sign Up
final userCredential = await _auth.createUserWithEmailAndPassword(
  email: 'test@example.com',
  password: '123456',
);

// Sign In
final loginCredential = await _auth.signInWithEmailAndPassword(
  email: 'test@example.com',
  password: '123456',
);

// Sign Out
await _auth.signOut();
```

### Google Sign-In Example

```dart
import 'package:google_sign_in/google_sign_in.dart';

final GoogleSignIn googleSignIn = GoogleSignIn();
final googleUser = await googleSignIn.signIn();
final googleAuth = await googleUser?.authentication;

final credential = GoogleAuthProvider.credential(
  accessToken: googleAuth?.accessToken,
  idToken: googleAuth?.idToken,
);

await FirebaseAuth.instance.signInWithCredential(credential);
```

---

# Cloud Firestore

## Overview

Cloud Firestore is a scalable, flexible NoSQL database for mobile and web apps.

## Structure

Firestore data is stored in:

* **Collections** → like tables
* **Documents** → like rows, but flexible
* **Fields** → key-value pairs inside documents
* **Subcollections** → nested collections inside documents

## Firestore vs Realtime Database

| Feature         | Firestore                 | Realtime Database       |
| --------------- | ------------------------- | ----------------------- |
| Data model      | Collections and Documents | JSON tree               |
| Querying        | Rich queries supported    | Limited queries         |
| Scalability     | Highly scalable           | Scales well but limited |
| Offline support | Built-in                  | Built-in                |

---

## Firestore Usage in Flutter

### Dependencies

```yaml
dependencies:
  cloud_firestore: ^4.17.0
```

### Basic Operations

#### Initialize Collection Reference

```dart
final CollectionReference tasks = FirebaseFirestore.instance.collection('tasks');
```

#### Create (Add Data)

```dart
await tasks.add({'title': 'New Task', 'completed': false});
```

#### Read (Listen for Data)

```dart
StreamBuilder<QuerySnapshot>(
  stream: tasks.snapshots(),
  builder: (context, snapshot) {
    if (!snapshot.hasData) return CircularProgressIndicator();
    return ListView(
      children: snapshot.data!.docs.map((doc) {
        return Text(doc['title']);
      }).toList(),
    );
  },
)
```

#### Update Data

```dart
await tasks.doc('document_id').update({'title': 'Updated Task'});
```

#### Delete Data

```dart
await tasks.doc('document_id').delete();
```

---

## Firestore Security Rules

Use Firebase Console to configure security rules.

Example:

```plaintext
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /tasks/{taskId} {
      allow read, write: if request.auth != null;
    }
  }
}
```

---

## Firestore Indexing

* Firestore automatically indexes simple queries.
* Compound queries (multiple filters) require creating composite indexes in the console.

---

## Use Cases

* Storing user data (profiles, posts)
* Real-time chat apps
* Task or note apps
* E-commerce product databases

---

If you want, I can also create **summarized cheat sheets** or a **GitHub markdown file** with examples. Let me know.
