# Docker MongoDB Replica

# Docker MongoDB Replica

# PULL mongo DB latest

```bash
docker pull mongo:latest
```

# Buat directory baru untuk compose dan data

```bash
mkdir C:\Users\Mas\Documents\dev\Mongo\mongodb-replica-set
cd C:\Users\Mas\Documents\dev\Mongo\mongodb-replica-set
mkdir data\mongo1
mkdir data\mongo2
mkdir data\mongo3
```

# Buat File `docker-compose.yml`

Buat file bernama `docker-compose.yml` di dalam direktori proyek Anda dengan isi berikut:

```yaml
version: '3.8'services:  mongo1:    image: mongo:latest    container_name: mongo1    command: ["mongod", "--replSet", "my-mongo-set"]    ports:      - "27017:27017"    networks:      - mongo-cluster    volumes:      - C:\Users\Mas\Documents\dev\Mongo\mongodb-replica-set\data\mongo1:/data/db  mongo2:    image: mongo:latest    container_name: mongo2    command: ["mongod", "--replSet", "my-mongo-set"]    ports:      - "27018:27017"    networks:      - mongo-cluster    volumes:      - C:\Users\Mas\Documents\dev\Mongo\mongodb-replica-set\data\mongo2:/data/db  mongo3:    image: mongo:latest    container_name: mongo3    command: ["mongod", "--replSet", "my-mongo-set"]    ports:      - "27019:27017"    networks:      - mongo-cluster    volumes:      - C:\Users\Mas\Documents\dev\Mongo\mongodb-replica-set\data\mongo3:/data/dbnetworks:  mongo-cluster:
```

# Menjalankan Docker Compose dan Inisialisasi Replika Set

Jalankan Docker Compose untuk memulai layanan MongoDB dengan menggunakan Command Prompt atau PowerShell:

```powershell
docker-compose up -d
```

Tunggu beberapa saat agar semua kontainer berjalan dengan baik. Kemudian Anda perlu menginisialisasi replika set dengan mengakses salah satu kontainer MongoDB:

```powershell
docker exec -it mongo1 mongosh
```

Setelah masuk ke shell MongoDB, jalankan skrip inisialisasi:

```jsx
rs.initiate({
  _id: "my-mongo-set",  members: [
    { _id: 0, host: "mongo1:27017", priority: 1 },    { _id: 1, host: "mongo2:27017", priority: 0.5 },    { _id: 2, host: "mongo3:27017", priority: 0.5 },  ],});
```

### Verifikasi Konfigurasi Replika Set

Anda bisa memverifikasi konfigurasi replika set dengan menjalankan perintah berikut dalam shell MongoDB:

```jsx
rs.status()
```