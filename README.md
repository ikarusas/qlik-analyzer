# DQ Türkiye — Qlik Proje Analizörü

Qlik Sense on-prem projeleri için otomatik durum analizi aracı. QRS API'ye bağlanarak app envanteri, QVD mimarisi ve AI destekli danışman brief'i üretir.

## 🚀 GitHub Pages'e Deploy

### 1. Repo oluştur
```bash
# GitHub'da yeni repo aç: qlik-analyzer (public)
git clone https://github.com/KULLANICI_ADIN/qlik-analyzer.git
cd qlik-analyzer
```

### 2. Dosyaları kopyala
```bash
cp index.html qlik-analyzer/
cd qlik-analyzer
git add index.html
git commit -m "feat: Qlik Proje Analizörü v1.0"
git push origin main
```

### 3. GitHub Pages aç
- Repo → **Settings** → **Pages**
- Source: **Deploy from a branch**
- Branch: `main` / `/ (root)`
- **Save**

5 dakika sonra `https://KULLANICI_ADIN.github.io/qlik-analyzer` adresinde yayında.

---

## ⚙️ Kullanım

### Gerçek QRS API
1. Qlik Sense sunucunuzda CORS'u açın (aşağıya bakın)
2. Sunucu adresi ve kullanıcı bilgilerini girin
3. Anthropic API key'i girin ([console.anthropic.com](https://console.anthropic.com))
4. **Analizi Başlat**'a tıklayın

### Demo Mod
API key veya QRS bağlantısı olmadan: **Demo veriyle test et** butonunu kullanın.

---

## 🔧 QRS API — CORS Konfigürasyonu

Qlik Sense on-prem'de tarayıcıdan QRS API'ye erişim için CORS ayarı gerekir.

### QMC üzerinden
1. QMC → **Virtual Proxies** → proxy seçin → **Edit**
2. **Additional response headers** alanına ekleyin:
```
Access-Control-Allow-Origin: https://KULLANICI_ADIN.github.io
Access-Control-Allow-Headers: X-Qlik-Xrfkey, X-Qlik-User, Content-Type
Access-Control-Allow-Methods: GET, POST, OPTIONS
```
3. **Apply**

### Alternatif: Local Proxy
CORS kısıtlaması varsa basit bir Node.js proxy kullanabilirsiniz:
```bash
npx cors-anywhere
# Sonra arayüzde sunucu adresini http://localhost:8080/https://qlik-server.com olarak girin
```

---

## 📦 Özellikler

| Özellik | Açıklama |
|---------|----------|
| **QRS API** | Stream, app listesi, son yükleme, boyut |
| **Sağlık skorlaması** | 30+ gün = Sorunlu, 7-30 gün = Uyarı |
| **Claude AI analizi** | Danışman handover brief (Anthropic API) |
| **Word raporu** | McKinsey tarzı .docx, tablo + metrikler |
| **Excel envanteri** | 3 sheet: App listesi, QVD, Özet |
| **Danışman brief** | .txt, Claude'a yapıştırılabilir context |

---

## 🔑 API Key Güvenliği

- Anthropic API key **tarayıcıda tutulur**, sunucuya gönderilmez
- Her oturumda tekrar girmeniz gerekir (localStorage kullanılmaz)
- Production kullanım için API key'i backend proxy'ye taşıyın

---

## 🏗️ Teknik Yapı

```
index.html          ← tek dosya, zero-dependency
├── QRS API         ← fetch() ile doğrudan bağlantı
├── Claude AI       ← api.anthropic.com/v1/messages
├── docx (CDN)      ← Word üretimi (cdnjs)
└── xlsx (CDN)      ← Excel üretimi (cdnjs)
```

---

## 📄 Lisans

DQ Türkiye dahili kullanım. © 2026 DQ Türkiye.
