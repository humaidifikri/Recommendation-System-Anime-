# Laporan Proyek Akhir: Sistem Rekomendasi - Humaidi Fikri
## Domain Proyek
Anime adalah sebuah  jenis animasi khas Jepang yang biasanya menggunakan gambar-gambar berwarna-warni untuk menggambarkan tokoh-tokoh dalam berbagai macam cerita dan latar belakang. Anime dapat digambar dengan tangan atau menggunakan teknologi komputer dan dipengaruhi oleh gaya gambar manga, komik khas Jepang. Anime dapat berbentuk serial TV, film, atau OVA (Original Video Animation) dan memiliki beragam genre, seperti fantasi, aksi, petualangan, romantis, dan horor[1]. Seiring berjalannya waktu jumlah anime dari tahun ke tahun mengalami peningkatan dratis dan menjadi sangat populer akhir-akhir ini di Indonesia. Banyak situs/aplikasi online untuk menonton anime online namun, terlalu banyak anime menjadikan penonton lebih banyak menghabiskan waktunya untuk mencari anime dari pada menontonnya. 

Berdasarkan masalah di atas maka proyek ini dilakukan untuk memberi rekomendasi anime kepada pengguna berdasarkan kemiripan dari preferensi pengguna terhadap anime sebelumnya. Harapannya dengan memberikan rekomendasi anime yang sesuai, pengguna dapat terhibur dan puas dengan anime yang direkomendasikan sehingga dapat mendatangkan keuntungan bagi pengembang sebagai penyedia sistem.

## Bussines Understanding
### Problem statement
 - Dapatkah sistem memberikan rekomendasi berdasarkan genre anime yang  pengguna tonton?
 - Berdasarkan anime yang ditonton pengguna, bagaimana cara membuat daftar rekomendasi anime dengan metode content-based-filtering?
### Goals
- Menampilkan Top-5 rekomendasi untuk pengguna berdasarkan genre anime yang ditonton
- Mengimplementasikan dan menguji model content-based filtering untuk memberikan rekomendasi yang relevan.

