# Gönderi Atölyesi Pro

Instagram gönderi oluşturucu (canvas tabanlı) + yapay zeka destekli açıklama & hashtag üretici.
Bu sürüm tamamen **statik** bir sayfadır — backend/sunucu yok, doğrudan GitHub Pages'te çalışır.
Yapay zeka için Google'ın **ücretsiz** Gemini API'si kullanılıyor.

## ÖNEMLİ — Güvenlik notu

API key `index.html` içine yazılıyor, bu yüzden sitenizi ziyaret eden herkes tarayıcının
"Sayfa Kaynağını Görüntüle" ekranından bu key'i görebilir. Bunu tamamen engellemenin yolu
bir backend kullanmaktır (bkz. önceki Vercel sürümü), ama bu proje bilerek daha basit
tutuldu çünkü:

- Gemini'nin ücretsiz katmanı **kredi kartı istemez** ve **faturalandırma yapmaz** —
  biri key'inizi kopyalasa sizi parasal olarak zarara uğratamaz, en fazla günlük/dakikalık
  kotanızı tüketip sizi geçici olarak durdurur.
- Riski azaltmak için key'i **kendi domaininize kilitleyebilirsiniz** (aşağıda 3. adım).

Ciddi/production bir proje ise yine backend üzerinden gizlemeniz önerilir.

## Kurulum

### 1. Ücretsiz Gemini API key alın

1. [aistudio.google.com](https://aistudio.google.com) adresine gidin, Google hesabınızla girin.
2. "Get API key" → "Create API key" deyin. Kredi kartı istemez.
3. Oluşan key'i kopyalayın (`AIza...` ile başlar).

### 2. Key'i projeye ekleyin

`index.html` dosyasını açın, şu satırı bulun:

```js
const GEMINI_API_KEY = 'BURAYA_GEMINI_API_KEYINIZI_YAPISTIRIN';
```

Tırnak içine kendi key'inizi yapıştırın.

### 3. (Önerilir) Key'i kendi domaininize kilitleyin

1. [console.cloud.google.com](https://console.cloud.google.com) → **APIs & Services → Credentials**.
2. Oluşturduğunuz key'e tıklayın.
3. **Application restrictions → Websites** seçin, GitHub Pages adresinizi ekleyin
   (örn. `kullaniciadi.github.io/*`).
4. Kaydedin. Artık key sadece bu domainden gelen isteklerde çalışır.

### 4. GitHub'a yükleyip yayınlayın

```bash
git init
git add .
git commit -m "İlk sürüm"
git branch -M main
git remote add origin https://github.com/KULLANICI_ADINIZ/REPO_ADINIZ.git
git push -u origin main
```

Sonra reponuzda **Settings → Pages → Branch: main → Save** deyin. Birkaç dakika içinde
`https://KULLANICI_ADINIZ.github.io/REPO_ADINIZ/` adresinde canlı olur.

## Ücretsiz kotayı aşarsam ne olur?

`gemini-2.5-flash` modeli ücretsiz katmanda dakikada ~10-15, günde ~1500 istek gibi bir
limitle çalışıyor (Google zaman zaman değiştirebiliyor — güncel rakamlar
[ai.google.dev/gemini-api/docs/rate-limits](https://ai.google.dev/gemini-api/docs/rate-limits)
sayfasında). Limit dolarsa buton "kota anlık limitine ulaşıldı" uyarısı gösterir, bir süre
sonra tekrar dener.

## Dosya yapısı

```
gonderi-atolyesi/
├── index.html   ← Arayüz + canvas çizimi + AI çağrısı (hepsi burada)
├── README.md
└── .gitignore
```

## Daha güvenli/production seçenek

Eğer ileride key'i tamamen gizlemek isterseniz, önceki mesajlarımdaki Vercel + backend
sürümüne dönebiliriz — orada key sunucu tarafında kalır, hiçbir kullanıcı göremez.
