# VPS'lere Extention Kurulumu ; 

## Gereksinimler : 

Minimum : 

- 1 CPU+ / 2 GB RAM /  4 MBit/sec İndirme Hızı

Tavsiye Edilen : 

- 2+ CPU ( Hızlı ) / 4+ GB RAM / 8+ MBit/sec İndirme Hızı 

#### Sistem paketlerini güncellemek ve yükseltmek için aşağıdaki komutu çalıştırın:

```bash
sudo apt update -y && sudo apt upgrade -y
```
## 2. Gerekli paketleri kurun:

```bash
sudo apt install htop ca-certificates zlib1g-dev libncurses5-dev libgdbm-dev libnss3-dev tmux iptables curl nvme-cli git wget make jq libleveldb-dev build-essential pkg-config ncdu tar clang bsdmainutils lsb-release libssl-dev libreadline-dev libffi-dev jq gcc screen unzip lz4 -y
```
## 3. Docker'ı Kur : 

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io -y
docker version
```

## 4. Docker Compose'u Kur : 

```bash
VER=$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep tag_name | cut -d '"' -f 4)
curl -L "https://github.com/docker/compose/releases/download/$VER/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
docker-compose --version
```

## 4. Docker Kullanıcı İzinleri

```bash
sudo groupadd docker
sudo usermod -aG docker $USER
```

## Zaman Dilimini Öğrenelim ; 

```bash
realpath --relative-to /usr/share/zoneinfo /etc/localtime
```

- Buradaki Zaman Dilimini Kaydediyoruz.

## Chromium İndirelim ; 

```bash
mkdir chromium
cd chromium
```

## Docker Compose Dosyası Oluşturalım ; 

```bash
nano docker-compose.yaml
```

## Değiştireceğimiz Yerler ; 

- User
- Password
- TZ

```bash
---
services:
  chromium:
    image: lscr.io/linuxserver/chromium:latest
    container_name: chromium
    security_opt:
      - seccomp:unconfined #optional
    environment:
      - CUSTOM_USER=burayakullaniciadiyaz
      - PASSWORD=burayakullanacainsifreyiyaz
      - PUID=1000
      - PGID=1000
      - TZ=Europe/London
      - CHROME_CLI=https://github.com/FurkanL0 #optional
    volumes:
      - /root/chromium/config:/config
    ports:
      - 3010:3000   #Change 3010 to your favorite port if needed
      - 3011:3001   #Change 3011 to your favorite port if needed
    shm_size: "1gb"
    restart: unless-stopped
```

- Örnek ; 

![resim](https://github.com/user-attachments/assets/1f488a20-8c0b-4d54-9b3c-f011cf94c343)


## CTRL X CTRL Y Enter yapıp kaydedin.

## Sunucuyu Çalıştıralım ; 
```bash
cd $HOME && cd chromium
```
```bash
docker compose up -d
```

Sunucumuza Erişelim ; 

```bash
http://Sunucu-ip:3010/
https://Sunucu-ip:3011/
```

- Ayarladığınız Kullanıcı adı - Şifreyi girin.

![resim](https://github.com/user-attachments/assets/88e6b139-b364-4c42-bd5f-653547b29bc5)

- Sonrasında artık Sanki Windows servera bağlanmış gibi chromium üzerinden eklentileri indirip giriş yapabilirsiniz.

![resim](https://github.com/user-attachments/assets/84930d45-62e6-484c-8465-880c35a9228b)

