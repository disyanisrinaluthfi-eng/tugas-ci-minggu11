# Tugas Minggu 11: CI untuk Repo Compose
![CI](https://github.com/disyanisrinaluthfi-eng/tugas-ci-minggu11/actions/workflows/compose-check.yml/badge.svg)

**Nama:** [Disya Nisrina Luthfi]
**NIM:** [202557201082]
**Mata kuliah:** Teknologi Open Source

## Deskripsi
Workflow ini memvalidasi sintaks file docker-compose.yml (WordPress + MariaDB) secara otomatis setiap kali ada push ke branch main atau ada Pull Request.

## Isi repositori
- .github/workflows/compose-check.yml : workflow CI
- docker-compose.yml : stack WordPress + MariaDB (tugas minggu 10)
- .env.example : contoh variabel lingkungan tanpa rahasia asli
- .gitignore : mengabaikan .env

## Cara kerja workflow
✏Workflow bernama Compose Check dipicu oleh dua kejadian, yaitu push ke branch main dan pull_request. Di dalamnya ada satu job bernama validate yang berjalan di runner ubuntu-latest milik GitHub. Job ini punya tiga step. Pertama, actions/checkout mengambil isi repositori ke runner. Kedua, perintah cp .env.example .env membuat file .env dari contoh, karena .env asli tidak ada di repositori dan variabelnya dibutuhkan oleh docker-compose.yml. Ketiga, docker compose config -q memeriksa sintaks file Compose tanpa menjalankan container. Jika semua step berhasil, job berwarna hijau, dan jika ada kesalahan, job berwarna merah.

## Bukti
1. Run hijau di tab Actions
![Run hijau](bukti/1-run-hijau.png)
2. Pull Request dengan check gagal (merah)
![PR merah](bukti/2-pr-merah.png)
3. Badge status di README
![Badge](bukti/3-badge.png)

Tautan Pull Request (sudah hijau dan di-merge): https://github.com/disyanisrinaluthfi-eng/tugas-ci-minggu11/pull/1

## Kendala dan cara mengatasinya
Saat pertama kali di-push, file .env.example ternyata masih berisi password asli dari tugas minggu 10 (wppass123 dan rootpass123). Karena repositori ini publik dan password itu sudah masuk ke riwayat commit, saya mengganti isinya dengan nilai contoh, lalu menimpa commit pertama dengan git commit --amend dan git push --force. Setelah itu saya memeriksa dengan git grep bahwa password lama sudah tidak ada di riwayat commit.
## Refleksi singkat
✏️Dengan CI, kesalahan pada file Compose langsung ketahuan di GitHub tanpa perlu menjalankannya satu per satu di laptop. Dalam tugas ini, Pull Request yang merah mencegah file yang rusak masuk ke main begitu saja. Selain itu, CI membuat pengerjaan kelompok lebih aman, karena setiap perubahan dari anggota mana pun diperiksa dengan cara yang sama.
