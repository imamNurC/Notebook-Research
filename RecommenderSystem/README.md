# Laporan Proyek Machine Learning - Imam Nurcakra

#### Anime Recomendations Content Based Filtering & Collaborative Filtering 

Sistem rekomendasi anime merupakan teknologi yang penting untuk membantu pengguna menemukan anime yang sesuai dengan preferensi pengguna. Mengingat banyaknya anime yang tersedia saat ini, pengguna sering kali merasa kesulitan untuk menemukan anime yang menarik bagi mereka. Tanpa sistem rekomendasi yang baik, pengguna harus menghabiskan banyak waktu untuk mencari dan mengevaluasi berbagai anime secara manual, yang bisa menjadi proses yang membosankan dan tidak efisien.


Masalah dalam rekomendasi anime yaitu Terdapat ribuan anime dengan berbagai genre, tema, dan tipe yang berbeda. Menghadapi volume data yang besar ini membuat pengguna kesulitan menemukan anime yang sesuai dengan preferensinya tanpa bantuan teknologi yang tepat.

Tujuan dalam pembuatan model machine learning pada permasalahan Rekomedasi anime ini yaitu penglaman pengguna yang lebih baik memberikan rekomendasi yang relavan dari judul anime dengan kemiripan genre nya

## Business Understanding


 untuk memahami kebutuhan dan preferensi pengguna. Apa yang menjadi preferensi utama pengguna dalam menonton anime yang di cari pengguna? Apakah mereka mencari anime berdasarkan genre tertentu, tema, atau faktor-faktor lain seperti rating atau durasi episode? Klarifikasi masalah mencakup pemahaman mendalam tentang preferensi dan harapan pengguna terhadap sistem rekomendasi.

Seberapa lengkap dan akuratnya data yang tersedia untuk digunakan dalam sistem rekomendasi ini? Apakah data mencakup informasi yang cukup tentang anime, seperti genre, rating, dan informasi lainnya yang relevan? Ketersediaan dan kualitas data akan mempengaruhi kemampuan sistem untuk memberikan rekomendasi yang akurat dan relevan.

**Problem Statements**

- Bagaimana dapat memberikan rekomendasi anime yang sesuai dengan preferensi pengguna secara akurat dan personal?
- Bagaimana untuk dapat memanfaatkan informasi kolaboratif antar pengguna yang berupaya meningkatkan kualitas rekomendasi anime?

 **Goals :**

 - Mengidentifikasi preferensi dan kecenderungan pengguna terhadap anime.
- Mengembangkan arsitektur model sistem rekomendasi yang mampu memahami dan memprediksi preferensi pengguna dengan tingkat akurasi yang tinggi dengan kesalahan rata rata error(RMSE) pada Train dan Validasi dibawah 0.5



**Solution Statement**:
- Pendekatan Solusi pertama menggunakan Algorithm content based filtering(CBF) cara meraih goals nya dengan tfidf vectorizer dan menghitung tingkat kesamaan dengan cosine similarity. Setelah itu, membuat sejumlah rekomendasi konten anime untuk user berdasarkan kesamaan yang telah dihitung sebelumnya dari interaksi user dengan konten 

- Pendekatan Solusi kedua menggunakan Algorithm Colaborative filtering user based content cara meraih goals nya adalah membuat arsitektur model dense layer pada nilai variable user dan anime dengan target rating

## Data Understanding

Data ini menjelaskan informasi data preferensi pengguna dari 73.516 pengguna pada 12.294 anime. Setiap pengguna(user) dapat menambahkan anime ke daftar lengkapnya dan memberinya peringkat(rating) dan kumpulan data ini adalah kompilasi dari peringkat tersebut

**Statistics Descriptive Anime**

dapat disimpulkan bahwa dataset ini mencakup berbagai anime dengan perbedaan signifikan dalam hal anime_id, rating, dan jumlah anggota. Rating rata-rata menunjukkan bahwa sebagian besar anime memiliki kualitas yang dapat diterima, sementara deviasi standar yang tinggi pada kolom anggota menunjukkan adanya variasi yang besar dalam popularitas anime.


