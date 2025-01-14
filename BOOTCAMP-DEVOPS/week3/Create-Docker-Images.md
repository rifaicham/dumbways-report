# Step by step build images and push to hub.docker.com

1. clone aplikasi yang akan dibuatkan images-nya. 
- `git clone https://github.com/sgnd/dumbflix-frontend`
- `git clone https://github.com/sgnd/dumbflix-backend`
<p align="center">
    <img src="assets\gitclone.jpg" />
</p>

2. Masuk kedalam direktori aplikasi dumbflix yang sudah didownload. Kemudian buat file Dockerfile
```
cd dumbflix-frontend
nano Dockerfile
```
Jika sudah maka isikan sesuai format dari docker dan bagaimana program berjalan.
```
# menggunakan node v10
FROM node:10 
# tempat dimana aplikasi akan disimpan
WORKDIR /app 
COPY . .
# Perintah untuk menjalankan program
RUN npm install
# menjalankan aplikasi diport 3000
EXPOSE 3000
CMD ["npm", "start"]
```
3. Jika sudah jalan perintah `docker build -t rifaicham/dumbflix-frontend .` Maka proses akan berjalan
<p align="center">
    <img src="assets\dockerbuildfrontend.jpg" />
</p>

4. Proses sudah selesai, kemudian jalankan perintah `docker images ` untuk melihat apakah images berhasil dibuat. Jika terdapat dalam list, maka images berhasil dibuat. 
<p align="center">
    <img src="assets\dockerimages.jpg" />
</p>

5. Untuk mengunggahnya ke docker hub, dapat dengan perintah
```
docker push <namaimages>:version
```
<p align="center">
    <img src="assets\dockerpush.jpg" />
</p>

6. Jika proses upload sudah berhasil, cek di docker hub apakah sudah teruupload.
<p align="center">
    <img src="assets\dockerpushproof.jpg" />
</p>


7. Proses docker push untuk aplikasi dumbflix-backend sama dengan aplikasi frontend
- menuju direktori backend `cd dumbflix-backend`
- buat file Dockerfile `nano Dockerfile`
- kemudian isi dockerfile sesuai dengan format
```
# menggunakan node v10
FROM node:10 
# tempat dimana aplikasi akan disimpan
WORKDIR /app 
# copy semua file
COPY . .
# Perintah untuk menjalankan program
RUN npm install
# menjalankan aplikasi diport 5000
EXPOSE 5000
CMD ["npm", "start"]
```
- Kemudian `docker build -t rifaicham/dumbflix-backend .`
- jika proses sudah selesai jalankan perintah `docker images`
- Kemudian push aplikasi dengan perintah `docker push rifaicham/dumbflix-backend .`
<p align="center">
    <img src="assets\dockerimages2.jpg" />
</p>
<p align="center">
    <img src="assets\dockerpush2.jpg" />
</p>
<p align="center">
    <img src="assets\dockerpushproof2.jpg" />
</p>