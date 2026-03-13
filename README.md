<img width="1371" height="208" alt="Screenshot 2026-03-13 071336" src="https://github.com/user-attachments/assets/63a5f58d-8a81-4598-ae15-e50d16e89e42" /># firebase-auth-postman
Tutorial Firebase Authentication menggunakan Postman

## 1. Membuat Project Firebase

1. Buka https://firebase.google.com
2. Klik Go to Console.
3. Klik Create a new project.
4. Isi nama project → klik Continue → klik Create Project.
5. Tunggu sampai proses selesai → klik Continue.

## Contoh
<img width="500" alt="Screenshot 2026-03-13 055713" src="https://github.com/user-attachments/assets/8d19d4b0-881c-4bc1-9452-cbd44e1213a1" />
<img width="500" alt="Screenshot 2026-03-13 055932" src="https://github.com/user-attachments/assets/c663f399-5c48-4807-b9c8-644833426b72" />
<img width="500" alt="Screenshot 2026-03-13 055947" src="https://github.com/user-attachments/assets/a9fa784f-999f-4033-9539-2f45e91edc6c" />
<img width="500" alt="Screenshot 2026-03-13 060001" src="https://github.com/user-attachments/assets/7e00f366-6115-4e47-89ad-7b9293dd794b" />
<img width="500" alt="Screenshot 2026-03-13 061104" src="https://github.com/user-attachments/assets/9649003f-d6b9-4660-8f3a-a1d405f32e16" />

## Langkah 2 — Aktifkan Autentikasi Email/Password
 
1. Di menu kiri Firebase Console, pilih **Authentication**
2. Klik tab **Metode Login** (Sign-in method)
3. Pilih **Email/Password** dari daftar
4. Aktifkan toggle **Aktifkan**, lalu klik **Simpan**

## Contoh

<img width="500" alt="Screenshot 2026-03-13 061551" src="https://github.com/user-attachments/assets/d6fe6f94-47fd-45eb-95f9-a181943c45ef" />
<img width="500" alt="Screenshot 2026-03-13 061610" src="https://github.com/user-attachments/assets/b72758dc-02ed-42d8-9c52-11c58187b60f" />
<img width="500" alt="Screenshot 2026-03-13 061627" src="https://github.com/user-attachments/assets/f37022c7-df90-475a-b442-ca605b88b116" />
<img width="400" alt="Screenshot 2026-03-13 061644" src="https://github.com/user-attachments/assets/852c3987-886a-40d7-bf9b-011a72f1772d" />
<img width="500" alt="Screenshot 2026-03-13 061912" src="https://github.com/user-attachments/assets/6e383026-4cbd-4ec9-8ef0-ca4ffae4bab0" />

## Langkah 3 — Daftarkan Aplikasi Web
 
1. Di halaman utama proyek, klik ikon **`</>`** (Web)
2. Beri nama aplikasi kamu, lalu klik **Daftarkan Aplikasi**
3. Salin konfigurasi `firebaseConfig` yang muncul — kamu akan membutuhkannya di langkah berikutnya

## Contoh
<img width="300" alt="Screenshot 2026-03-13 062356" src="https://github.com/user-attachments/assets/7d43dd1b-519f-4830-891e-ef76a9d77bf3" />
<img width="300" alt="Screenshot 2026-03-13 062410" src="https://github.com/user-attachments/assets/5e7579a9-0c64-4ceb-9bb3-bbc8c8a9e453" />
<img width="300" alt="Screenshot 2026-03-13 062656" src="https://github.com/user-attachments/assets/912d5d48-ba21-45be-94f7-0f19595f8fc9" />
<img width="400" alt="Screenshot 2026-03-13 062742" src="https://github.com/user-attachments/assets/c62d5bdc-0e1b-4928-b1a1-1c2597075d4d" />

## Setup Environment Postman
 
Buat environment baru agar tidak perlu copy-paste API key berulang kali:
 
1. Postman → **Environments** → **New Environment** → beri nama `Firebase Auth Dev`
2. Tambahkan variabel berikut:
 
| Variabel | Initial Value | Keterangan |
|---|---|---|
| `FIREBASE_API_KEY` | `AIzaSyB_xxx...` | Web API Key dari Firebase Console |
| `FIREBASE_ID_TOKEN` | *(kosong)* | Diisi otomatis setelah login via Test Script |
| `BACKEND_BASE_URL` | `http://localhost:8080/v1` | Base URL backend kamu |
| `BACKEND_TOKEN` | *(kosong)* | Token JWT dari backend (diisi setelah verify) |
| `USER_EMAIL` | `test@example.com` | Email untuk testing |
| `USER_PASSWORD` | `Test@12345` | Password untuk testing |

