# Cyberbullying-Classfication

# Domain Proyek
## Latar Belakang 
Dengan maraknya media sosial ditambah dengan pandemi Covid-19, *cyberbullying* telah mencapai titik tertinggi sepanjang masa. Kita dapat memerangi ini dengan membuat model untuk secara otomatis menandai tweet yang berpotensi berbahaya serta memecah pola kebencian.

Pada tanggal 15 April 2020, UNICEF mengeluarkan peringatan sebagai tanggapan atas peningkatan risiko *cyberbullying* selama pandemi COVID-19 karena sekolah-sekolah tutup, peningkatan aktivitas depan layar komputer, dan penurunan interaksi sosial tatap muka. Statistik *cyberbullying* benar-benar mengkhawatirkan, sebanyak 36,5% siswa sekolah menengah dan menengah telah merasakan *cyberbullying* dan 87% telah mengamati *cyberbullying*, dengan efek mulai dari penurunan kinerja akademik hingga depresi hingga pikiran untuk bunuh diri.

# Business Understanding

## Problem Statement 
Berdasarkan kondisi yang telah diuraikan sebelumnya, perusahaan akan mengembangkan sebuah sistem klasifikasi tipe *cyberbullying* untuk menjawab permasalahan berikut.

 - Dari permasalahan diatas tweet apa saja yang bisa dikategorikan sebagai cyberbullying
 - Apa saja kategori dari cyberbullying berdasarkan tweet tersebut

## Goals

 - Mengetahui tweet mana yang merupakan cyberbullying dan tweet apa yang bukan
 - Mengklasfikasikan jenis cyberbullying berdasarkan tweet apabila tweet tersebut merupakan cyberbullying
 - 
## Metrik
Untuk kasus klasifikasi jenis cyberbullying dari tweet yang merupakan text, berarti digunakan NLP dalam pembuatan modelnya. Model yang digunakan adalah sequential dengan layer Embedding, LSTM, dan Dense beroutput 6 karena banyak label adalah 6. Metrik yang akan digunakan adalah Accuracy, Val_Accuracy, Loss dan Val_Loss.

# Data Understanding
## Dataset Information
Dataset yang digunakan adalah dataset Cyberbullying Classification yang di upload oleh andrewmvd.
https://www.kaggle.com/datasets/andrewmvd/cyberbullying-classification

Memiliki 2 kolom yaitu tweet_text dan cyberbullying_type.
Dataset ini berisi lebih dari 47000 tweet yang diberi label sesuai dengan kelas cyberbullying:

Age;
Ethnicity;
Gender;
Religion;
Other Cyberbullying;
Not Cyberbullying;
Data telah diseimbangkan untuk menampung ~8000 dari setiap kelas.

## Data Loading
Dengan menggunakan google collab dan API dari kaggle saya mengunduh file dataset tersebut langsung dari google collab ke kaggle.

## Explarotary Data Analysis

### Variable Description
Dataset memiliki feature tweet_text dan label cyberbullying_type.
 - tweet_text 					= Isi text dari tweet
 - cyberbullying_type 	= Tipe - tipe cyberbullying

### Data Condition
Isi dari tweet_text merupakan text yang masih belum rapi, masih terdiri dari campuran huruf besar dan kecil, stopwords, dan special character.

Kolom tweet_text memiliki 47692 baris data, dan kolom cyberbullying_type memiliki 47692 baris data juga.

# Data Preparation
## One Hot Encoding
Karena isi dari cyberbullying_type merupakan baris data text dan ini juga merupakan tipe multiclass text classification maka kolom cyberbullying_type harus di one hot encoding terlebih dahulu. Menggunakan fungsi get_dummies yang berparameter kolom tersebut.

## Merubah semua text ke huruf kecil
Dengan adanya kombinasi huruf kecil dan besar maka komputer akan sulit untuk mempelajari kalimat, maka dari itu perlu adanya normalisasi text dari kombinasi huruf kecil dan besar menjadi huruf kecil saja. Menggunakan str.lower() dengan parameter kolom tweet_text

## Menghapus special character
Adanya special character akan mempersulit komputer untuk mempelajari kalimat, sama seperti normalisasi huruf besar dan kecil, sepcial character harus dinormalisasi sehingga hilang dari semua isi text, dengan menggunakan fungsi str.replace() berparameter semua special character tersebut maka kita dapat menghapus special character.

