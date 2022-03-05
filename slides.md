---
theme: unicorn
download: true

logoHeader: "https://upload.wikimedia.org/wikipedia/commons/thumb/4/49/Flag_of_Ukraine.svg/1599px-Flag_of_Ukraine.svg.png?20100406171642"
website: "with-humanity.vercel.app"
handle: "andrei_zgirvaci"

layout: intro
introImage: "https://i.imgur.com/2LkQdTM.jpg"
---

<style>
  img {
      @apply rounded-md shadow-lg border-opacity-25
  }
</style>

# How I built with-humanity

Andrei Zgirvaci

---

# Tech Stack

- Front-End:
  - Next.js (TypeScript)
  - Tailwind CSS, Headless UI, Heroicons
  - google-map-react
  - ga-4-react

<br />

<v-click>

- Back-End:
  - Firebase (Firestore)

</v-click>

---

# Create Next.js project

<br />

**1. Create a new project in Next.js:**
```bash
npx create-next-app@latest --ts
```

<br />

**2. Add google-map-react & firebase:**
```bash
npm add firebase google-map-react
```

---

**3. Create Firebase Project:**

<img src="https://i.imgur.com/983QhNI.jpg" class="h-50 mx-auto" />

---

**4. Enable Google Maps APIs:**

<img src="https://i.imgur.com/PdJ2mSh.jpg" class="h-80 mx-auto" />

<br />

\* <u>Make sure that billing is enabled for your Cloud project.</u>

\* _Google Cloud offers a $0.00 charge trial. The trial expires at either end of 90 days or after the account has accrued $300 worth of charges, whichever comes first._

---

**5. Create Firebase Web App:**

<img src="https://i.imgur.com/ox4b03O.jpg" class="h-110 mx-auto" />

---

**6. Copy App Credentials & Initialize Firebase:**

```ts
// pages/_app.tsx
import { initializeApp } from 'firebase/app';

const firebaseConfig = {
  apiKey: process.env.NEXT_PUBLIC_FIREBASE_API_KEY,
  authDomain: 'nowar-livemap.firebaseapp.com',
  projectId: 'nowar-livemap',
  storageBucket: 'nowar-livemap.appspot.com',
  messagingSenderId: '80290698930',
  appId: '1:80290698930:web:60de91103d9e5e44061bab',
  measurementId: 'G-E1R6X235XR',
};

initializeApp(firebaseConfig);

/* ... */
```

<br />

**7. Copy `apiKey` and add it as an environment variable:**

```js
// .env.local
NEXT_PUBLIC_FIREBASE_API_KEY=***************************************
```

---

**8. Add Google Map component:**

```tsx {11-25|16|3-9,18-19}
// pages/index.tsx
const Home = () => {
  const defaultProps = {
    center: {
      lat: 50.46372175476692, // kiev
      lng: 30.529234424870925, // kiev
    },
    zoom: 6,
  };

  return (
    <main className="flex flex-col h-full">
      <section className="flex flex-1">
        <GoogleMapReact
          bootstrapURLKeys={{
            key: process.env.NEXT_PUBLIC_FIREBASE_API_KEY,
          }}
          defaultCenter={defaultProps.center}
          defaultZoom={defaultProps.zoom}
        />
      </section>
    </main>
  );
}
```

---

**9. Create a new collection in Firestore:**

<img src="https://i.imgur.com/3TjC73o.jpg" class="h-60 mx-auto" />

---

**10. Create a test document in `markers` collection:**

<img src="https://i.imgur.com/VDlhS0d.jpg" class="h-100 mx-auto" />

---

**11. Get Markers from Firestore:**

```tsx {2,4|7|9|11-21|23}
// lib/markers.ts
import { collection, getDocs, getFirestore } from 'firebase/firestore';

const db = getFirestore();

export default async function getMarkers() {
  const querySnapshot = await getDocs(collection(db, 'markers'));

  const markers = [];

  querySnapshot.forEach((doc) => {
    const data = doc.data();

    markers.push({
      id: doc.id,
      lat: data.location._lat,
      lng: data.location._long,
      type: data.type,
      description: data.description,
    });
  });

  return markers;
}
```

---

**12. Render Markers in Maps:**

```tsx {2,5-11|3,15-17}
// pages/index.tsx
import getMarkers from 'lib/markers';
import Marker from 'components/Marker';

export async function getServerSideProps() {
  const markers = await getMarkers();

  return {
    props: { markers },
  };
}

...
<GoogleMapReact ...>
  {markers.map((marker) => (
    <Marker key={marker.id} {...marker} />
  ))}
<GoogleMapReact/>
...
```

---

**13. Create `Marker` Component:**

```tsx {7|9,15|10-14|2,12}
// components/Marker.tsx
import PopoverComponent from 'components/Popover';

export default function Marker({ $hover, description }) {
  return (
    <Popover>
      <Icon className={`h-7 w-7 text-blue-500`} />

      {$hover && (
        <Popover.Panel static>
          <div className="w-72 h-72 bg-white p-2">
            <PopoverComponent description={description} />
          </div>
        </Popover.Panel>
      )}
    </Popover>
  );
}
```

---

**14. Create `Popover` Component:**

```tsx
// components/Popover.tsx
export default function Popover({ type, description }) {
  return (
    <>
      <p className="mb-2 text-lg font-bold">{type.toUpperCase()}}</p>

      <hr className="mb-4" />

      <div className="text-sm text-slate-900">
        <p>{description}</p>
      </div>
    </>
  );
}
```

---

**15. Deploy your application to Vercel (1)**

<img src="https://i.imgur.com/O8cbdvF.jpg" class="h-80 mx-auto" />

---

**16. Deploy your application to Vercel (2):**

<img src="https://i.imgur.com/IgptnWb.jpg" class="h-70 mx-auto" />

---

**17. Deploy your application to Vercel (3):**

<img src="https://i.imgur.com/j6bbgQU.jpg" class="h-50 mx-auto" />

---

# Demo

<img src="https://i.imgur.com/NEayOsm.jpg" class="h-110 mx-auto" />

---
layout: center
---

# That's it! Thank you!

---

<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />

# Q&A

<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />

**website:** https://with-humanity.vercel.app
<br/>
**github:** https://github.com/andrei-zgirvaci/with-humanity