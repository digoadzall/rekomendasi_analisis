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
File yang digunakan: u.item

Jumlah Data
- Jumlah baris (data film): 1682
- Jumlah kolom: 24 kolom, terdiri dari:
- 1 kolom ID
- 3 kolom metadata
- 19 kolom genre biner
- 1 kolom URL (tidak digunakan)

Hasil pengecekan nilai kosong:
- video_release_date: memiliki 1682 missing values (seluruh baris kosong), sehingga tidak digunakan dalam proyek.
- IMDb_URL: memiliki 3 missing values, tidak digunakan.
- release_date: memiliki 1 missing value, namun tidak berdampak terhadap proses pembuatan sistem rekomendasi berbasis genre.
- Kolom penting seperti movie_id, title, dan kolom genre tidak memiliki missing values.

Duplikat
Terdapat 18 duplikat pada kolom title, namun kemungkinan besar film berbeda bisa memiliki judul yang sama. Untuk proyek ini, movie_id tetap menjadi ID unik utama.
 
Berikut penjelasan masing-masing kolom yang terdapat dalam u.item:

Kolom	Deskripsi
- movie_id	ID unik dari masing-masing film
- title	Judul film
- release_date	Tanggal rilis film (format: dd-MMM-yyyy)
- video_release_date	Tanggal rilis dalam bentuk video (banyak bernilai kosong, tidak digunakan)
- IMDb_URL	Tautan ke halaman IMDb (tidak digunakan dalam proyek)
- genre (19 kolom)	Mewakili genre film dalam bentuk biner (1: film memiliki genre tersebut)

Genre yang tersedia antara lain:
Action, Adventure, Animation, Children’s, Comedy, Crime, Documentary, Drama, Fantasy, Film-Noir, Horror, Musical, Mystery, Romance, Sci-Fi, Thriller, War, Western, dan Unknown.

## Data Preparation
Beberapa tahapan preprocessing dilakukan:
- Seleksi kolom: Mengambil hanya kolom penting seperti title dan genre
- Menggabungkan genre: Genre dibuat menjadi satu kolom genre_text dengan format string
Vectorization: Menggunakan CountVectorizer dari scikit-learn untuk mengubah teks genre menjadi fitur numerik
Tahapan ini penting agar sistem bisa membaca konten genre sebagai masukan bagi perhitungan kemiripan film.

## Modeling
Sebelum melakukan perhitungan kesamaan antar film, dilakukan konversi data genre dari format one-hot encoding (19 kolom biner) menjadi satu kolom teks yang merepresentasikan genre tiap film.

Langkah-langkahnya sebagai berikut:

1. Menggabungkan Genre Menjadi Teks
Setiap film memiliki genre dalam format biner (0 atau 1) untuk 19 kategori genre. Untuk mempermudah analisis berbasis teks, genre-genre yang memiliki nilai 1 digabungkan menjadi satu string.
Contoh hasil:
"Action Adventure Sci-Fi"

2. Ekstraksi Fitur dengan CountVectorizer (Bag of Words)
Kolom teks genre yang sudah digabungkan kemudian diekstrak menggunakan CountVectorizer. Teknik ini mengubah teks menjadi representasi numerik berdasarkan frekuensi kemunculan setiap genre.

3. Perhitungan Kemiripan antar Film
Kemiripan antar film dihitung dengan cosine similarity menggunakan hasil vektorisasi dari genre.
Hasilnya adalah sebuah matriks 2D (cosine_sim) yang berisi nilai kesamaan antar semua pasangan film.
Kriteria 8] Data Preparation :
Proses perhitungan kemiripan antar film, khususnya dalam konteks content-based filtering, sebenarnya bukan merupakan bagian dari data preparation. Langkah tersebut lebih tepat dikategorikan sebagai bagian dari tahap modeling. Oleh sebab itu silahkan dihapus saja.

Model sistem rekomendasi yang digunakan dalam proyek ini adalah Content-Based Filtering berbasis genre film. Pendekatan ini merekomendasikan film berdasarkan kesamaan konten antar film.

1. Skema Model
A. Content-Based Filtering (Berbasis Genre):
Representasi Konten:
Genre film diubah menjadi format teks dan diproses menggunakan CountVectorizer untuk membentuk matriks fitur berbasis frekuensi.
B. Perhitungan Kemiripan:
Kemiripan antar film dihitung menggunakan cosine similarity terhadap vektor genre. Skor ini mengukur seberapa mirip dua film berdasarkan genre-nya.
C. Fungsi Rekomendasi:
Sistem mencari film dengan skor cosine similarity tertinggi terhadap film input dan mengembalikan Top-N film sebagai rekomendasi.

2. Kelebihan
- Tidak membutuhkan data pengguna atau histori rating.
- Dapat menangani cold-start untuk item baru (film baru).

3. Kekurangan
- Rekomendasi hanya berdasarkan kemiripan literal genre.
- Tidak mempertimbangkan preferensi pengguna.

Hasil Rekomendasi (Top-5)
Contoh hasil top-5 rekomendasi untuk film Toy Story (1995) berdasarkan cosine similarity genre:

 Judul Film	 Genre
Aladdin and the King of Thieves (1996)	Animation Children's Comedy

Aladdin (1992)	Animation Children's Comedy Musical

Goofy Movie, A (1995)	Animation Children's Comedy Romance

Santa Clause, The (1994)	Children's Comedy

Home Alone (1990)	Children's Comedy

Film Input: Toy Story (1995)
Genre: Animation Children's Comedy

## Evaluation
1. Metrik Evaluasi yang Digunakan
Karena model ini menggunakan pendekatan Content-Based Filtering berbasis atribut kategorikal (genre), maka evaluasi dilakukan dengan pendekatan berbasis konten. Dua metrik yang relevan digunakan:

- Precision: Persentase film yang direkomendasikan yang benar-benar relevan (memiliki genre yang serupa dengan film input).
- Recall: Persentase film relevan yang berhasil ditemukan dari seluruh film relevan dalam dataset.
Dalam konteks ini, dua film dianggap relevan jika mereka memiliki setidaknya satu genre yang sama.

2. Karena hanya digunakan satu skema Content-Based Filtering, maka belum ada perbandingan antar skema. Namun, sistem ini sudah menunjukkan performa yang baik dalam mengenali film-film yang sejenis berdasarkan genre-nya.

3. Keterkaitan dengan Business Understanding
Model ini dibangun untuk menjawab kebutuhan sistem rekomendasi film sederhana yang:
- tidak bergantung pada histori pengguna (solusi untuk masalah cold-start user)
- Dapat bekerja dengan informasi film saja (fitur genre)
- Memberikan rekomendasi cepat dan relevan berdasarkan genre yang disukai pengguna

Ketercapaian Goals:
- Sistem berhasil memberikan rekomendasi film yang mirip secara genre.
- Mendukung peningkatan pengalaman pengguna pada platform film.
- Potensial untuk digunakan sebagai content discovery bagi pengguna baru.
