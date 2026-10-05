# Essensora

Ruang tenang untuk napas sejenak saat pikiran terasa penuh. PWA (HTML + Firebase) dengan halaman login.

## Struktur file

```
icons/               ikon PWA dan logo
.nojekyll            supaya GitHub Pages menyajikan file apa adanya
README.md
index.html           halaman login + aplikasi
manifest.json        pengaturan PWA
service-worker.js    cache offline
```

## Langkah setup

1. **Isi config Firebase** di `index.html` (blok `firebaseConfig`).
   Firebase Console > Project settings > General > Your apps > tambahkan Web app > salin config.
2. **Aktifkan login email**: Firebase Console > Authentication > Sign-in method > Email/Password > Enable.
3. **Izinkan domain GitHub**: Authentication > Settings > Authorized domains > tambahkan `USERNAME.github.io`.
4. **Ganti Firestore rules** (Firestore Database > Rules), lalu Publish:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{userId} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
  }
}
```

Rules lama (`allow read, write: if false`) menolak semua akses, jadi data pengguna tidak akan tersimpan sebelum diganti.

5. **Upload ke GitHub**: upload semua file dan folder `icons` ke repository, lalu Settings > Pages > Deploy from a branch > `main` / `(root)`.

## Catatan

- Jika kamu memperbarui file, naikkan `CACHE` di `service-worker.js` (misalnya `essensora-v2`) agar pengguna menerima versi baru.
- `apiKey` Firebase untuk web memang tampil di kode; keamanan data dijaga oleh Firestore rules.