## Contoh
<img width="800" alt="image" src="https://github.com/user-attachments/assets/0174d709-f07f-47e4-8889-8b8f381d50ce" />


## Urutan Pengujian
 
```
STEP 1 → Register (Firebase REST API)
STEP 2 → Kirim Verifikasi Email
STEP 3 → Cek Status Verifikasi (via Firebase & Backend)
STEP 4 → Login (Firebase REST API)
STEP 5 → POST Token ke Backend
STEP 6 → Akses Protected Endpoint
```
## Step 1 — Register Akun Baru
 
**Firebase: `signUp` endpoint**
 
### Endpoint
 
```
POST https://identitytoolkit.googleapis.com/v1/accounts:signUp?key={{FIREBASE_API_KEY}}
```
 
### Headers
 
| Key | Value |
|---|---|
| `Content-Type` | `application/json` |
 
### Request Body
 
```json
{
  "email": "{{USER_EMAIL}}",
  "password": "{{USER_PASSWORD}}",
  "returnSecureToken": true
}
```
 
| Field | Tipe | Keterangan |
|---|---|---|
| `email` | string | Harus format email valid |
| `password` | string | Minimal 6 karakter |
| `returnSecureToken` | boolean | **WAJIB** `true` agar Firebase mengembalikan `idToken` & `refreshToken` |
 
### Response Sukses — 200 OK
 
```json
{
  "kind": "identitytoolkit#SignupNewUserResponse",
  "localId": "aBcDeFgHiJkLmN",
  "email": "test@example.com",
  "idToken": "eyJhbGciOiJSUzI1...",
  "registered": false,
  "refreshToken": "AMf-vBxK...",
  "expiresIn": "3600"
}
```
 
### Response Error — Contoh
 
```json
{
  "error": {
    "code": 400,
    "message": "EMAIL_EXISTS",
    "status": "INVALID_ARGUMENT"
  }
}
```


| Kode Error | Artinya | Solusi |
|---|---|---|
| `EMAIL_EXISTS` | Email sudah terdaftar | Gunakan email lain |
| `INVALID_EMAIL` | Format email salah | Periksa format email |
| `WEAK_PASSWORD` | Password kurang dari 6 karakter | Gunakan minimal 6 karakter |
| `OPERATION_NOT_ALLOWED` | Email/Password auth belum diaktifkan | Aktifkan di Firebase Console → Authentication → Sign-in method |
| `TOO_MANY_ATTEMPTS_TRY_LATER` | Terlalu banyak percobaan | Tunggu beberapa menit |
 
### Postman Test Script — Auto-save Token
 
Tempel di tab **Tests**:
 
```js
const json = pm.response.json();
 
if (pm.response.code === 200) {
  pm.environment.set("FIREBASE_ID_TOKEN", json.idToken);
  pm.environment.set("FIREBASE_LOCAL_ID", json.localId);
  pm.environment.set("FIREBASE_REFRESH_TOKEN", json.refreshToken);
  console.log("Register sukses. UID:", json.localId);
  console.log("PERHATIAN: Email belum diverifikasi. Lanjut ke Step 2.");
} else {
  console.log("Register gagal:", json.error.message);
}
```

## Contoh

<img width="500" alt="Screenshot 2026-03-13 070021" src="https://github.com/user-attachments/assets/7296108a-b60d-45c7-9e24-a92cae0fda95" />
<img width="500" alt="Screenshot 2026-03-13 070036" src="https://github.com/user-attachments/assets/c75ef167-53f7-4331-bbdd-1010d844a2a4" />
<img width="500" alt="Screenshot 2026-03-13 070058" src="https://github.com/user-attachments/assets/23ed8698-caf6-45fb-b100-da41ea89d1af" />
<img width="800" alt="image" src="https://github.com/user-attachments/assets/2e5e1eed-05dc-4614-9b47-bda9bc704f06" />


## Step 2 — Kirim Email Verifikasi
 
**Firebase: `sendOobCode` — meminta Firebase mengirim link ke inbox**
 
### Endpoint
 
```
POST https://identitytoolkit.googleapis.com/v1/accounts:sendOobCode?key={{FIREBASE_API_KEY}}
```
 
### Headers
 
| Key | Value |
|---|---|
| `Content-Type` | `application/json` |
 
### Request Body
 
```json
{
  "requestType": "VERIFY_EMAIL",
  "idToken": "{{FIREBASE_ID_TOKEN}}"
}
```
 