|       | anime_id   | rating   | members     |
|-------|------------|----------|-------------|
| count | 12294.0000 | 12064.00 | 1.2294e+04  |
| mean  | 14058.2216 | 6.4739   | 1.80713e+04 |
| std   | 11455.2947 | 1.0267   | 5.48206e+04 |
| min   | 1.0000          | 1.6700     | 5.000000           |
| 25%   | 3484.2500       | 5.8800     | 2.250000         |
| 50%   | 10260.5000      | 6.5700     | 1.550000        |
| 75%   | 24794.5000      | 7.0000     | 9.437000        |
| max   | 342527.000        | 10.0000      | 1.013917         |

<sub>tabel 1. Statistics descriptive pada data anime</sub>

**Statistics Descriptive Rating**

Dari data ini, bahwa terdapat variasi yang luas baik dalam ID pengguna(user_id) maupun ID anime(anime_id). Rating yang diberikan oleh pengguna juga menunjukkan variasi yang signifikan, yang bisa menunjukkan perbedaan preferensi atau kualitas anime yang beragam. 


|       | user_id     | anime_id    | rating      |
|-------|-------------|-------------|-------------|
| count | 7.813737e+06| 7.813737e+06| 7.813737e+06|
| mean  | 3.672796e+04| 8.909072e+03| 6.144300e+00|
| std   | 2.099759e+04| 8.883950e+03| 3.727800e+00|
| min   | 1.000000e+00| 1.000000e+00| -1.000000e+00|
| 25%   | 1.897400e+04| 1.240000e+03| 6.000000e+00|
| 50%   | 3.679100e+04| 6.213000e+03| 7.000000e+00|
| 75%   | 5.475500e+04| 4.093900e+04| 9.900000e+00|
| max   | 7.351600e+04| 3.451900e+04| 1.000010e+02|

<sub>tabel 2. Statistics descriptive pada data rating</sub>


**Anime Type Exploratory**

Berikut ini adalah persebaran popularitas anime_id berdasarkan kategori tipenya

