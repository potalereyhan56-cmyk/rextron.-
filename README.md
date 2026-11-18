# REXTRON — Vercel deployment

- Login/dashboard UI tetap memakai desain asli; logo yang diberikan ditempatkan di samping link `rextron.vercel.app`.
- Tidak menggunakan Supabase.
- Key divalidasi di Vercel Function dan satu key hanya dapat diklaim oleh satu ID browser/perangkat yang tersimpan.
- Penyimpanan lock memakai Redis melalui integrasi Marketplace Vercel (Upstash Redis). Ini diperlukan karena filesystem/serverless Vercel tidak menyediakan penyimpanan permanen bersama.

## Deploy
1. Upload/import folder project ini ke GitHub lalu import repository ke Vercel, atau gunakan Vercel CLI.
2. Di Vercel, buka **Storage / Marketplace** dan buat Redis (Upstash) database untuk project.
3. Pastikan environment variables tersedia pada deployment: `UPSTASH_REDIS_REST_URL` dan `UPSTASH_REDIS_REST_TOKEN` (atau `KV_REST_API_URL` dan `KV_REST_API_TOKEN`).
4. Redeploy project.
5. Tambahkan domain `rextron.vercel.app` pada Project Settings → Domains jika belum terpasang.

## Catatan batas perangkat
Identitas perangkat dibuat sebagai ID acak yang disimpan di browser. Key akan tetap terikat pada ID tersebut di Redis, sehingga browser/perangkat lain ditolak. Ini bukan identifikasi hardware fisik; menghapus data situs/browser dapat membuat perangkat yang sama kehilangan ID dan tidak bisa memakai key itu lagi. Untuk membuka ikatan key, hapus lock `rextron:key-device:<KEY>` dari Redis secara manual.
