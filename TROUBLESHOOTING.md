# 🔧 TROUBLESHOOTING - Octra CLI Sorun Giderme Rehberi

Bu dosya, Octra CLI kullanırken karşılaşabileceğiniz sık görülen hataları ve çözümlerini içermektedir.

## 📋 İçindekiler
- [Kurulum Sorunları](#kurulum-sorunları)
- [Cüzdan Sorunları](#cüzdan-sorunları)
- [Network/Bağlantı Sorunları](#networkbağlantı-sorunları)
- [CLI Arayüzü Sorunları](#cli-arayüzü-sorunları)
- [İşlem Sorunları](#işlem-sorunları)
- [Genel Çözümler](#genel-çözümler)

---

## 🚀 Kurulum Sorunları

### ❌ **Sorun**: `wget: command not found`
**Neden**: wget paketi yüklü değil  
**Çözüm**:
```bash
# Ubuntu/Debian için:
sudo apt update && sudo apt install wget -y

# CentOS/RHEL için:
sudo yum install wget -y
```

### ❌ **Sorun**: `chmod: Operation not permitted`
**Neden**: Dosya izinleri veya NTFS dosya sistemi  
**Çözüm**:
```bash
# WSL için:
sudo chmod +x octrascript.sh

# Alternatif çözüm:
bash octrascript.sh
```

### ❌ **Sorun**: Script indirme hatası "SSL certificate problem"
**Neden**: SSL sertifika doğrulama sorunu  
**Çözüm**:
```bash
# Geçici çözüm (güvenlik riski):
wget --no-check-certificate https://raw.githubusercontent.com/galipgokalp/Octra-CLI/refs/heads/main/octrascript.sh

# Daha güvenli çözüm:
sudo apt update && sudo apt install ca-certificates -y
```

### ❌ **Sorun**: "No such file or directory" hatası
**Neden**: Dosya yolu veya çalışma dizini hatası  
**Çözüm**:
```bash
# Mevcut dizini kontrol et:
pwd
ls -la

# Doğru dizine git ve tekrar dene:
cd ~ && ./octrascript.sh
```

---

## 💳 Cüzdan Sorunları

### ❌ **Sorun**: Cüzdan oluşturulmuyor veya sayfa açılmıyor
**Neden**: Port 8888 kullanımda veya güvenlik duvarı engeli  
**Çözüm**:
```bash
# Port kontrol:
sudo netstat -tulpn | grep :8888

# Port'u öldür:
sudo fuser -k 8888/tcp

# Güvenlik duvarı (varsa):
sudo ufw allow 8888
```

### ❌ **Sorun**: "Connection refused" hatası
**Neden**: Servis başlamamış veya farklı IP/port  
**Çözüm**:
- WSL: `http://localhost:8888` veya `http://127.0.0.1:8888`
- VPS: `http://SUNUCU_IP:8888`
- Script menüsünden "1" seçeneğini tekrar çalıştır

### ❌ **Sorun**: Cüzdan bilgileri kayboldu
**Neden**: Wallet dosyası silinmiş veya bozulmuş  
**Çözüm**:
```bash
# Yedek kontrol:
ls -la octra_pre_client/

# Yeni cüzdan oluştur:
rm -rf octra_pre_client/wallet.json
./octrascript.sh
# Menüden 1'i seç ve yeni cüzdan oluştur
```

---

## 🌐 Network/Bağlantı Sorunları

### ❌ **Sorun**: Faucet sitesi açılmıyor
**Neden**: DNS problemi veya site geçici kapalı  
**Çözüm**:
```bash
# DNS değiştir:
echo "nameserver 8.8.8.8" | sudo tee /etc/resolv.conf

# Alternatif faucet linkleri dene:
# https://faucet.octra.network/
# (Topluluk kanallarından güncel link al)
```

### ❌ **Sorun**: "Network timeout" veya bağlantı kesilmesi
**Neden**: İnternet bağlantısı veya VPN sorunu  
**Çözüm**:
- İnternet bağlantısını kontrol et
- VPN kullanıyorsan kapat/değiştir
- Proxy ayarlarını kontrol et
- Biraz bekleyip tekrar dene

### ❌ **Sorun**: İşlemler onaylanmıyor
**Neden**: Network congestion veya düşük gas fee  
**Çözüm**:
- 10-30 saniye bekle
- İşlemi iptal edip tekrar dene
- Farklı bir zamanda tekrar dene

---

## 🖥️ CLI Arayüzü Sorunları

### ❌ **Sorun**: CLI arayüzü bozuk görünüyor veya karakterler karışık
**Neden**: Terminal encoding veya terminal boyutu  
**Çözüm**:
```bash
# Terminal boyutunu ayarla:
resize

# CLI'da görüntüyü yenile:
# CLI arayüzünde "2" yaz ve ENTER

# Alternatif terminal dene:
# Windows Terminal, WSL2 veya farklı bir terminal emulator kullan
```

### ❌ **Sorun**: Klavye girdileri çalışmıyor
**Neden**: Terminal focus sorunu  
**Çözüm**:
- Terminal penceresine tıkla
- `CTRL+C` ile çık, tekrar başlat
- Script'i yeniden çalıştır: `./octrascript.sh`

### ❌ **Sorun**: CLI menü seçenekleri görünmüyor
**Neden**: Terminal boyutu veya renk ayarları  
**Çözüm**:
```bash
# Terminal boyutunu büyült
# Renk desteğini kontrol et:
echo $TERM

# Clear screen:
clear

# CLI'ı yeniden başlat:
# Ana menüden 4'ü seç
```

---

## 💸 İşlem Sorunları

### ❌ **Sorun**: "Insufficient funds" hatası
**Neden**: Yetersiz $OCT bakiyesi  
**Çözüm**:
- Faucet'ten token al: https://faucet.octra.network/
- Bakiyeni kontrol et (CLI menüsünde)
- 24 saat bekleyip tekrar faucet'e git

### ❌ **Sorun**: Transfer işlemi başarısız
**Neden**: Geçersiz adres veya network hatası  
**Çözüm**:
- Alıcı adresini kontrol et (`oct...` ile başlamalı)
- Miktarı kontrol et (bakiyenden az olmalı)
- Network bağlantısını kontrol et

### ❌ **Sorun**: "Encrypt Balance" işlemi takılıyor
**Neden**: Uzun işlem süresi (normal durum)  
**Çözüm**:
- 5-10 dakika sabırlı bekle
- İşlem sırasında başka işlem yapma
- Eğer 15 dakika geçerse `CTRL+C` ile iptal et ve tekrar dene

### ❌ **Sorun**: Private Transfer çalışmıyor
**Neden**: Önce encrypt balance yapılması gerek  
**Çözüm**:
1. CLI menüsünde önce "4" (Encrypt Balance)
2. Encrypt işlemi bittikten sonra "6" (Private Transfer)

---

## ⚡ Genel Çözümler

### 🔄 **Tamamen Yeniden Başlat**
```bash
# CLI'dan çık:
CTRL+C

# Process'leri öldür:
sudo pkill -f octra

# Temiz başlangıç:
./octrascript.sh
```

### 🧹 **Temizlik Komutları**
```bash
# Geçici dosyaları temizle:
rm -rf /tmp/octra*

# Log dosyalarını kontrol et:
ls -la ~/.octra/

# Cache temizle:
rm -rf ~/.cache/octra*
```

### 🔍 **Debug/Log Kontrol**
```bash
# Script debug modda çalıştır:
bash -x octrascript.sh

# Network bağlantısı test:
ping google.com
curl -I https://faucet.octra.network/

# Sistem kaynaklarını kontrol et:
free -h
df -h
```

### 💾 **Yedekleme**
```bash
# Önemli dosyaları yedekle:
cp -r octra_pre_client/ ~/octra_backup/
cp octrascript.sh ~/octra_backup/

# Wallet bilgilerini güvenli bir yerde sakla!
```

---

## 🆘 Acil Durum Çözümleri

### 🚨 **Sistem Tamamen Dondu**
1. `CTRL+C` ile dur
2. `sudo reboot` (VPS için)
3. WSL yeniden başlat: `wsl --shutdown`
4. Script'i yeniden indir ve çalıştır

### 🚨 **Cüzdan Erişim Sorunu**
1. Private key'ini sakla (çok önemli!)
2. `rm -rf octra_pre_client/`
3. Script'i yeniden çalıştır
4. Existing wallet seçeneğiyle private key'i gir

### 🚨 **Faucet Token Alamıyor**
- VPN kullanıp farklı IP dene
- 24 saat bekle
- Topluluk kanallarından yardım iste
- Farklı tarayıcı dene

---

## 📞 Yardım Alın

Sorunun devam ediyorsa:

1. **Hata mesajının screenshot'ını al**
2. **Hangi adımda takıldığını belirt**
3. **İşletim sistemini söyle (WSL/VPS/Ubuntu vs.)**
4. **Topluluk kanallarına paylaş**

### 🔗 Yararlı Linkler
- [Ana README](README.md)
- [Octra Official Faucet](https://faucet.octra.network/)
- [WSL Kurulum Rehberi](https://x.com/itemcoin/status/1974972114072723531)

---

## 📝 Notlar

- Bu rehber sürekli güncellenmektedir
- Yeni hatalarla karşılaştığında bu dosyayı güncelleyebilirsin
- Çözüm bulduğun hataları toplulukla paylaş

**💡 İpucu**: Çoğu sorun, script'i tamamen kapatıp yeniden başlatmakla çözülür!

---

**⭐ Bu rehber faydalı olduysa repo'yu star'lamayı unutma!**
