# Bisnis Catering — Website (Next.js + Tailwind CSS)

README ini menjelaskan spesifikasi, struktur, dan panduan pengembangan untuk website bisnis catering yang dibangun dengan Next.js dan Tailwind CSS. Dokumen ini ditujukan untuk pengembang yang akan membangun, menjalankan, dan menerapkan situs.

## Ringkasan proyek
Website ini menyajikan:
- Halaman landing / home yang menarik
- Daftar paket catering dan rincian menu
- Form pemesanan / booking (dengan validasi)
- Halaman testimonial dan galeri foto
- Dashboard admin sederhana (opsional) untuk mengelola pesanan
- SEO dasar, performa, dan aksesibilitas

Tech stack utama:
- Next.js (React + SSG/SSR/ISR)
- Tailwind CSS untuk styling utility-first
- TypeScript (direkomendasikan) atau JavaScript
- API endpoint serverless (Next API Routes) atau backend terpisah
- Penyimpanan gambar: lokal / Cloud (S3, Cloudinary)
- Deploy: Vercel (direkomendasikan) atau Netlify

---

## Fitur & Halaman Utama
- Home: hero, keunggulan, CTA
- Menu / Paket: daftar paket, filter berdasarkan kategori / harga
- Detail Paket: menu, harga per porsi, galeri, opsi tambah layanan (dekor, waiter)
- Booking: form pemesanan (nama, kontak, tanggal, jumlah tamu, alamat, preferensi)
- FAQ & Kebijakan: pembatalan, pembayaran, jangkauan pengantaran
- Galeri: foto acara & makanan
- Kontak: telepon, WhatsApp, email, peta lokasi
- (Opsional) Admin: lihat & konfirmasi pesanan, export CSV

---

## Struktur Direktori (disarankan)
- /app atau /pages (Next.js 13+: gunakan app router jika diinginkan)
- /components — reusable UI (Hero, Card, Navbar, Footer, Modal, Form)
- /layouts — layout global / halaman
- /lib — helper, api clients, util
- /hooks — custom hooks (useForm, useFetch)
- /styles — konfigurasi Tailwind (tailwind.config.js), global CSS
- /public — gambar statis, favicon
- /pages/api atau /app/api — endpoint serverless
- /data — sample data / mock (opsional)
- /tests — unit / e2e tests (opsional)

Contoh:
```
/components
  /ui
  Navbar.tsx
  Footer.tsx
/pages
  index.tsx
  menu/[slug].tsx
  booking.tsx
  api/
    orders.ts
/styles
  globals.css
tailwind.config.js
next.config.js
```

---

## Styling (Tailwind CSS)
Rekomendasi setup Tailwind:
1. Install
```
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```
2. tailwind.config.js (contoh minimal)
```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: ['./app/**/*.{js,ts,jsx,tsx}','./pages/**/*.{js,ts,jsx,tsx}','./components/**/*.{js,ts,jsx,tsx}'],
  theme: { extend: {} },
  plugins: [],
}
```
3. globals.css
```css
@tailwind base;
@tailwind components;
@tailwind utilities;

/* custom utilities / overrides */
```
4. Rekomendasi UI patterns:
- Variants untuk tombol (primary/secondary)
- Token warna di tailwind.config.js untuk brand
- Utility classes untuk responsif dan aksesibilitas

---

## Setup Pengembangan (local)
1. Clone repo
```
git clone https://github.com/<owner>/<repo>.git
cd <repo>
```
2. Install dependensi
```
npm install
# atau
yarn
```
3. Jalankan development server
```
npm run dev
# atau
yarn dev
```
Buka http://localhost:3000

Script npm (contoh di package.json):
```json
{
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "next lint",
    "format": "prettier --write ."
  }
}
```

---

## Environment Variables (contoh)
Buat file `.env.local`:
```
NEXT_PUBLIC_API_URL=https://api.example.com
NEXT_PUBLIC_GOOGLE_ANALYTICS=G-XXXXXXX
MAIL_TO=orders@namacatering.com
NEXTAUTH_URL=http://localhost:3000
```
- Gunakan prefix NEXT_PUBLIC_ untuk variabel yang dipakai di klien.
- Jangan commit kunci/secret ke repo publik.

---

