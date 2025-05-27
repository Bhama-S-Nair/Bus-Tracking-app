# College Bus Tracker

A Next.js web application to track and guide college buses in real-time using Google Maps and Firebase.

---

## Features

- Select a bus from 7 available buses (Bus 1 to Bus 7).
- **Guide Mode:** When you are on the bus, share your live location with others.
- **Track Mode:** Track the real-time location of the bus guided by the user in Guide Mode.
- If no one is guiding the bus, a message will notify that the bus cannot be tracked.
- Maps display powered by Google Maps Embed API.
- Real-time location updates handled via Firebase Firestore.

---

## Technologies Used

- [Next.js](https://nextjs.org/) - React framework for server-rendered apps.
- [Firebase Firestore](https://firebase.google.com/docs/firestore) - Real-time database for location sharing.
- Google Maps Embed API - For displaying maps and locations.
- JavaScript & CSS - Frontend logic and styling.

---

## Setup & Installation

### 1. Clone the repository:
```bash
git clone https://github.com/yourusername/college-bus-tracker.git
cd college-bus-tracker
```

### 2. Install dependencies:
```bash
npm install
```

### 3. Create a `.env.local` file in the root directory and add the following:
```ini
NEXT_PUBLIC_FIREBASE_API_KEY=your_firebase_api_key
NEXT_PUBLIC_FIREBASE_AUTH_DOMAIN=your_project.firebaseapp.com
NEXT_PUBLIC_FIREBASE_PROJECT_ID=your_project_id
NEXT_PUBLIC_FIREBASE_STORAGE_BUCKET=your_project.appspot.com
NEXT_PUBLIC_FIREBASE_MESSAGING_SENDER_ID=your_sender_id
NEXT_PUBLIC_FIREBASE_APP_ID=your_app_id
NEXT_PUBLIC_GOOGLE_MAPS_API_KEY=your_google_maps_api_key
```

### 4. Run the development server:
```bash
npm run dev
```

### 5. Open your browser and go to:
[http://localhost:3000](http://localhost:3000)

---

## Usage, Contributing, License & Acknowledgements

### 1. Usage
- On the home page, select a bus number.
- Choose either **Guide** (to share your location) or **Track** (to view the bus location).
- The map will open showing the live location or a message if no guide is available.

### 2. Contributing
Feel free to open issues or submit pull requests for improvements.

### 3. License
This project is licensed under the MIT License.

### 4. Acknowledgements
- Google Maps API  
- Firebase  
- Next.js  
- Open source contributors

