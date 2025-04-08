# Laporan Proyek Machine Learning - Muhammad Faizal Pratama

## Domain Proyek

Sistem rekomendasi film merupakan salah satu fitur penting dalam industri hiburan, terutama pada platform streaming seperti Netflix, Disney+, dan Amazon Prime. Dengan banyaknya pilihan film yang tersedia, pengguna dapat merasa kebingungan untuk memilih tontonan yang sesuai dengan preferensi mereka.
Salah satu pendekatan yang digunakan untuk mengatasi masalah ini adalah dengan Content-Based Filtering, yaitu merekomendasikan film berdasarkan kemiripan konten seperti genre, sinopsis, atau sutradara.

Referensi:
- A Survey of Recommender Systems Based on Content-Based Filtering
- Statista - Number of users in the Video Streaming segment

## Business Understanding
Problem Statements
- Bagaimana membangun sistem rekomendasi film yang dapat memberikan rekomendasi relevan berdasarkan preferensi pengguna?
- Bagaimana menggunakan informasi genre film untuk mengukur kemiripan antar film?

### Goals

Menjelaskan tujuan dari pernyataan masalah:
- Mengembangkan sistem rekomendasi yang dapat menyarankan 5 film serupa berdasarkan satu input judul film.
- Meningkatkan pengalaman pengguna dalam memilih film melalui rekomendasi berbasis konten.

    ### Solution statements
    - Menggunakan algoritma Content-Based Filtering dengan pendekatan Bag-of-Words untuk mengubah genre menjadi representasi numerik.
    - Mengukur kemiripan antar film menggunakan cosine similarity dari genre vector.

## Data Understanding
Dataset yang digunakan adalah MovieLens 100K dari GroupLens. Dataset ini berisi metadata dan rating dari ribuan pengguna terhadap berbagai film.

Sumber: MovieLens 100K Dataset / https://grouplens.org/datasets/movielens/100k/

Dalam proyek ini, hanya file u.item yang digunakan, yang berisi informasi film dan genre.
- Variabel dalam u.item:
- movie_id: ID unik untuk film
- title: Judul film
- release_date: Tanggal rilis
- genre: Terdiri dari 19 kolom genre biner seperti Action, Comedy, Romance, dll

## Data Preparation
Beberapa tahapan preprocessing dilakukan:
- Seleksi kolom: Mengambil hanya kolom penting seperti title dan genre
- Menggabungkan genre: Genre dibuat menjadi satu kolom genre_text dengan format string
Vectorization: Menggunakan CountVectorizer dari scikit-learn untuk mengubah teks genre menjadi fitur numerik
Tahapan ini penting agar sistem bisa membaca konten genre sebagai masukan bagi perhitungan kemiripan film.

## Modeling
Modeling dilakukan dengan pendekatan Content-Based Filtering:
1. CountVectorizer: Membentuk matriks genre berdasarkan frekuensi token
2. Cosine Similarity: Mengukur jarak antar film berdasarkan vektor genre
3. Fungsi rekomendasi: Mengambil film dengan skor kemiripan tertinggi
Kelebihan pendekatan ini:
- Tidak memerlukan data user/rating
- Dapat digunakan pada cold-start item (film baru)
Kekurangan:
- Rekomendasi terbatas pada konten yang mirip secara literal (misal genre saja)

## Evaluation
Karena model ini tidak memprediksi label atau rating, evaluasi dilakukan secara manual dan visual:
- Visualisasi genre paling banyak: Memahami distribusi genre dalam dataset
- Heatmap cosine similarity: Mengecek apakah genre yang mirip menghasilkan kemiripan tinggi
- Evaluasi manual: Memeriksa apakah film hasil rekomendasi memiliki genre yang serupa dengan input

Contoh:

- Input: Toy Story (1995) → Genre: Animation, Children’s, Comedy
- Rekomendasi: Film dengan genre serupa seperti James and the Giant Peach (1996), Babe (1995), dll
- Hal ini menunjukkan sistem mampu memberikan rekomendasi berdasarkan kemiripan konten dengan cukup baik.
