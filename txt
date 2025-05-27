# College Bus Tracker App - Next.js (JavaScript) + Firebase (Realtime DB) + Google Maps Embed API
# WITHOUT Tailwind CSS

--------------------------------------
1. INITIAL SETUP
--------------------------------------

# Create a new Next.js app with App Router and JavaScript
npx create-next-app@latest college-bus-tracker
cd college-bus-tracker

# Install Firebase SDK
npm install firebase

# Folder structure:
.
├── app/
│   ├── page.js                       # Homepage: Select Bus
│   └── bus/
│       └── [id]/
│           ├── page.js              # Choose: Guide or Track
│           ├── guide/
│           │   └── page.js          # Guide Page
│           └── track/
│               └── page.js          # Track Page
├── lib/
│   └── firebase.js                  # Firebase Configuration
└── public/
    └── favicon.ico (default)

--------------------------------------
2. FIREBASE CONFIGURATION
--------------------------------------

# File: lib/firebase.js

import { initializeApp } from 'firebase/app';
import { getDatabase } from 'firebase/database';

const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_PROJECT_ID.firebaseapp.com",
  databaseURL: "https://YOUR_PROJECT_ID.firebaseio.com",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_PROJECT_ID.appspot.com",
  messagingSenderId: "YOUR_SENDER_ID",
  appId: "YOUR_APP_ID"
};

const app = initializeApp(firebaseConfig);
export const database = getDatabase(app);

# Replace all placeholder values with your actual Firebase project credentials.

--------------------------------------
3. PAGE: app/page.js - Bus Selection
--------------------------------------

'use client';

import Link from 'next/link';

export default function HomePage() {
  const buses = Array(7).fill(0).map((_, i) => i + 1);
  return (
    <main style={{ display: 'flex', flexDirection: 'column', alignItems: 'center', minHeight: '100vh', justifyContent: 'center' }}>
      <h1 style={{ fontSize: '2rem', marginBottom: '24px' }}>Select Your Bus</h1>
      <div style={{ display: 'grid', gridTemplateColumns: 'repeat(2, 120px)', gap: '16px' }}>
        {buses.map((num) => (
          <Link key={num} href={`/bus/${num}`}>
            <button style={{ padding: '12px', backgroundColor: '#2563eb', color: 'white', borderRadius: '6px', border: 'none' }}>Bus {num}</button>
          </Link>
        ))}
      </div>
    </main>
  );
}

--------------------------------------
4. PAGE: app/bus/[id]/page.js - Guide or Track
--------------------------------------

'use client';

import { useParams } from 'next/navigation';
import Link from 'next/link';

export default function BusOptions() {
  const { id: busId } = useParams();
  return (
    <main style={{ display: 'flex', flexDirection: 'column', alignItems: 'center', minHeight: '100vh', justifyContent: 'center' }}>
      <h1 style={{ fontSize: '2rem', marginBottom: '24px' }}>Bus {busId}</h1>
      <div style={{ display: 'flex', gap: '20px' }}>
        <Link href={`/bus/${busId}/guide`}>
          <button style={{ padding: '12px 24px', backgroundColor: '#22c55e', color: 'white', borderRadius: '6px', border: 'none' }}>Guide</button>
        </Link>
        <Link href={`/bus/${busId}/track`}>
          <button style={{ padding: '12px 24px', backgroundColor: '#7c3aed', color: 'white', borderRadius: '6px', border: 'none' }}>Track</button>
        </Link>
      </div>
    </main>
  );
}

--------------------------------------
5. PAGE: app/bus/[id]/guide/page.js - Guide Page (Send Location)
--------------------------------------

'use client';

import { useEffect } from 'react';
import { useParams } from 'next/navigation';
import { ref, set } from 'firebase/database';
import { database } from '@/lib/firebase';

export default function GuidePage() {
  const { id: busId } = useParams();

  useEffect(() => {
    if (!busId) return;

    if ('geolocation' in navigator) {
      const watchId = navigator.geolocation.watchPosition(
        (position) => {
          const { latitude, longitude } = position.coords;
          set(ref(database, `bus_locations/bus_${busId}`), {
            lat: latitude,
            lng: longitude,
            timestamp: Date.now(),
          });
        },
        (error) => alert('Location Error: ' + error.message),
        { enableHighAccuracy: true }
      );

      return () => navigator.geolocation.clearWatch(watchId);
    } else {
      alert('Geolocation not supported');
    }
  }, [busId]);

  return (
    <main style={{ display: 'flex', flexDirection: 'column', alignItems: 'center', minHeight: '100vh', justifyContent: 'center' }}>
      <h1 style={{ fontSize: '2rem', marginBottom: '16px' }}>Guiding Bus {busId}</h1>
      <p>Sharing your location with others...</p>
    </main>
  );
}

--------------------------------------
6. PAGE: app/bus/[id]/track/page.js - Track Page (View on Map)
--------------------------------------

'use client';

import { useState, useEffect } from 'react';
import { useParams } from 'next/navigation';
import { ref, onValue } from 'firebase/database';
import { database } from '@/lib/firebase';

export default function TrackPage() {
  const { id: busId } = useParams();
  const [location, setLocation] = useState(null);

  useEffect(() => {
    const locationRef = ref(database, `bus_locations/bus_${busId}`);
    const unsub = onValue(locationRef, (snapshot) => {
      const data = snapshot.val();
      if (data?.lat && data?.lng) {
        setLocation({ lat: data.lat, lng: data.lng });
      } else {
        setLocation(null);
      }
    });
    return () => unsub();
  }, [busId]);

  return (
    <main style={{ display: 'flex', flexDirection: 'column', alignItems: 'center', minHeight: '100vh', justifyContent: 'center' }}>
      <h1 style={{ fontSize: '2rem', marginBottom: '16px' }}>Tracking Bus {busId}</h1>
      {location ? (
        <iframe
          style={{ border: 0, width: '100%', maxWidth: '600px', height: '450px' }}
          src={`https://www.google.com/maps/embed/v1/view?key=YOUR_GOOGLE_MAPS_API_KEY&center=${location.lat},${location.lng}&zoom=15`}
          allowFullScreen
          loading="lazy"
        />
      ) : (
        <p>No one is guiding this bus right now. Can't track it.</p>
      )}
    </main>
  );
}

--------------------------------------
7. GOOGLE MAPS
--------------------------------------

# Use the Maps Embed API from:
https://console.cloud.google.com/apis/library/maps-embed-backend.googleapis.com

# Enable billing and add your key to the iframe src:
?key=YOUR_GOOGLE_MAPS_API_KEY

--------------------------------------
8. DONE ✅
--------------------------------------

You now have:
- A home page to choose buses.
- Guide mode to share real-time location.
- Track mode to view the shared location.

Deploy on Vercel or any Next.js host.

