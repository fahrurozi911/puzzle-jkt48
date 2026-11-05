# Puzzle Foto: panduan pemasangan

## 1. Buat project Firebase (gratis)
1. Buka console.firebase.google.com > Add project.
2. Project settings > Your apps > ikon Web (`</>`) > daftarkan app > salin `firebaseConfig`.
3. Tempel nilainya ke `firebase-config.js`.

## 2. Aktifkan Firestore
Build > Firestore Database > Create database (mode production). Lalu buka tab **Rules** dan tempel:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /photos/{id} {
      allow read: if true;
      allow write: if request.auth != null;
    }
    match /users/{id}  { allow read, create: if true; }
    match /scores/{id} { allow read, write: if true; }
  }
}
```

## 3. Buat akun admin
Build > Authentication > Get started > aktifkan **Email/Password** saja (jangan aktifkan Anonymous).
Tab Users > Add user > isi email dan password admin milikmu.

## 4. Hosting di GitHub
1. Buat repository, upload keempat file ini.
2. Settings > Pages > Source: branch `main`, folder `/ (root)`.
3. Game: `https://USERNAME.github.io/NAMA-REPO/`
4. Admin: `https://USERNAME.github.io/NAMA-REPO/admin.html` (tidak ada link ke sini dari game)

Setelah Pages aktif, tambahkan domain `USERNAME.github.io` di Authentication > Settings > Authorized domains.

## Rumus skor
- Dasar = jumlah kotak x 100 (4x4 = 1.600, 16x16 = 25.600)
- Waktu acuan = jumlah kotak x 6 detik
- Faktor = 1,5 - (waktu / waktu acuan), minimal 0,1
- Skor = dasar x faktor. Leaderboard menyimpan skor terbaik tiap pemain.

## Batasan
- PIN disimpan sebagai hash, tetapi PIN 4 angka tetap bisa ditebak oleh orang yang berniat. Cocok untuk game santai, bukan data penting.
- Skor ditulis dari browser, jadi pemain yang paham teknis bisa memalsukannya.
