# Setup Monitoring Server

## List Aplikasi yang di-Install untuk monitoring
1. Node Exporter
2. Prometheus
3. Grafana

### Install Node Exporter
1. Download terlebih dahuluh node exporternya. untuk versi terbaru dapat dilihat di githubnya node_exporter 
```
curl -LO https://github.com/prometheus/node_exporter/releases/download/v1.3.1/node_exporter-1.3.1.linux-amd64.tar.gz
```
2. kemudian ekstrak file yang telah didownload 
```
tar -xvf node_exporter-1.3.1.linux-amd64.tar.gz
```
3. Pindahkan file node exporter kedalam usr/local/bin/
```
sudo mv node_exporter-1.3.1.linux-amd64/node_exporter /usr/local/bin/
sudo useradd -rs /bin/false node_exporter
```
4.  Buat file dalam systemd node exporter
```
sudo nano /etc/systemd/system/node_exporter.service
```
isi filenya
```
[Unit]
Description=Node Exporter
After=network.target

[Service]
User=node_exporter
Group=node_exporter
Type=simple
ExecStart=/usr/local/bin/node_exporter

[Install]
WantedBy=multi-user.target
```
5. Kemudian reload daemon
```
sudo systemctl daemon-reload
```
6. Jalankan node exporter dan buat enable node_exporter
```
sudo systemctl start node_exporter
sudo systemctl enable node_exporter
```
7. dan cek apakah node_exporter berjalan dengan
```
sudo systemctl enable node_exporter
```
8. Buka web browser dan akses di `http://<alamatIP>:9001`

### Install Prometheus
1. Download prometheus terlebih dahulu. untuk versi lebih lengkapnya bisa dilihat diwebsite prometheus.io
```
wget https://github.com/prometheus/prometheus/releases/download/v2.32.1/prometheus-2.32.1.linux-amd64.tar.gz
```
2. Kemudia ekstrak prometheusnya
```
tar xvf prometheus-2.32.1.linux-amd64.tar.gz
```
3. buat file di etc/
```
sudo mkdir /etc/prometheus
sudo mkdir /var/lib/prometheus
```
4. copy file-file ke dalam folder etc/prometheus
```
sudo cp prometheus-2.32.1.linux-amd64/prometheus /usr/local/bin/
sudo cp prometheus-2.32.1.linux-amd64/promtool /usr
/local/bin/
sudo cp -r prometheus-2.32.1.linux-amd64/consoles /etc/prometheus
sudo cp -r prometheus-2.32.1.linux-amd64/console_li
braries /etc/prometheus
```
5. Buat konfigurasi prometheus

- prometheus.io
```
sudo nano /etc/systemd/system/prometheus.service
```
isi file prometheus.service
```
[Unit]
Description=Prometheus
Wants=network-online.target
After=network-online.target

[Service]
User=prometheus
Group=prometheus
Type=simple
ExecStart=/usr/local/bin/prometheus \
    --config.file /etc/prometheus/prometheus.yml \
    --storage.tsdb.path /var/lib/prometheus/ \
    --web.console.templates=/etc/prometheus/consoles \
    --web.console.libraries=/etc/prometheus/console_libraries

[Install]
WantedBy=multi-user.target
```

- prometheus.yml
```
sudo nano /etc/prometheus/prometheus.yml
```
isi dari file prometheus.yml
```
global:
  scrape_interval: 10s

scrape_configs:
  - job_name: 'prometheus'
    scrape_interval: 5s
    static_configs:
      - targets: ['localhost:9090', <alamat IP server yang dipantau>]
```

6. Buat user prometheus
```
sudo useradd -rs /bin/false prometheus
```

7. dan ubah semua konfigurasi agar bisa diakses oleh prometheus
```
sudo chown prometheus:prometheus /etc/prometheus
sudo chown prometheus:prometheus /var/lib/prometheus
```
### Menghubungkan server lain ke server monitoring
1. Install node_exporter di server yang akan dimonitoring
2. Jika sudah terinstall tambahkan alamat server kedalam file prometheus.yml
3. kemudian restart aplikasi prometheus
dan buka web browser, pilih targets dan dapat dilihat apakah sudah terkoneksi atau belum

### Install Grafana