![anime info](https://raw.githubusercontent.com/imamNurC/Notebook-Research/main/RecommenderSystem/img/exploratory2.png)

<sub>gambar 1. Univariate analysis anime type</sub>

sumber dataset yang digunakan dari kaggle terdiri dari anime.csv dan rating.csv dan untuk lebih detailnya data tersebut diambil dari API myanimelist sebagai berikut
- [Kaggle Anime Recommender system](https://www.kaggle.com/datasets/CooperUnion/anime-recommendations-database/data)
- [anime list API](https://myanimelist.net/)


Variabel-variabel pada Anime Recomendations Kaggle dataset memiliki fitur yang terdiri sebagai berikut:

**Anime.csv**
1. anime_id - id unik dari anime. dari myanime
2. name - nama/judul lengkap anime.
3. genre - kumpulan koma nilai genre.
4. type - movie, TV, OVA, dll.
5. episodes - banyaknya tayangan yang sudah di putar (1 jika movie).
6. rating - rata rata rating untuk 1 anime.
7. members - number of community members that are in this anime's "group".

**Rating.csv**
1. user_id - id pengguna yang dihasilkan secara acak yang berarti user yang telah memberi rating ke anime
2. anime_id - anime yang sudah di rating oleh user
3. rating - peringkat dari 1 sampai 10 yang ditetapkan user ini (-1 jika pengguna menontonnya tetapi tidak memberikan rating).



## Data Preparation
Berikut ini adalah beberapa teknik yang di lakukan pada tahapan data preparation
**1. Pengumpulan Data:**
- Mengumpulkan data dari sumber-sumber seperti API (contohnya MyAnimeList API), atau dataset yang sudah tersedia di platform seperti Kaggle.
- Data yang dikumpulkan termasuk informasi tentang anime (judul, genre, sinopsis, dll) dan data interaksi pengguna (rating, ulasan, dll).

dalam mengumpulkan fitur fitur target yang layak untuk di jadikan sistem rekomendasi, fitur yang paling berpotensi yaitu
Informasi tentang anime(anime_id) dan tentang interaksi pengguna dengan anime itu sendiri (user_id)

**2. Pembersihan Data :**
- Menghapus duplikasi dari data rating.csv pada kolom 'user_id' dan 'anime_id'
- Mengatasi missing values (nilai yang hilang) dengan mengisi nilai -1 menjadi 0 pada kolom rating di tabel rating_df
- menggabungkan data yang sudah dihapus duplikat dan -1 diganti 0 dengan data anime.csv, data di gabung pada kolom 'anime_id' 

pada proses tahapan tersebut untuk memastikan konsistensi dan format data yang seragam sehingga Data yang bersih dan konsisten memastikan bahwa model layak dan tidak terpengaruh oleh noise atau informasi yang salah, yang bisa mengurangi performa rekomendasi.
**Tujuan** :


**3. CF Data preparation :**
Prosesnya ada bebrapa tambahan setelah dilakukan pembersihan data 
- **penghapusan missing value**
pada data preparation CBF menggunakan data yang missing value nya sudah tidak ada, ketika data anime dengan data rating yang sudah di gabungkan 

    **Tujuan** : membantu dalam meningkatkan kualitas dataset. Hal ini memungkinkan model untuk belajar dari data yang lebih konsisten dan dapat diandalkan

- **proses Encoding**
jenis encoding yang di gunakan yakni integer encoding, mengonversi data kategorikal, dalam hal ini user_id dan anime_id, menjadi representasi numerik yang unik       
    
     **Tujuan** : memungkinkan model untuk menangani data kategorikal dan membantu dalam mengurangi dimensi data. Ini juga memfasilitasi operasi seperti perhitungan jarak atau kesamaan antar pengguna atau item dalam sistem rekomendasi.


- **Train-Test Split Data**
memilih subset jumlah data interaksi user dari dataset dengan pembagian 80% untuk pelatihan dan 20% untuk validasi 

     **Tujuan** : Train-test split adalah teknik penting untuk menghindari overfitting, di mana model terlalu spesifik pada data pelatihan dan gagal untuk memprediksi dengan baik pada data baru. Dengan membagi data, dapat dipastikan bahwa model memiliki generalisasi yang baik dan dapat memberikan rekomendasi yang akurat pada pengguna atau item yang belum pernah dilihat sebelumnya.

## Modeling

**Content-Based Filtering (CBF)**

###### **Teknik yang digunakan:**
TF-IDF Vectorizer: Teknik ini digunakan untuk merepresentasikan fitur-fitur dari anime dalam bentuk vektor. TF-IDF (Term Frequency-Inverse Document Frequency) menghitung bobot pentingnya setiap kata dalam deskripsi atau genre anime relatif terhadap seluruh data.

Cosine Similarity: Metode ini digunakan untuk menghitung kesamaan antara anime berdasarkan vektor yang dihasilkan oleh TF-IDF. Cosine similarity mengukur sudut antara dua vektor, yang memberi indikasi seberapa mirip dua anime tersebut.



Teknik ini dipilih karena TF-IDF dan cosine similarity sangat efektif dalam menangani data tekstual seperti genre dan deskripsi anime. dan juga Metode ini relatif mudah diimplementasikan dan cepat dalam proses komputasi, cocok untuk dataset yang besar.

**Kelebihan:**
- Personalisasi Tinggi 
- Tidak Memerlukan Data Pengguna Lain

**Kekurangan :**
- Keterbatasan dalam Menangkap Minat yang Kompleks
- Kesulitan dalam Menangani Preferensi Dinamis


Berikut ini adalah output Top-N dari similiarity anime berdasarkan genre,  

1. Pertama cek dahulu apa agenre dari anime 'Shingeki no Kyojin'

    | id  | judul_anime     | genre                                      |
    |-----|-----------------|--------------------------------------------|
    |16498|Shingeki no Kyojin | Action, Drama, Fantasy, Shounen, Super Power



2. panggil Fungsi anime_recommendations yang akan di tes untuk menampilkan Top-N Similiarity based genre

1.        anime_recomendations('Shingeki no Kyojin')


    | No | Judul Anime                                   | Genre |
    |----|-----------------------------------------------|-------|
    | 0  | Final Fantasy VII: Advent Children            | Action, Fantasy, Super Power |
    | 1  | One Piece Movie 1                             | Action, Adventure, Comedy, Fantasy, Shounen, Super Power |
    | 2  | Katekyo Hitman Reborn!                        | Action, Comedy, Shounen, Super Power |
    | 3  | Hunter x Hunter (2011)                        | Action, Adventure, Shounen, Super Power |
    | 4  | Hunter x Hunter: Greed Island                 | Action, Adventure, Shounen, Super Power |
    | 5  | Hunter x Hunter                               | Action, Adventure, Shounen, Super Power |
    | 6  | Naruto: Takigakure no Shitou - Ore ga Eiyuu Da... | Action, Adventure, Comedy, Shounen, Super Power |
    | 7  | Bleach                                        | Action, Comedy, Shounen, Super Power, Supernatural |
    | 8  | Boku no Hero Academia                         | Action, Comedy, School, Shounen, Super Power |
    | 9  | GetBackers                                    | Action, Comedy, Drama, Mystery, Shounen, Superpower |

    <sub>tabel 3.1. Top-N Content Based filtering dari kemiripan genre anime yang berjudul shingeki no kyojin</sub>

**Collaborative Filtering (CF)**
 
 
**1.  User-Item Matrix**
Membangun matriks pengguna-item di mana setiap entri merepresentasikan penilaian yang diberikan oleh pengguna terhadap anime. Matriks ini akan menjadi dasar untuk menghitung kesamaan antar pengguna atau antar item.

**2. Matrix Factorization (MF)**
Menggunakan teknik matrix factorization seperti Singular Value Decomposition (SVD) atau Alternating Least Squares (ALS) untuk mengurai matriks pengguna-item menjadi dua matriks dengan dimensi lebih rendah. Tujuannya adalah untuk menemukan faktor laten yang merepresentasikan pengguna dan anime:



#### Model Architecture

| Layer (type)               | Output Shape    | Param #   | Connected to         |
|----------------------------|-----------------|-----------|----------------------|
| user (InputLayer)          | (None, 1)       | 0         | -                    |
| anime (InputLayer)         | (None, 1)       | 0         | -                    |
| user_embedding (Embedding) | (None, 1, 128)  | 69,760    | user[0][0]           |
| anime_embedding (Embedding)| (None, 1, 128)  | 69,760    | anime[0][0]          |
| dot_product (Dot)          | (None, 1, 1)    | 0         | user_embedding[0][0], anime_embedding[0][0] |
| flatten_2 (Flatten)        | (None, 1)       | 0         | dot_product[0][0]    |
| dense_2 (Dense)            | (None, 1)       | 2         | flatten_2[0][0]      |
| batch_normalization_2 (BatchNormalization) | (None, 1) | 4 | dense_2[0][0]        |
| activation_2 (Activation)  | (None, 1)       | 0         | batch_normalization_2[0][0] |

<sub>tabel 4. Arsitektur model</sub>

##### Deskripsi Layers :

1. **user (InputLayer)**: Lapisan input untuk ID pengguna.
2. **anime (InputLayer)**: Lapisan input untuk ID anime.
3. **user_embedding (Embedding)**: Lapisan embedding yang mengubah ID pengguna menjadi vektor padat dengan ukuran tetap (128).
4. **anime_embedding (Embedding)**: Lapisan embedding yang mengubah ID anime menjadi vektor padat dengan ukuran tetap (128).
5. **dot_product (Dot)**: Menghitung dot product dari embedding pengguna dan anime untuk mengukur kesamaan.
6. **flatten_2 (Flatten)**: Mengubah output dari dot product menjadi bentuk yang kompatibel dengan lapisan dense.
7. **dense_2 (Dense)**: Lapisan fully connected yang memproses hasil dari flatten dot product.
8. **batch_normalization_2 (BatchNormalization)**: Menormalkan output dari lapisan dense.
9. **activation_2 (Activation)**: Lapisan aktivasi yang menerapkan fungsi aktivasi pada output yang sudah dinormalisasi.


Dalam Project ini Collaborative filtering menampilkan beberapa film yang mendapatkan rating tinggi dari user_id nomor 7 sehingga didapatkan top-N recommendation :

| No. | Judul Anime       | rating |
|-----|-------------------|--------|
| 1   | Oni Chichi        | 7.47 
| 2   | Kuroko no Basket  |  8.46
| 3   | Bleach            | 7.95
| 4   | Dragon Ball       | 8.16
| 5   | Green Green       | 6.44

<sub>tabel 5. Top-N collaborative filtering </sub>

## Evaluation
Metrik Evaluasi yang digunakan yaitu RMSE(Root Mean Squared Error) adalah metrik yang digunakan untuk mengukur kesalahan model dalam memprediksi data kuantitatif, atau bisa juga di sebut rata-rata selisih kuadrat antara nilai prediksi dan nilai aktual

 ![Univariate](https://raw.githubusercontent.com/imamNurC/Notebook-Research/main/RecommenderSystem/img/metrics.png)
 
<sub>gambar 2. Evaluation metrics train dan tes dataset</sub>

berdasarkan metrik tersebut yakni :
- **RMSE pada Data Latih (Biru):** Pada data latihan, nilai Root Mean Squared Error (RMSE) menurun tajam pada awalnya dan kemudian terus menurun dengan laju yang lebih lambat, menunjukkan peningkatan performa selama proses pelatihan, yang ditandai dengan penurunan nilai RMSE pada data latihan.

- **RMSE pada Data Uji (Oranye):**  Tetap relatif stabil sepanjang epoch, menunjukkan tingkat kesalahan yang konsisten dalam prediksi data uji. yang bisa mengindikasikan bahwa model mungkin overfitting terhadap data latih.


**Hasil :**

pada permasalahan content based filtering metode dapat menjawab permasalahan tentang kecenderungan pengguna terhadap anime berdasarkan genre. dan dinyatakan berhasil dengan penjelasan evaluasi metrik sebagai berikut :

## Genre-Precision@10

**Definisi**: Proporsi item yang direkomendasikan dalam 10 besar yang memiliki setidaknya satu genre yang sama dengan genre pilihan penggun

**Formula**: 

![Precision Formula](https://raw.githubusercontent.com/imamNurC/Notebook-Research/main/RecommenderSystem/img/gp10.png)

<sub>formula top 10 genre Presisi</sub>

**Kalkulasi**:
Kesepuluh item yang direkomendasikan memiliki setidaknya satu genre yang sama dengan genre pilihan pengguna.

Genre-Precision@10 = 10/10 = 1.0

## Genre-Recall@10

**Definisi**: Proporsi semua item relevan (berdasarkan genre) yang termasuk dalam 10 rekomendasi teratas.

**Formula**: 

![Precision Formula](https://raw.githubusercontent.com/imamNurC/Notebook-Research/main/RecommenderSystem/img/gr10.png)

<sub>formula top 10 genre recall</sub>

**Kalkulasi**:
Dengan asumsi ada 15 item yang relevan (berdasarkan genre yang tumpang tindih) dalam kumpulan data:

Genre-Precision@10 = 10/15 = 0.667

## F1 Score Calculation

**Definisi**: Rata-rata harmonik Genre-Precision@N dan Genre-Recall@N.


**Formula**:

![Precision Formula](https://raw.githubusercontent.com/imamNurC/Notebook-Research/main/RecommenderSystem/img/f1Score.png)

<sub>formula f1 score berdasarkan ke 10 rekomendasi genre yang relavan </sub>

**Kalkulasi**:
f1@10 = 2 * ((gp@10 * gr@10) / (gp@10 + gr@10) ) 
-  =  2 *  (0.667 / 1.667 ) ≈ 0.8


##### Hasil
Evaluasi metrics menggunakan f1 score tujuannya untuk memberikan pengukuran yang seimbang antara presisi (precision) dan recall untuk mengevaluasi kualitas rekomendasi. Dengan mempertimbangkan kedua aspek ini, F1 score memberikan gambaran yang lebih holistik tentang seberapa baik sistem rekomendasi dapat menyarankan item yang relevan dengan preferensi pengguna. yang dimana dalam hal ini Hasil F1 score mendekati 0.8





namun untuk metode Collaborative filtering 
dalam project ini dinyatakan gagal karena pada metrik tersebut  **overfitting** yakni  garis data uji tidak menunjukkan penurunan yang konsisten. bisa jadi tanda bahwa model terlalu menyesuaikan diri dengan data latih dan tidak menggeneralisasi dengan baik pada data uji.

tapi ada beberapa kemungkinan metode yang harus di perbaiki dari evaluasi ini 
1. Pemilihan fitur/ Feature engineering yang digunakan untuk mempertimbangkan seberapa besar dataset pengaruh terhadap RMSE, dalam project ini  pemilihan fitur terbaik memerlukan proses effort yang lebih rumit, dan belum terlaksana sampai se dalam itu

2. Optimasi Hyperparameter seperti learning rate, jumlah epoch, atau batch size dapat membantu meningkatkan performa model. tetapi dalam project ini saya belum menumukan nilai optimasi yang terbaik




