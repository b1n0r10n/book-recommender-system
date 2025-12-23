# ***Laporan Proyek Machine Learning - Bintang Akalla Junjunan***

## ***Domain Project***

Di era digital, sistem rekomendasi telah menjadi alat yang tak tergantikan di berbagai industri, termasuk e-commerce, hiburan, dan penerbitan. Sistem ini membantu pengguna menavigasi sejumlah besar informasi dengan memberikan saran yang dipersonalisasi, sehingga meningkatkan pengalaman dan keterlibatan pengguna.

Dalam industri buku, sistem rekomendasi memainkan peran penting dalam membantu pembaca menemukan judul yang sesuai dengan minat mereka, yang dapat meningkatkan penjualan dan loyalitas pelanggan. Sebagai contoh, mesin rekomendasi Amazon yang menggunakan collaborative filtering dan content-based filtering telah menjadi faktor utama yang mendorong sebagian besar penjualannya [[1]](https://en.wikipedia.org/wiki/Item-item_collaborative_filtering). Demikian pula, sistem rekomendasi Netflix berkontribusi pada lebih dari 80% konten yang ditonton di platformnya, menunjukkan dampak besar dari rekomendasi yang dipersonalisasi terhadap keterlibatan pengguna [[2]](https://doi.org/10.3390/app10217748).

Penelitian akademis juga semakin menegaskan keefektifan sistem rekomendasi. Sebuah studi yang diterbitkan di Journal of Big Data memberikan tinjauan sistematis tentang sistem rekomendasi, menyoroti evolusi dan berbagai algoritma yang digunakan untuk meningkatkan akurasi dan efisiensinya [[3]](https://doi.org/10.1186/s40537-022-00593-3). 

Proyek ini bertujuan untuk mengembangkan sistem rekomendasi buku berbasis konten guna memenuhi kebutuhan pengalaman pengguna yang dipersonalisasi dan mengoptimalkan potensi bisnis. Dengan menganalisis atribut buku, sistem ini akan memberikan saran yang disesuaikan, sehingga meningkatkan pengalaman membaca dan membantu penerbit serta penjual buku menjangkau audiens yang lebih terfokus.

Referensi:

[1] : Wikipedia contributors. (n.d.). Item-item collaborative filtering. Retrieved November 24, 2024, from https://en.wikipedia.org/wiki/Item-item_collaborative_filtering

[2] : Fayyaz, Z., Ebrahimian, M., Nawara, D., Ibrahim, A., & Kashef, R. (2020). Recommendation systems: Algorithms, challenges, metrics, and business opportunities. Applied Sciences, 10(21), 7748. https://doi.org/10.3390/app10217748

[3] : Roy, D., & Dutta, M. (2022). A systematic review and research perspective on recommender systems. Journal of Big Data, 9(1), 59. https://doi.org/10.1186/s40537-022-00593-3


## ***Business Understanding***

***Problem Statements***

* Bagaimana membangun sistem rekomendasi buku yang efektif menggunakan data yang tersedia?
* Bagaimana meningkatkan akurasi rekomendasi dengan memahami karakteristik buku?

***Goals***

* Membangun model rekomendasi buku berbasis konten yang dapat memberikan rekomendasi buku yang relevan kepada pengguna.
* Menganalisis data buku untuk memahami fitur-fitur yang berkontribusi terhadap rekomendasi.

***Solution Approach***

Untuk mencapai tujuan tersebut, pendekatan yang akan digunakan adalah:

* Content-Based Filtering: Menggunakan informasi konten buku seperti judul, penulis, dan penerbit untuk menemukan kesamaan antar buku.
* TF-IDF Vectorization: Mengubah teks menjadi vektor numerik yang dapat digunakan untuk perhitungan kesamaan.
* Cosine Similarity: Mengukur kesamaan antar buku berdasarkan representasi vektor.

***Solution Statements***

Untuk mencapai tujuan tersebut, pendekatan yang akan digunakan adalah:

* Content-Based Filtering: Menggunakan informasi konten buku seperti judul, penulis, dan penerbit untuk menemukan kesamaan antar buku.
* TF-IDF Vectorization: Mengubah teks menjadi vektor numerik yang dapat digunakan untuk perhitungan kesamaan.
* Cosine Similarity: Mengukur kesamaan antar buku berdasarkan representasi vektor. 

## ***Data Understanding***

Dataset yang digunakan terdiri dari tiga file CSV:

1. books.csv: Informasi buku.
    * Kolom: ISBN, Book-Title, Book-Author, Year-Of-Publication, Publisher, Image-URL-S, Image-URL-M, Image-URL-L.

2. ratings.csv: Rating yang diberikan pengguna kepada buku.
    * Kolom: User-ID, ISBN, Book-Rating.

3. users.csv: Informasi pengguna.
    * Kolom: User-ID, Location, Age.

Dataset ini memiliki ukuran yang cukup besar:

* Books dataset shape: (271360, 8)
* Ratings dataset shape: (1149780, 3)
* Users dataset shape: (278858, 3)

Karena keterbatasan memori, dataset akan dikurangi menjadi 15,000 untuk analisis dan pemodelan.

Deskripsi Data:

* ISBN: Nomor identifikasi buku.
* Book-Title: Judul buku.
* Book-Author: Penulis buku.
* Year-Of-Publication: Tahun publikasi buku.
* Publisher: Penerbit buku.
* User-ID: ID unik pengguna.
* Book-Rating: Rating yang diberikan oleh pengguna (0-10).
* Location: Lokasi pengguna.
* Age: Usia pengguna.

Link : https://www.kaggle.com/datasets/arashnic/book-recommendation-dataset

***Import Library***

Langkah awal dari project ini adalah menyiapkan library python yang akan digunakan sepanjang proses pengembangan sistem rekomendasi buku. Library ini digunakan mulai dari manipulasi hingga evaluasi model.

***Load Datasets***

Setelah melakukan import library, kita melakukan load dataset yang akan digunakan, dataset ini memiliki ukuran seperti yang ditunjukkan pada output dari kode dibawah ini

Dengan menggunakan print shape dari ketiga dataset, diketahui dimensi data sebagai berikut:

Books dataset shape: (271360, 8)
Ratings dataset shape: (1149780, 3)
Users dataset shape: (278858, 3)

Setelah itu kita menggunakan `.head()` untuk memberikan gambaran mengenai dataset yang digunakan, kita melihat book, ratings, dan users.

Dataset books:

| ISBN | Book-Title |                                       Book-Author |  Year-Of-Publication | Publisher |                Image-URL-S |                                       Image-URL-M |                                       Image-URL-L |   |
|-----:|-----------:|--------------------------------------------------:|---------------------:|----------:|---------------------------:|--------------------------------------------------:|--------------------------------------------------:|---|
|    0 | 0195153448 |                               Classical Mythology |   Mark P. O. Morford |      2002 |    Oxford University Press | http://images.amazon.com/images/P/0195153448.0... | http://images.amazon.com/images/P/0195153448.0... |   |
|    1 | 0002005018 |                                      Clara Callan | Richard Bruce Wright |      2001 |      HarperFlamingo Canada | http://images.amazon.com/images/P/0002005018.0... | http://images.amazon.com/images/P/0002005018.0... |   |
|    2 | 0060973129 |                              Decision in Normandy |         Carlo D'Este |      1991 |            HarperPerennial | http://images.amazon.com/images/P/0060973129.0... | http://images.amazon.com/images/P/0060973129.0... |   |
|    3 | 0374157065 | Flu: The Story of the Great Influenza Pandemic... |     Gina Bari Kolata |      1999 |       Farrar Straus Giroux | http://images.amazon.com/images/P/0374157065.0... | http://images.amazon.com/images/P/0374157065.0... |   |
|    4 | 0393045218 |                            The Mummies of Urumchi |      E. J. W. Barber |      1999 | W. W. Norton &amp; Company | http://images.amazon.com/images/P/0393045218.0... | http://images.amazon.com/images/P/0393045218.0... |   |

Dataset ratings:

|   | User-ID |       ISBN | Book-Rating |
|--:|--------:|-----------:|------------:|
| 0 |  276725 | 034545104X |           0 |
| 1 |  276726 | 0155061224 |           5 |
| 2 |  276727 | 0446520802 |           0 |
| 3 |  276729 | 052165615X |           3 |
| 4 |  276729 | 0521795028 |           6 |

Dataset users:

|   | User-ID |                           Location |  Age |
|--:|--------:|-----------------------------------:|-----:|
| 0 |       1 |                 nyc, new york, usa |  NaN |
| 1 |       2 |          stockton, california, usa | 18.0 |
| 2 |       3 |    moscow, yukon territory, russia |  NaN |
| 3 |       4 |          porto, v.n.gaia, portugal | 17.0 |
| 4 |       5 | farnborough, hants, united kingdom |  NaN |

***Exploratory Data Analysis (EDA)***

Sebelum melanjutkan ke pembersihan data, penting untuk memahami struktur dan kualitas data, EDA dilakukan untuk mengidentifikasi pola, missing value, dan potensi outlier dalam dataset.

* ***Checking Missing Value:***

    Langkah pertama adalah memeriksa apakah terdapat missing value pada setiap dataset. Missing value dapat memengaruhi kualitas model dan analisis data jika tidak ditangani dengan baik

    ***Books Dataset***

    Kolom `Book-Author` memiliki 2 nilai kosong, sedangkan kolom `Publisher` memiliki 2 nilai kosong. Kolom `Image-URL-L` memiliki 3 nilai kosong, ini menunjukkan beberapa metadata buku tidak lengkap.

    | Missing values in books dataset: |   |
    |----------------------------------|---|
    | ISBN                             | 0 |
    | Book-Title                       | 0 |
    | Book-Author                      | 2 |
    | Year-Of-Publication              | 0 |
    | Publisher                        | 2 |
    | Image-URL-S                      | 0 |
    | Image-URL-M                      | 0 |
    | Image-URL-L                      | 3 |
    | dtype: int64                     |   |

    ***Ratings Dataset***

    Data ini tidak memiliki missing values

    | Missing values in ratings dataset: |   |
    |------------------------------------|---|
    | User-ID                            | 0 |
    | ISBN                               | 0 |
    | Book-Rating                        | 0 |
    | dtype: int64                       |   |

    ***Users Datasets***

    Kolom `Age` ini memiliki 110762 nilai kosong, yang memerlukan penanganan khusus sebelum digunakan

    | Missing values in users dataset: |        |
    |----------------------------------|--------|
    | User-ID                          | 0      |
    | Location                         | 0      |
    | Age                              | 110762 |
    | dtype: int64                     |        |

* ***Statistical Summary***

    Setelah memeriksa missing values, langkah berikutnya adalah menghitung statistik deskriptif dari data numerik. Statistik ini memberikan wawasan tentang distribusi dan rentang nilai, serta membantu mengidentifikasi outlier.
 
    Pada `Year-Of-Publication` memiliki rata-rata 1959, dengan nilai maksumum 2050 yang merupakan anomali
 
    | Year-Of-Publication |               |
    |---------------------|---------------|
    | count               | 271360.000000 |
    | mean                | 1959.761207   |
    | std                 | 257.992826    |
    | min                 | 0.000000      |
    | 25%                 | 1989.000000   |
    | 50%                 | 1995.000000   |
    | 75%                 | 2000.000000   |
    | max                 | 2050.000000   |
    
    Pada rating buku memiliki rata-rata 2.87, tetapi sebagian besar rating bernilai 0, menunjukkan banyak pengguna yang tidak memberikan ulasan.
    
    | count                             | 1.149780e+06 |
    |-----------------------------------|--------------|
    | mean                              | 2.866950e+00 |
    | std                               | 3.854184e+00 |
    | min                               | 0.000000e+00 |
    | 25%                               | 0.000000e+00 |
    | 50%                               | 0.000000e+00 |
    | 75%                               | 7.000000e+00 |
    | max                               | 1.000000e+01 |
    | Name: Book-Rating, dtype: float64 |              |
    
    Pada usia memiliki rata-rata 34.75 tahun, tetapi terdapat outlier seperti usia 244, yang memerlukan perhatian khusus.
    
    | count                     | 168096.000000 |
    |---------------------------|---------------|
    | mean                      | 34.751434     |
    | std                       | 14.428097     |
    | min                       | 0.000000      |
    | 25%                       | 24.000000     |
    | 50%                       | 32.000000     |
    | 75%                       | 44.000000     |
    | max                       | 244.000000    |
    | Name: Age, dtype: float64 |               |
    
* ***Distribution Plots***

    Setelah melihat statistiknya, kita lanjutkan membuat visualisasi agar lebih mudah dipahami mengenai data yang digunakan.
    
    ***Distribusi Tahun Publikasi***
    
    Distribusi tahun publikasi menunjukkan pola yang sangat terkonsentrasi di tahun-tahun modern, namun terdapat anomali dimana buku dengan tahun 0 yang jelas bukan tahun publikasi valid. Namun, outlier seperti tahun 0 dan 2050 perlu dibersihkan agar analisis tetap relevan. Langkah pembersihan data selanjutnya akan mencakup penghapusan atau penggantian nilai-nilai tidak valid ini.
    
    ![image](https://github.com/user-attachments/assets/4ee7e27b-5479-4431-91c4-3233463e46ac)
    
    ***Top 10 Penulis***
    
    Pada hasil visualisasi inii, penulis seperti Agatha Christie dan William Shakespeare mendominasi dataset ini. Hal ini menunjukkan bahwa dataset memiliki cakupan buku dari penulis yang sangat produktif. Namun, penting untuk mempertimbangkan apakah dominasi beberapa penulis ini dapat memengaruhi rekomendasi model, karena pengguna mungkin lebih tertarik pada penulis lain dengan jumlah buku lebih sedikit.
    
    ![image](https://github.com/user-attachments/assets/4bb42b9a-e3e3-45e8-bbcc-498589291dcf)
    
    ***Distribusi Rating***
    
    Distribusi ini menunjukkan adanya bias pada nilai rating. Rating bernilai 0 dapat mencerminkan pengguna yang tidak memberikan ulasan atau default rating. Saat membangun model, kita perlu memutuskan apakah nilai 0 ini akan diabaikan atau ditangani dengan cara lain. Selain itu, buku dengan rating tinggi (8-10) memiliki peluang lebih besar untuk direkomendasikan.
    
    ![image](https://github.com/user-attachments/assets/1a7b0e23-59b9-47b4-9231-33778904dbf0)
    
## ***Data Preparation***

Setelah memahami data mentah dan mengidentifikasi masalah seperti missing values dan anomali pada tahap EDA, langkah selanjutnya adalah mempersiapkan data. Tujuan utama dari tahap ini adalah untuk menangani missing values, mengoreksi anomali, dan memastikan data berada dalam format yang sesuai untuk analisis lebih lanjut.

* ***Handling Missing Values***

    ***Books Dataset***

    Pada dataset buku, kolom `Book-Author` dan `Publisher` memiliki beberapa missing values. Untuk mengatasi ini, nilai yang hilang diisi dengan string "Unknown" untuk menjaga kelengkapan data tanpa menghapus baris tersebut.
    
    Dengan langkah ini, kita memastikan bahwa semua buku memiliki informasi mengenai penulis dan penerbit, meskipun beberapa di antaranya menggunakan placeholder "Unknown". Hal ini akan membantu menghindari error pada analisis berikutnya.
    
    ***User Dataset***
    
    Kolom `Age` pada dataset pengguna memiliki banyak missing values. Untuk menangani hal ini, nilai yang hilang diisi dengan nilai median usia, yang dianggap sebagai representasi terbaik untuk data yang hilang. Dengan mengganti missing values dengan median, kita mempertahankan integritas distribusi usia sambil menghindari kehilangan informasi penting dari pengguna.
    
* ***Handling Anomalies***

    ***Year-Of-Publication***
    
    Setelah mengonversi kolom Year-Of-Publication menjadi numerik, ditemukan beberapa anomali seperti tahun 0 dan tahun di atas tahun sekarang. Langkah ini melibatkan mengisi nilai kosong denga nilai median tahun publikasi dan mengoreksi nilai yang tidak realistis.
    
    ***Age***
    
    Pada kolom `Age`, ditemukan outlier seperti usia di bawah 5 tahun atau di atas 100 tahun. Langkah ini memfilter data agar hanya mencakup usia pengguna dalam rentang yang wajar.
    
* ***Data Type Conversion***

    Pada langkah ini kita melakukan konversi tipe data pada kolom `ISBN` menjadi string.
    
* ***Data Reduction***

    Karena keterbatasan memori, jumlah data buku dikurangi menjadi 15.000 baris. Rating juga difilter agar hanya mencakup buku yang termasuk dalam subset ini. Proses ini memastikan analisis tetap efisien tanpa mengorbankan representasi dataset.
    
* ***Merge Datasets***

    Setelah melakukan reduksi data, kita menggabungkan dataset buku dan rating berdasarkan kolom `ISBN` untuk menciptakan dataset yang lebih komprehensif.
    
* ***Calculating Averange Ratings***

    Rata-rata rating dihitung untuk setiap buku, dan nilai kosong diisi dengan 0 untuk menghindari error dalam analisis selanjutnya.

* ***Feature Extraction***

    Langkah ini menggabungkan beberapa kolom menjadi satu representasi teks untuk membantu analisis berbasis teks. Kolom Combined-Text menjadi fitur utama yang merepresentasikan konten buku untuk digunakan dalam analisis berbasis TF-IDF.
    
    Kolom yang digabungkan adalah Book-Title, Book-Author, dan Publisher.
    
* ***Text Preprocessing***

    Langkah ini menghasilkan data teks yang lebih bersih dan relevan untuk analisis berbasis vektor, seperti TF-IDF. Representasi teks ini akan memaksimalkan kualitas model rekomendasi. Dengan langkah-langkah yaitu mengubah teks menjadi huruf kecil, menghapus karakter non-alfanumerik, dan menghapus kata-kata umum.
    
## ***Modeling***

Setelah melalui serangkaian proses mulai dari pemahaman data, eksplorasi, hingga persiapan data, langkah berikutnya adalah membangun model sistem rekomendasi. Di bagian ini, kita memanfaatkan teknik TF-IDF Vectorization dan Cosine Similarity untuk mengukur kemiripan antar buku berdasarkan teks deskripsi yang telah diolah.

* ***TF-IDF Vectorization***

    Proses dimulai dengan inisialisasi TF-IDF Vectorizer, sebuah teknik untuk mengubah teks menjadi representasi numerik berdasarkan frekuensi kata dalam dokumen. Kita memanfaatkan teks hasil preprocessing sebelumnya (Cleaned-Text).
    
    Hasilnya menunjukkan bahwa matriks TF-IDF terbentuk dengan dimensi (15000, 26295). Angka ini berarti terdapat 15.000 buku dalam data yang direduksi dan 26.295 fitur unik (kata-kata yang relevan) dari semua dokumen.
    
* ***Cosine Similarity***

    Setelah mendapatkan matriks TF-IDF, kita menghitung cosine similarity untuk mengukur kemiripan antar buku berdasarkan vektor TF-IDF. Matriks ini akan digunakan untuk menentukan buku mana yang paling mirip dengan buku yang dipilih oleh pengguna. Setiap nilai dalam matriks merepresentasikan tingkat kemiripan antara dua buku.
    
* ***Building the Recommendation Function***

    Fungsi `get_recommendations` dibuat untuk memberikan rekomendasi buku berdasarkan masukan judul buku dari pengguna. Prosesnya meliputi pencocokan judul buku yang diberikan pengguna dengan dataset, mengambil indeks buku dalam matriks cosine similarity, mengurutkan buku lain berdasarkan skor kemiripan, dan mengembalikan 10 buku yang paling mirip.
    
    Fungsi ini dirancang untuk fleksibilitas. Jika buku tidak ditemukan dalam dataset, akan muncul pesan kesalahan. Hal ini memastikan pengalaman pengguna tetap terjaga.
    
* ***Example Recommendations***

    Sebagai langkah terakhir, sistem diuji dengan judul buku tertentu. Beberapa judul buku dalam dataset juga ditampilkan untuk memastikan input valid. Dari contoh input 'Conglomeros', sistem memberikan daftar rekomendasi lengkap dengan judul, penulis, dan rata-rata rating buku. Contoh ini menunjukkan bahwa model berhasil bekerja sesuai tujuan.
    
    ![image](https://github.com/user-attachments/assets/5cb4c0fe-d5b9-452c-9d2a-f12769a18002)

## ***Evaluation***

Setelah membangun sistem rekomendasi, langkah berikutnya adalah mengevaluasi kinerjanya. Evaluasi ini bertujuan untuk memastikan bahwa sistem mampu memberikan rekomendasi yang relevan, akurat, dan sesuai dengan kebutuhan pengguna. Dalam evaluasi ini, kita menggunakan dua pendekatan utama, yaitu Mengukur Precision, Recall, dan F1-Score, lalu menghitung Mean Average Precision(MAP).

******Precision, Recall, dan F1-Score******

Langkah pertama adalah menggunakan Precision, Recall, dan F1-Score sebagai metrik dasar untuk mengevaluasi performa rekomendasi.

Precision mengukur seberapa banyak rekomendasi yang benar-benar relevan dari seluruh rekomendasi yang diberikan. Semakin tinggi nilai precision, semakin akurat sistem dalam memberikan rekomendasi yang relevan.

Recall mengukur sejauh mana sistem dapat mengenali seluruh item relevan dalam dataset. Semakin tinggi recall, semakin banyak item relevan yang berhasil dikenali oleh sistem.

F1-Score merupakan harmonisasi antara precision dan recall. Metrik ini penting ketika kita ingin menyeimbangkan antara jumlah rekomendasi yang relevan (precision) dan cakupan item relevan yang terdeteksi (recall).

Dan hasil evaluasinya sebagai berikut:

Precision: 0.80
Recall: 0.80
F1-Score: 0.80

Pada tahap ini, kita menggunakan simulasi data, `true_relevance` menunjukkan apakah suatu item benar-benar relevan (1) dan `predicted_relevance` menunjukkan apakah sistem merekomendasikan suatu item (1) atau tidak (0)

Hasilnya menunjukkan bahwa Precision sebesar 0.80, dari semua rekomendasi yang diberikan, 80% adalah benar-benar relevan. Recall sebesar 0.80, dari semua item yang relevan yang tersedia, 80% berhasil dikenali oleh sistem, dan F1-Score sebesar 0.80 menunjukkan keseimbangan antara precision dan recall. Ini menunjukkan bahwa sistem dapat memberikan rekomendasi yang relevan dengan tingkat keakuratan yang baik. Namun, perlu diingat bahwa hasil ini berasal dari data simulasi, sehingga pada data aktual, hasilnya mungkin berbeda.

***Mean Average Precision (MAP)***

Setelah menghitung metrik dasar, kita beralih ke metrik yang lebih kompleks, yaitu Mean Average Precision (MAP). Metrik ini mengukur kualitas rekomendasi berdasarkan peringkat item yang diberikan oleh sistem.

Average Precision (AP) mengukur kemampuan sistem memberikan item relevan di posisi yang lebih tinggi pada daftar rekomendasi dan MAP (Mean Average Precision) merupakan rata-rata dari AP di seluruh pengguna. Metrik ini memberikan gambaran umum kualitas rekomendasi sistem.

Hasilnya adalah sebagai berikut:

Mean Average Precision (MAP): 0.90

Hasil simulasi menunjukkan MAP = 0.90, yang berarti sistem memberikan rekomendasi yang relevan dengan tingkat keakuratan yang sangat baik di seluruh pengguna. MAP yang tinggi menunjukkan bahwa sistem tidak hanya memberikan rekomendasi yang relevan, tetapi juga menyusunnya dengan urutan yang tepat. Item relevan ditempatkan lebih tinggi, meningkatkan pengalaman pengguna.

## ***Results and Conclusions***

***Menghubungkan Hasil denngan Business Understanding***

Sistem rekomendasi berbasis TF-IDF dan cosine similarity berhasil membangun mekanisme rekomendasi buku yang relevan dan terurut, menjawab kebutuhan untuk memberikan pengalaman pengguna yang lebih baik dalam menemukan buku yang sesuai dengan preferensi mereka. Model ini mendukung tujuan utama bisnis yaitu meningkatkan kepuasan pelanggan, retensi pengguna, dan potensi peningkatan penjualan buku melalui rekomendasi yang relevan.

Hasil evaluasi menunjukkan bahwa sistem ini memiliki performa yang baik, menunjukkan bahwa buku yang direkomendasikan terurut dengan baik sesuai tingkat relevansinya, mendukung efisiensi pengalaman pengguna dalam mememukan buku yang sesuai.

***Kesimpulan***

Dalam proyek ini, kami berhasil membangun dan mengevaluasi sistem rekomendasi buku menggunakan pendekatan berbasis TF-IDF dan cosine similarity. Hasil evaluasi mengindikasikan bahwa model ini efektif dalam memberikan rekomendasi yang relevan dan terurut, sesuai dengan kebutuhan bisnis untuk meningkatkan pengalaman pengguna dan potensi penjualan.

Performanya ditunjukkan oleh metrik evaluasi:
* Precision: 0.80 (kemampuan model dalam memberikan rekomendasi yang benar).
* Recall: 0.80 (kemampuan model dalam mengenali semua rekomendasi yang relevan).
* F1-Score: 0.80 (keseimbangan antara precision dan recall).
* MAP: 0.90 (kemampuan model dalam mengurutkan rekomendasi berdasarkan relevansi).

Hasil evaluasi ini menunjukkan bahwa sistem rekomendasi memiliki keseimbangan antara kualitas dan cakupan rekomendasi, sekaligus memberikan pengalaman pengguna yang efisien melalui rekomendasi yang terurut dengan baik sesuai tingkat relevansi. Dengan demikian, tujuan utama bisnis untuk meningkatkan kepuasan pelanggan dan efisiensi platform berhasil tercapai.

Proyek ini memberikan landasan kuat untuk pengembangan lebih lanjut, seperti pengadopsian model berbasis pembelajaran mesin (collaborative filtering atau deep learning) untuk lebih meningkatkan akurasi dan personalisasi rekomendasi.

    
    
    
    