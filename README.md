# Pro Portfolio Manager

Kişisel portföy takip uygulaması - PWA destekli, mobil uyumlu.

## 🚀 Kurulum

1. Ayarlar sayfasına gidin
2. Aşağıdaki bilgileri girin:

| Alan | Açıklama |
|------|----------|
| **Supabase URL** | `https://YOUR_PROJECT.supabase.co` |
| **Supabase Key** | Anon/Public key (Dashboard > Settings > API) |
| **OpenAI API Key** | (Opsiyonel) AI analiz için |

3. "Kaydet & Yenile" butonuna tıklayın

## 📱 Özellikler

- ✅ PWA - Ana ekrana eklenebilir
- ✅ Mobil responsive tasarım
- ✅ iPhone notch/safe-area desteği
- ✅ Parmakla kaydırarak sekme geçişi
- ✅ Canlı fiyat takibi (Supabase)
- ✅ AI portföy analizi (OpenAI)
- ✅ Excel/CSV export

## 🔒 Güvenlik

Bu repo'da **hiçbir hardcoded API key bulunmamaktadır**.  
Tüm API bilgileri kullanıcı tarafından Ayarlar'dan girilir ve tarayıcının localStorage'ında saklanır.

## 📁 Dosya Yapısı

```
portföy/
├── index.html      # Ana uygulama (tek dosya)
├── manifest.json   # PWA manifest
├── sw.js           # Service Worker
├── icon-192.png    # PWA icon
├── icon-512.png    # PWA icon
└── README.md       # Bu dosya
```

## 📝 Lisans

Private - Tüm hakları saklıdır.