## Menghapus Stopwords
Stopword merupakan kata yang diabaikan dalam pemrosesan dan biasanya disimpan di dalam stop lists. 
Dengan menggunakan stop list yang sudah disediakan NLTK maka kita dapat menggunakan stop list tersebut untuk menghapus stopwords yang ada di text tweet_text.

## Data Splitting
Data yang sudah dinormalisasi seperti diatas akan dipisah menjadi data train dan data train, mengunakan library train_test_split kita dapat memisahkan atribut dan label menjadi data train dan data test. 
Rasio test adata adalah 20% dari total data.

## Tokenisasi dan Sekuens
Tokenisasi diperlukan untuk mengonversi kata-kata ke bilangan numerik. Isi dari tweet_text tersebut lah yang akan di tokenisasi karena tweet_text berisi banyak kumpulan text yang akan diproses oleh model nantinya.
Setiap text yang sudah di tokenisasi maka dilanjutkan dengan mengonversi menjadi sequence.

# Modeling
Model yang digunakan adalah model sequential karena dengan model sequential kita dapat menggunakan layer seperti embedding dan lstm.
Berikut adalah layer beserta aktivasi yang digunakan

Embedding(input_dim=5000, output_dim=16),    (memiliki dimensi input 5000, dan output dimensi 16)
LSTM(64),																	(memiliki 64 node)
Dense(512, activation='relu'),									(memiliki 512 node, dan activation relu)
Dense(256, activation='relu'),
.Dense(128, activation='relu'),
Dense(64, activation='relu'),
Dense(6, activation='softmax'),								(memiliki 6 node, dan activation softmax karena model memprediksi multiclass classification)

## Embedding

Setelah isi tweet_text tadi sudah di tokenisasi maka lanjut ke tahap embedding.

Embedding diperlukan karena embedding merupakan kunci dalam klasifikasi teks di Tensorflow. Embedding memungkinkan model ML untuk memahami makna di setiap kata dan mengelompokkan kata-kata dengan makna yang mirip agar berdekatan.

Embedding digunakan dengan memanggil fungsi embedding pada model sequential berparameter input_dim=5000, dan output_dim=16.

## LSTM 
LSTM adalah Long Short-Term Memory yang digunakan agar komputer dapat memahami kata sesuai dengan kalimat yang digunakan.
LSTM adalah teknik yang umum digunakan dalam pemrosesan bahasa alami yang memungkinkan agar model dapat memahami makna sebuah kalimat berdasarkan urutan kata

LSTM digunakan dengan memaggil fungsi LSTM setelah layer Embedding dengan parameter sebanyak 64.

## Compile Model
Model di compile dengan parameter loss = categorical_crossentropy karena tipe klasifikasi adalah categorical, optimizer = adam, dan metrics = accuracy.

## Training Model
Setelah model di define lalu tahap selanjutnya adalah training model menggunakan data yang sudah disiapkan tadi.
#### Callback
Sebelum data di train diperlukan membuat callback, callback yang digunakan adalah EarlyStopping yang berguna untuk memonitor metrik apakah berkembang atau tidak.
Parameter :
 - patience 	= 1					(ketika tidak ada perkembangan terhadap metrik yang dimonitor sebanyak 1 kali maka training berhenti)
 - monitor 	= "val_loss"		(memonitor metrik val_loss)
 
 Callback dengan parameter tersebut disimpan dalam variable my_callback

## Fitting Model
Model yang sudah di define akan dimasukkan data yang sudah disiapkan tadi, menggunakan epoch sebanyak 10 karena data yang kita punya sudah banyak maka 10 epoch sudah cukup.
Parameter :
 - x = padded_latih												(text yang sudah di padding)
 - y = label_latih														(label dari kolom cyberbullying_type)
 - epochs = 10
 - validation data = padded_test, label_test 		(data yang akan digunakan untuk validasi) 
 - verbose =2
 - callback = my_callback

## Evaluation
Metrik yang digunakan antara lain
 - Accuracy  = Berapa akurat data training yang dilatih
 - Val_accuracy = Berapa akurat data test yang dilatih
 - Loss = Persentase kesalahan prediksi terhadap data latih
 - Val_loss = Persentase kesalahan prediksi terhadap data train

Hasil dari training model tersebut adalah sebagai berikut
 - Accuracy : 0.84
 - Val_accuracy : 0.82
 - Loss : 0.37
 - Val_loss : 0.42

![image](https://user-images.githubusercontent.com/110660648/191799776-668c9a1f-9138-4304-a904-cc66ac04933b.png)

![image](https://user-images.githubusercontent.com/110660648/191799809-d1a92450-33fa-4231-8fc3-46ac82a20eae.png)