| Field | Tipe | Keterangan |
|---|---|---|
| `requestType` | string | **WAJIB** diisi `"VERIFY_EMAIL"` |
| `idToken` | string | Firebase ID Token dari Step 1 |
 
### Response Sukses — 200 OK
 
```json
{
  "kind": "identitytoolkit#GetOobConfirmationCodeResponse",
  "email": "test@example.com"
}
```
 
**Yang terjadi di background:**
1. Firebase membuat "action link" unik
2. Firebase mengirim email ke user berisi link verifikasi
3. User membuka email dan mengklik link tersebut
4. Status `emailVerified` di Firebase berubah menjadi `true`
 
### Response Error
 
| Kode Error | Artinya | Solusi |
|---|---|---|
| `INVALID_ID_TOKEN` | Token kadaluarsa atau tidak valid | Login ulang di Step 4, lalu kirim ulang |
| `USER_NOT_FOUND` | User tidak ditemukan | Pastikan register berhasil di Step 1 |
| `TOO_MANY_ATTEMPTS_TRY_LATER` | Firebase membatasi pengiriman email | Tunggu beberapa menit |
 
### Postman Test Script
 
```js
if (pm.response.code === 200) {
  const json = pm.response.json();
  console.log("Email verifikasi dikirim ke:", json.email);
  console.log("Buka inbox email dan klik link verifikasi.");
  console.log("Setelah klik, lanjut ke Step 3 untuk cek status.");
} else {
  console.log("Gagal kirim email:", pm.response.json().error.message);
}
```
## Contoh

<img width="500" alt="Screenshot 2026-03-13 071220" src="https://github.com/user-attachments/assets/dd48d03d-43fe-46d4-bc01-133d9a183e30" />
<img width="500" alt="Screenshot 2026-03-13 071232" src="https://github.com/user-attachments/assets/9701c046-eeea-4fc7-9409-c883bd257adb" />
<img width="500" alt="Screenshot 2026-03-13 071312" src="https://github.com/user-attachments/assets/62e40c96-6cb0-4e49-ac65-01a4a992ba48" />
<img width="500" alt="Screenshot 2026-03-13 071336" src="https://github.com/user-attachments/assets/90e892c3-7dd3-432f-9ef2-6fcf5b08589c" />
<img width="500" alt="Screenshot 2026-03-13 071445" src="https://github.com/user-attachments/assets/bbe8126b-93d3-41a1-808a-b07de0710c38" />
<img width="500" alt="Screenshot 2026-03-13 071500" src="https://github.com/user-attachments/assets/61ef53c3-cae7-4d64-b93d-28a3f34a5c91" />
<img width="300" alt="Screenshot 2026-03-13 071516" src="https://github.com/user-attachments/assets/7f489d84-71a7-46c0-8bbc-21a265a067fc" />

## Step 3 — Cek Status Verifikasi
 
Ada **2 cara** mengecek apakah email sudah diverifikasi:
 
| Cara | Endpoint | Kapan Dipakai |
|---|---|---|
| **Cara A** | Firebase `accounts:lookup` | Debugging, backend belum siap |
| **Cara B** | Backend `/auth/verify-token` | Testing alur login normal, sekalian dapat Backend Token |
 
### Cara A — Cek Langsung ke Firebase
 
```
POST https://identitytoolkit.googleapis.com/v1/accounts:lookup?key={{FIREBASE_API_KEY}}
```
 
**Request Body:**
```json
{
  "idToken": "{{FIREBASE_ID_TOKEN}}"
}
```
 
**Response — Email BELUM diverifikasi:**
```json
{
  "users": [
    {
      "localId": "aBcDeFgHiJkLmN",
      "email": "test@example.com",
      "emailVerified": false,
      ...
    }
  ]
}
```
 
**Response — Email SUDAH diverifikasi:**
```json
{
  "users": [
    {
      "localId": "aBcDeFgHiJkLmN",
      "email": "test@example.com",
      "emailVerified": true,
      ...
    }
  ]
}
```
## Contoh

<img width="500"  alt="Screenshot 2026-03-13 072430" src="https://github.com/user-attachments/assets/74904e60-3775-4695-86ee-04e2dba47651" />
<img width="500"  alt="Screenshot 2026-03-13 072451" src="https://github.com/user-attachments/assets/dc22a23a-25fb-4808-8c2d-1d0590e20606" />
<img width="500" alt="Screenshot 2026-03-13 072510" src="https://github.com/user-attachments/assets/835d793c-75db-40ae-9657-a05f111bc17b" />

