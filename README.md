# DQ Türkiye — Qlik Proje Analizörü

Qlik Sense projelerini otomatik olarak analiz eden, **On-Premises** ve **Qlik Cloud** ortamlarını destekleyen tek dosyalık web uygulaması. App envanteri, veri mimarisi ve AI destekli danışman brief'i üretir.

---

## 🚀 Kurulum

### IIS ile Yayına Alma (Önerilen)

Uygulamayı kendi sunucunuzda IIS üzerinden yayınlayarak müşterilerin kurumsal ağlarından erişimini kolaylaştırabilirsiniz.

**1. IIS'te yeni site oluşturun**

- IIS Manager → **Sites** → **Add Website**
- Site name: `qlik-analyzer`
- Physical path: `C:\inetpub\wwwroot\qlik-analyzer`
- Port: `80` (veya SSL için `443`)
- **OK**

**2. Dosyayı kopyalayın**

```
C:\inetpub\wwwroot\qlik-analyzer\
└── index.html
```

**3. SSL (Önerilen)**

Sertifikanızı IIS'e bağlayın ve `https://analiz.dqturkiye.com` gibi bir adresle yayına alın. Müşteri IT ekipleri `github.io` yerine bu domaini açar.

**4. Güncelleme**

Tek dosya olduğu için güncelleme basittir — yeni `index.html`'i klasöre yapıştırmanız yeterlidir.

---

### GitHub Pages ile Yayına Alma

```bash
# Repo oluştur ve dosyayı yükle
git clone https://github.com/KULLANICI_ADIN/qlik-analyzer.git
cd qlik-analyzer
cp index.html .
git add index.html
git commit -m "feat: Qlik Proje Analizörü"
git push origin main
```

- Repo → **Settings** → **Pages** → Branch: `main` / `/(root)` → **Save**
- 5 dakika sonra `https://KULLANICI_ADIN.github.io/qlik-analyzer` adresinde yayında

> ⚠️ Kurumsal ağlarda `github.io` domaini firewall tarafından engellenebilir. Bu durumda IIS veya Vercel tercih edilmelidir.

---

### Vercel ile Yayına Alma

1. [vercel.com](https://vercel.com)'a girin, GitHub ile bağlayın
2. `qlik-analyzer` reposunu seçip deploy edin
3. İsteğe bağlı olarak kendi domaininizi bağlayın

---

## ⚙️ Kullanım

### Bağlantı Türü Seçimi

Uygulamada sidebar'da **On-Premises** ve **Qlik Cloud** arasında geçiş yapabilirsiniz.

---

### 🖥 On-Premises (QRS API)

| Alan | Açıklama |
|---|---|
| Sunucu adresi | `https://qlik.sirket.com` |
| Port | `4242` (varsayılan) |
| Kullanıcı | `DOMAIN\qlik_service` formatında |

**CORS Konfigürasyonu**

Tarayıcıdan QRS API'ye erişim için Qlik Sense sunucusunda CORS açık olmalıdır:

1. QMC → **Virtual Proxies** → proxy seçin → **Edit**
2. **Additional response headers** alanına ekleyin:

```
Access-Control-Allow-Origin: https://analiz.dqturkiye.com
Access-Control-Allow-Headers: X-Qlik-Xrfkey, X-Qlik-User, Content-Type
Access-Control-Allow-Methods: GET, POST, OPTIONS
```

3. **Apply**

---

### ☁️ Qlik Cloud

| Alan | Açıklama |
|---|---|
| Tenant URL | `https://sirket.eu.qlikcloud.com` |
| API Token | Profil → API Keys → Token oluştur |

**API Token Nasıl Alınır**

1. Qlik Cloud → sağ üst köşede profil ikonuna tıklayın
2. **Profile settings** → **API Keys**
3. **Generate new key** → tokeni kopyalayıp uygulamaya yapıştırın

**Cloud ile çekilen veriler:**
- Space listesi ve app envanteri
- Veri bağlantıları (QVD, veritabanı, vs.)
- Reload task durumları

---

### Demo Mod

API key veya bağlantı olmadan: **Demo veriyle test et** butonunu kullanın. Her iki mod için de demo verisi mevcuttur.

---

## 📦 Özellikler

| Özellik | On-Premises | Cloud |
|---|---|---|
| App envanteri | ✅ QRS API | ✅ Cloud API |
| Stream / Space listesi | ✅ Stream | ✅ Space |
| Veri bağlantıları | ✅ QVD tespiti | ✅ Data connections |
| Reload task analizi | ✅ | ✅ |
| Sağlık skorlaması | ✅ | ✅ |
| Claude AI analizi | ✅ | ✅ |
| Word raporu | ✅ | ✅ |
| Excel envanteri | ✅ | ✅ |
| Danışman brief | ✅ | ✅ |

**Sağlık skorlama kriterleri:**
- 🟢 **Sağlıklı** — Son 7 gün içinde yüklenmiş
- 🟡 **Uyarı** — 7–30 gün arası
- 🔴 **Sorunlu** — 30+ gün yüklenmemiş

---

## 🔑 API Key Güvenliği

- Anthropic API key **yalnızca tarayıcıda** tutulur, sunucuya gönderilmez
- Her oturumda yeniden girilmesi gerekir
- Qlik Cloud token da aynı şekilde yalnızca tarayıcı oturumunda saklanır
- Production kullanım için API key'leri backend proxy üzerinden yönetmek tavsiye edilir

---

## 🏗️ Teknik Yapı

```
index.html                  ← tek dosya, sıfır bağımlılık
├── Bağlantı katmanı
│   ├── QRS API             ← On-Premises (fetch + XRF auth)
│   └── Qlik Cloud API      ← /api/v1 (Bearer token, sayfalama)
├── Claude AI               ← api.anthropic.com/v1/messages
├── docx (CDN)              ← Word üretimi
└── xlsx (CDN)              ← Excel üretimi
```

---

## 📄 Lisans

DQ Türkiye dahili kullanım. © 2026 DQ Türkiye.
