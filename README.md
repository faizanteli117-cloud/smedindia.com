# SMED Precision Hardware website

Files: `index.html` (whole site) + `images/` (starting pictures cropped from your Stitch design).
Upload both to your GitHub Pages repo (keep the `images` folder next to `index.html`).

## 1. Meta Pixel (whenever you are ready)
Open `index.html`, scroll to the bottom `window.SMED = {...}` block and set:
    metaPixelId: "YOUR_PIXEL_ID"
Page views, enquiry (Contact) and dealer/catalogue (Lead) events then fire automatically.

## 2. Upload product images yourself
1. console.firebase.google.com > create project > Build > Firestore Database > Create (production mode).
2. Build > Authentication > Get started > enable Email/Password > Users > Add user (your email + password).
3. Project settings > Your apps > Web (</>) > copy apiKey, authDomain, projectId, appId into the `firebase` block in `index.html`.
4. Authentication > Settings > Authorized domains > add your site domain (smedindia.in and yourname.github.io).
5. Firestore > Rules > paste this (replace YOUR_EMAIL) and Publish:

    rules_version = '2';
    service cloud.firestore {
      match /databases/{db}/documents {
        match /images/{id} {
          allow read: if true;
          allow write: if request.auth != null && request.auth.token.email == 'YOUR_EMAIL';
        }
        match /leads/{id} {
          allow create: if request.resource.data.name is string && request.resource.data.name.size() < 200;
          allow read, update, delete: if request.auth != null && request.auth.token.email == 'YOUR_EMAIL';
        }
      }
    }

6. Open your site, tap the round user icon (top right), sign in. Every picture now shows a gold "Change image" button.
   Pick a photo from your phone: it is resized automatically and saved. The small reset button brings back the original.

Enquiry and dealer forms are saved in Firestore > `leads` (without Firebase they open an email to smedindia.in@gmail.com).

## 3. Catalogue button
Put a PDF link in `catalogueUrl` (Google Drive share link or a PDF in the repo). Empty = it opens a "Request the catalogue" form.
