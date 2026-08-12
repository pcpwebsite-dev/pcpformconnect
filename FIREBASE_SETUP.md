# FIREBASE SETUP — PCP FORM SYSTEM

## 1. Authentication
Firebase Console → Authentication → Sign-in method → Email/Password → Enable.

Buat akun admin dari Firebase Console:
Authentication → Users → Add user.

Tidak ada tombol Register di aplikasi.

Semua akun Firebase Authentication yang berhasil login dapat mengakses sisi admin.

## 2. Firestore
Buat Firestore Database.

Struktur data aplikasi:

forms/{formId}
    title
    description
    published
    createdBy
    createdAt
    updatedAt
    questions: [...]
    settings: {...}

forms/{formId}/responses/{responseId}
    answers: {...}
    submittedAt
    userAgent
    formVersion

## 3. Firestore Rules
Copy isi file firestore.rules ke:
Firestore Database → Rules.

Rule public:
- published form boleh dibaca tanpa login
- response boleh dibuat tanpa login
- response tidak boleh dibaca/edit/delete oleh public
- admin harus login Firebase Authentication untuk membaca/manajemen form dan response

## 4. Firebase config
Config yang diberikan sudah dimasukkan ke js/firebase.js.

Firebase Web API key bukan secret credential. Security tetap bergantung pada Authentication + Firestore Rules.

## 5. Jalankan
Karena memakai ES modules, jalankan melalui web server, bukan file://.

Contoh:
python3 -m http.server 5500

Lalu buka:
http://localhost:5500/admin/

Public form:
http://localhost:5500/form/?id=FORM_ID

## 6. Catatan
Versi ini sengaja tidak memakai Firebase Storage karena kebutuhan utama adalah Firestore.
Untuk attachment/file upload seperti Google Forms, tambahkan Firebase Storage + Cloud Functions/Rules sebelum fitur tersebut dibuka ke public.