## Form Pemesanan & Validasi
- Gunakan library form seperti react-hook-form + zod / yup untuk validasi.
- Validasi di client dan server (API route).
- Setelah submit, simpan pesanan ke DB (Firestore, Supabase, atau backend Anda) dan kirim konfirmasi via email/WhatsApp.

Contoh alur:
1. User isi form -> client validates -> POST /api/orders
2. API menyimpan data -> mengirim email konfirmasi & notifikasi WA (via provider)
3. Tampilkan halaman sukses / nomor order

---

## Gambar & Performansi
- Gunakan Next/Image (jika memakai Next.js) untuk optimasi gambar.
- Simpan gambar besar di CDN (Cloudinary, S3) dan gunakan lazy loading.
- Aktifkan caching dan incremental static regeneration (ISR) untuk halaman menu agar cepat.

---

## SEO & Social Sharing
- Set meta tags per halaman (title, description, og:image).
- Gunakan struktur schema.org JSON-LD untuk bisnis lokal (LocalBusiness) — termasuk alamat, jam operasional, nomor telepon.
- Sitemap & robots.txt: generate otomatis pada build.

---

## Aksesibilitas & Internasionalisasi
- Pastikan kontras warna cukup, elemen fokus teratur.
- Gunakan semantic HTML (button, nav, header, main, footer).
- Pertimbangkan i18n/locale jika target pengguna multi-bahasa (Next.js i18n routing).

---

## Testing & Quality
- Linting: ESLint + Prettier
- Unit tests: Jest + React Testing Library (opsional)
- E2E tests: Playwright / Cypress (opsional)
- CI: configure GitHub Actions untuk build & lint sebelum merge

Contoh GitHub Action: build & test pada push ke main.

---

## Deployment
Direkomendasikan: Vercel
- Branch utama otomatis deploy
- Set environment variables di dashboard Vercel
- Enable preview deploys untuk PR

Alternatif: Netlify (menggunakan Next.js adapter) atau Docker.

---

## Admin / Dashboard (opsional)
- Protected route dengan NextAuth.js atau sistem auth lain
- View pesanan, filter berdasarkan status, export CSV, ubah status (confirmed / cancelled)
- Notifikasi via email / WhatsApp saat pesanan masuk

---

## Panduan UI & Konten
- Gunakan foto berkualitas tinggi untuk menu & acara
- Sediakan template menu / PDF yang bisa diunduh
- Testimoni: tampilkan nama & event singkat + foto (opsional)
- CTA jelas pada setiap halaman: Pesan Sekarang / Hubungi Kami

---

## Checklist sebelum go-live
- [ ] Semua halaman responsif (mobile-first)
- [ ] Meta tags & OG untuk setiap halaman
- [ ] Form pemesanan teruji & email konfirmasi berfungsi
- [ ] Backup gambar & data
- [ ] Cek kebijakan privasi & syarat layanan (jika diperlukan)

---

## Contoh Konfigurasi Tailwind & Next (referensi singkat)
tailwind.config.js:
```js
module.exports = {
  content: ['./app/**/*.{ts,tsx,js,jsx}','./components/**/*.{ts,tsx,js,jsx}'],
  theme: { extend: {
    colors: {
      brand: {
        50: '#FFF8F0',
        500: '#D97706',
      }
    }
  }},
  plugins: [],
}
```

next.config.js (contoh jika menggunakan domain image eksternal):
```js
module.exports = {
  images: {
    domains: ['res.cloudinary.com', 'images.unsplash.com'],
  },
}
```

---

## Contributing
- Gunakan branch feature/xxx untuk fitur baru
- Buka PR dengan deskripsi & checklist perubahan
- Sertakan screenshot atau link preview jika ada perubahan UI
- Ikuti aturan coding style (ESLint + Prettier)

---

## License & Credits
- Hak cipta © YYYY Nama Catering Anda
- Lisensi: MIT (sesuaikan jika perlu)

---

## Kontak
- Pemilik / Admin: Nama Pemilik
- Email: orders@namacatering.com
- WhatsApp: +62-812-xxx-xxxx

---

Butuh contoh halaman atau komponen starter (Next.js + Tailwind)? Saya bisa buatkan template dasar (Home, Menu, Booking form) dalam TypeScript jika Anda mau — sebutkan preferensi (app/pages router, TypeScript/JS, dan apakah ingin admin). 