## Data Understanding
Dataset yang digunakan dalam proyek ini merupakan data berdasarkan judul anime hingga jumlah penggemar serta rata-rata rating dari pengguna. Dataset ini dapat diunduh di [Anime Recommendations Database](https://www.kaggle.com/datasets/CooperUnion/anime-recommendations-database). Di link tersebut ada dua dataset namun, dalam proyek kali ini hanya memakai anime.csv saja. 
1.  Dataset anime  
    - Dataset memiliki format CSV.  
    - Dataset memiliki 12294 sample dengan 7 fitur.  
    - Dataset memiliki 1 fitur bertipe float(64), 2 fitur bertipe int64, dan 4 fitur bertipe object.  
    - Terdapat missing value pada dataset.

**Variable - Variable Pada Dataset**
-   anime_id - ID dari masing-masing judul anime di situs myanimelist.net.
-   name - Judul anime.
-   genre - list genre dari masing-masing anime . 
-   type - Tipe penayangan anime seperti movie, TV, OVA, dsb.
-   episodes - Banyak jumlah episode dalam penayangan. (1 untuk movie).
-   rating - Rata-rata rating dari pengguna.
-   members - Jumlah anggota dalam grup anime.

**Univariate Analysis**
Univariate Analysis adalah menganalisis setiap fitur secara terpisah.
![uni](https://github.com/humaidifikri/gambar_subs_dicoding/blob/main/vis_uni_rek.PNG?raw=true)
Dari hasil visualisasi diatas, kolom `type` kebanyakan anime bertipe  TV dan OVA. Jadi, data anime yang kita miliki lebih banyak bersumber dari TV sedangkan OVA adalah animasi yang nggak dirilis secara langsung dan nggak siarkan di TV maupun di bioskop. Jadi, OVA ini dirilis dalam format Blu-ray, VHS, CD, ataupun DVD, lalu nantinya dijual secara terbatas[4]. Lalu dari kolom `rating` kebanyakan anime mendapatkan rating 6 - 7 yang menandakan bahwa kurang tersebarnya anime yang menarik bagi pengguna untuk ditonton.
## Data Preparation
- Mengganti value yang `Unknown` yang terdapat pada kolom `episodes` menjadi `Ongoing`. 
Cara ini dilakukan agar developer lebih mudah membaca dan memahami. 
- Menghapus simbol pada judul anime. 
	Menghapus simbol dapat memudahkan developer untuk mengenali judul anime tersebut. fungsi `text_cleaning` bertujuan untuk membersihkan teks dari karakter atau pola tertentu yang tidak diinginkan, dan melakukan penggantian tertentu untuk membuat teks lebih konsisten dan mudah diproses. Fungsi ini menggunakan ekspresi reguler (RegEx) melalui modul `re` untuk melakukan pencarian dan penggantian teks dalam string.
-  Inisialisasi `CountVectorizer`

	    bow = CountVectorizer(stop_words="english", tokenizer=word_tokenize)
	-   `CountVectorizer`: Ini adalah kelas dari scikit-learn yang digunakan untuk mengubah kumpulan dokumen teks menjadi matriks fitur berbasis frekuensi kata (Bag of Words).
	-   `stop_words="english"`: Parameter ini memberitahu `CountVectorizer` untuk mengabaikan kata-kata umum dalam bahasa Inggris (seperti "the", "is", "in", dll.) yang tidak menambah makna signifikan dalam analisis teks.
	-   `tokenizer=word_tokenize`: `tokenizer` digunakan untuk memecah teks menjadi kata-kata (token). Di sini, kita menggunakan `word_tokenize` dari NLTK (Natural Language Toolkit), yang merupakan fungsi untuk memecah teks menjadi kata-kata berdasarkan aturan tokenisasi.


## Modeling and Result
### Content Based Filtering
Sistem yang dibangun oleh proyek ini adalah sistem rekomendasi berdasarkan genre anime berbasis content-based-filtering. Content-based filtering adalah metode yang digunakan dalam sistem rekomendasi dan analisis data yang berfokus pada karakteristik atau konten dari item-item yang ingin direkomendasikan atau dianalisis. Pendekatan ini menggunakan atribut-atribut atau fitur-fitur item untuk menentukan kesamaan antara item yang ada dan preferensi pengguna[2]. Misalkan jika pengguna menyukai anime One Piece, maka sistem akan merekomendasikan anime dengan genre yang relevan dengan anime lainnya.

**CountVectorizer**
CountVectorizer adalah sebuah alat yang digunakan dalam pemrosesan bahasa alami (Natural Language Processing, NLP) dan machine learning untuk mengubah teks menjadi representasi numerik yang dapat digunakan oleh algoritma pembelajaran mesin. Fungsi utamanya adalah untuk mengubah koleksi dokumen teks menjadi matriks hitungan fitur (feature count matrix).  CountVectorizer digunakan pada sistem rekomendasi anime untuk menentukan representasi fitur penting dari setiap genre anime. Untuk menggunakan CountVectorizer bisa dilakukan dengan cara [CountVectorizer()](https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.CountVectorizer.html) dari library sklearn.

![contoh](https://miro.medium.com/v2/resize:fit:1212/1*64-SAdEtdZW2Et8HZzqJYA.png)

**Cosine Distance**
Cosine Distance adalah ukuran jarak yang digunakan untuk menghitung seberapa mirip atau berbeda dua vektor dalam ruang vektor. Biasanya, konsep ini diterapkan pada teks dan dokumen yang direpresentasikan sebagai vektor, tetapi juga dapat digunakan dalam konteks lain di mana data dapat diwakili sebagai vektor. Cosine distance mengukur jarak antara dua vektor berdasarkan sudut kosinusnya. Semakin kecil sudut cosinus antara dua vektor, semakin besar nilai kemiripan cosinusnya.![contoh2](https://user-images.githubusercontent.com/107544829/190915969-92ac61ae-b1ac-44d9-9ec1-92f778c19602.png)
Cosine distance digunakan untuk menghitung derajat kesamaan antar genre anime. Untuk menjalankan cosine distance digunakan fungsi  [cosine_distance](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.pairwise.cosine_distances.html)  dari library sklearn.

**Content-Based Filtering**
 1. Menerapkan `CountVectorizer` pada kolom `genre`

	    bank = bow.fit_transform(dataframe[column])
-   Variabel `bank` menyimpan hasil dari `fit_transform`, yaitu sebuah matriks sparse yang mewakili representasi numerik (Bag of Words) dari teks dalam kolom `genre`.`
- `bow.fit_transform`: Fungsi ini menggabungkan dua langkah:
    -   `fit`: Mempelajari kosa kata dari teks dalam kolom `genre` pada DataFrame `dataframe`.
    -   `transform`: Mengubah teks dalam kolom `genre` menjadi matriks sparse berdasarkan frekuensi kata.
-   `dataframe[column]`: Ini merujuk pada kolom `genre` dari DataFrame `df_anime`.

2. Mengambil nilai pada baris dengan indeks `idx` dan kolom `genre`

	    content = dataframe.loc[idx, column]

-   `dataframe.loc[idx, column]`: Mengambil nilai pada kolom `genre` dari baris yang ditentukan oleh indeks `idx`. Dalam hal ini, `idx` adalah `1`, sehingga `content` akan berisi genre dari anime pada indeks `1`.

3. Transformasi konten menjadi representasi Bag-of-Words

		`code = bow.transform([content])` 

-   `bow.transform([content])`: Mengubah teks `content` menjadi representasi numerik Bag-of-Words. `content` dimasukkan sebagai list `[content]` karena `transform` mengharapkan input berupa iterable. Hasilnya adalah vektor sparse yang mewakili frekuensi kata dalam `content`.

4. Menghitung jarak kosinus antara `code` dan `bank`
			
		`dist = cosine_distances(code, bank)` 

-   `cosine_distances`: Fungsi dari scikit-learn yang menghitung jarak kosinus antara dua matriks sparse. Jarak kosinus adalah ukuran seberapa mirip dua vektor, dihitung sebagai 1 - cosine similarity.
-   `code`: Vektor sparse dari `content`.
-   `bank`: Matriks sparse yang berisi representasi BoW dari seluruh genre dalam dataset.

5. Mengurutkan jarak kosinus dan mendapatkan indeks dengan jarak terkecil

		rec_idx = dist.argsort()[0, 1:6] 

-  `dist.argsort()`: Mengurutkan indeks berdasarkan nilai jarak kosinus dari yang terkecil ke terbesar.
-   `[0, 1:6]`: Mengambil indeks dari urutan 1 hingga 5 (tidak termasuk 0 karena itu adalah dokumen itu sendiri).

6. Mencetak informasi tentang anime referensi dan genrenya

	    print(f"Anime '{dataframe['name'][idx]}' \ndengan genre {dataframe['genre'][idx]}'\nRekomendasi untuk user: ") 

-   `dataframe['name'][idx]`: Mengambil nama anime pada indeks `1`.
-   `dataframe['genre'][idx]`: Mengambil genre anime pada indeks `1`.
-   `print`: Mencetak informasi tentang anime referensi dan genrenya.

7. Mengembalikan DataFrame yang berisi nama dan genre anime yang direkomendasikan

		return df_anime.loc[rec_idx, ["name", "genre"]] 

-   `df_anime.loc[rec_idx, ["name", "genre"]]`: Mengambil baris dari DataFrame `df_anime` berdasarkan indeks yang disimpan dalam `rec_idx`, dan hanya mengembalikan kolom `name` dan `genre`.
-   **Hasil**: Mengembalikan DataFrame dengan nama dan genre anime yang direkomendasikan berdasarkan kesamaan genre.

### Result

Setelah menginisialisasi codenya sekarang kita gabung menjadi satu fungsi `Recommendation()`

    def Recommendation(idx,dataframe,column):
	    bow = CountVectorizer(stop_words="english",tokenizer=word_tokenize)
	    bank = bow.fit_transform(dataframe[column])
	    content = dataframe.loc[idx,column]
	    code = bow.transform([content])
	    dist = cosine_distances(code, bank)
	    rec_idx = dist.argsort()[0,1:6]
	    print(f"Anime '{dataframe['name'][idx]}' \ndengan genre '{dataframe['genre'][idx]}'\nRekomendasi untuk user: ")
	    return df_anime.loc[rec_idx, ["name", "genre"]]
	    
	Recommendation(1,df_anime,'genre')

Disini kita mencoba mengambil anime dari indeks ke-1. 
-   idx: Indeks anime yang akan digunakan sebagai referensi untuk rekomendasi.
-   dataframe: DataFrame yang berisi data anime.
-   column: Nama kolom yang berisi genre yang akan digunakan untuk perhitungan kesamaan.

Lalu fungsi tersebut mencetak informasi tentang anime referensi dan genrenya dan mengembalikan DataFrame yang berisi nama dan genre anime yang direkomendasikan berdasarkan kesamaan genre.

Fungsi `Recommendation()` bertujuan untuk mencari anime yang mirip berdasarkan indeks yang diinputkan. Dengan menggunakan  cosine distance, fungsi ini mampu memberikan rekomendasi anime yang paling relevan berdasarkan kesamaan genre, sehingga pengguna dapat menemukan anime baru yang sesuai dengan preferensi mereka. Berikut rekomendasi yang mirip dengan anime yang terdapat pada indeks `1`:
![res](https://github.com/humaidifikri/gambar_subs_dicoding/blob/main/result_vis_rek.PNG?raw=true)
Fungsi `Recommendation()` telah berhasil merekomendasikan top 5 anime yang mirip dengan indeks ke-1 yaitu `Fullmetal Alchemist: Brotherhood` dan seri yang serupa dari anime tersebut. Jadi, jika pengguna menyukai `Fullmetal Alchemist: Brotherhood` maka sistem dapat merekomendasikan seri lainnya atau genre yang sama.

## Evaluation
Sistem rekomendasi anime berbasis content-based filtering ini berhasil memberikan top 5 rekomendasi berdasarkan genre yang ditonton pengguna, dengan presisi sebesar 70% dari 15 rekomendasi yang diuji[3]. Hal ini menunjukkan bahwa sebagian besar rekomendasi yang dihasilkan sesuai dengan preferensi pengguna, meskipun masih ada ruang untuk perbaikan. Sistem ini berhasil menjawab problem statement dengan memberikan rekomendasi berdasarkan genre yang relevan dan membuktikan efektivitas metode content-based filtering dalam membuat daftar rekomendasi yang sesuai. Jadi, jika pengguna menyukai Fullmetal Alchemist: Brotherhood, maka sistem dapat merekomendasikan seri atau movie lainnya. Secara keseluruhan, solusi yang diterapkan telah berhasil mencapai goals yang diharapkan dan memberikan nilai tambah bagi pengalaman pengguna.
![rumus](https://dicoding-web-img.sgp1.cdn.digitaloceanspaces.com/original/academy/dos:819311f78d87da1e0fd8660171fa58e620211012160253.png)

## Referensi
[1] https://id.wikipedia.org/wiki/Anime 

[2] https://dqlab.id/content-based-filtering-dalam-algoritma-data-science

[3] https://www.dicoding.com/academies/319/discussions/134402

[4]https://www.blibli.com/friends/blog/istilah-dalam-anime-09/
