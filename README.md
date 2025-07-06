
# ☁️ Hazelcast on AWS EC2 with Docker

Bu proje, **Hazelcast** ve **Hazelcast Management Center**’ın AWS EC2 bulut sunucusu üzerinde **Docker** kullanılarak nasıl kurulduğunu adım adım göstermektedir.

---

## 🚀 Amaç

- AWS EC2 üzerinde Hazelcast node'u çalıştırmak  
- Hazelcast Management Center ile cluster durumunu web arayüzünden izlemek  
- Local yerine tamamen cloud ortamında kurulum yapmak  

---

## 🛠️ Kullanılan Teknolojiler

- **AWS EC2** (Ubuntu 24.04 LTS)  
- **Docker**  
- **Hazelcast**  
  👉 [https://hub.docker.com/r/hazelcast/hazelcast](https://hub.docker.com/r/hazelcast/hazelcast)  
- **Hazelcast Management Center**  
  👉 [https://hub.docker.com/r/hazelcast/management-center](https://hub.docker.com/r/hazelcast/management-center)

---

## ⚙️ Kurulum Adımları

### 1️⃣ EC2 Instance Oluştur

- AWS Console > EC2 > Launch Instance
- AMI: **Ubuntu Server 24.04 LTS**
- Instance type: `t3.micro` (ücretsiz katman)
- Security Group Ayarları:
  - ✅ **SSH (port 22)** → Kendi IP’niz
  - ✅ **TCP 8080** → `0.0.0.0/0` (Management Center erişimi için)
  - ✅ **TCP 5701** → `0.0.0.0/0` (Hazelcast portu)
- Key pair oluşturun ve `.pem` dosyasını indirin.

---

### 2️⃣ SSH ile Sunucuya Bağlan

```bash
ssh -i "path/to/my-aws.pem" ubuntu@<EC2_PUBLIC_IP>
````

---

### 3️⃣ Docker Kurulumu

```bash
sudo apt update
sudo apt install -y ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
  sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
  https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

---

### 4️⃣ Docker Container’ları Başlat

#### 🔹 Hazelcast

```bash
sudo docker run -d --name hazelcast hazelcast/hazelcast:latest

```

![Ekran görüntüsü 2025-07-06 155308](https://github.com/user-attachments/assets/e2e7d899-7a2b-4e04-8232-11548bdc45e2)


```

#### 🔹 Management Center

```bash
sudo docker run -d --name man-center -p 8080:8080 hazelcast/management-center:latest
```

![Ekran görüntüsü 2025-07-06 155410](https://github.com/user-attachments/assets/36134f35-1e20-4bde-8516-429683df95d8)

---

### 5️⃣ Management Center’a Web Arayüzü ile Erişim

Tarayıcınızdan aşağıdaki URL’ye gidin:

```
http://<EC2_PUBLIC_IP>:8080
```

#### Örnek:

```
http://IP:8080
```

> Açılan arayüzde Hazelcast node'unuzu görebilir, cluster yapılandırmasını ve durumunu görüntüleyebilirsiniz.
![Ekran görüntüsü 2025-07-06 155759](https://github.com/user-attachments/assets/9c6cddf1-8ed8-4995-abb0-9b8ae3b4a661)


---

## ✅ Sonuç

Bu kurulumla birlikte AWS EC2 üzerinde çalışan Hazelcast cluster’ınızı merkezi bir arayüzle yönetebilir ve izleyebilirsiniz. Proje tamamen cloud tabanlı olup, local bağımlılığı ortadan kaldırmaktadır.



