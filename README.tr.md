# İş Başvurusu Otomasyonu (n8n)

Küçük ve orta ölçekli teknoloji firmalarını bulan, alan adlarını doğrulayan ve zamanlanmış şekilde kişiselleştirilmiş iş başvurusu e-postaları gönderen bir n8n projesi. Güvenlik önlemleri içerir: MX doğrulama, günlük limit, geri dönen (bounce) mail takibi ve otomatik durdurma.

> **Durum:** çalışan prototip. Öğrenme/portföy projesi olarak geliştirildi. Neyin çalışıp neyin çalışmadığı için [Sınırlamalar](#sınırlamalar-ve-öğrenilenler) bölümüne bak.

## Nasıl çalışır
1. Hafta içi 10:00'da tetiklenir, Google Sheets'teki `Queue` listesini okur.
2. Bir LLM'den (Groq) firma ve tahmini İK e-posta adresleri ister.
3. Tekrarları eler, alan adlarının mail kabul edip etmediğini (MX) Google DNS ile kontrol eder.
4. Geçenleri kuyruğa yazar; günlük limite ve geri dönme koruması kurallarına göre o günün listesini seçer.
5. Her firmaya sırayla CV ekli mail gönderir, `sent` işaretler, 2 dakika bekler.
6. İkinci workflow her sabah geri dönen mailleri (`mailer-daemon`) tarayıp ilgili satırları `bounced` yapar.

## Güvenlik önlemleri
MX doğrulama · tekrar eleme · günlük limit (varsayılan 5) · geri dönme oranı %30'u geçerse durdurma · yalnızca hafta içi · sırlar n8n credential'larında, repoda yok.

## Kurulum
Kurulum adımları için İngilizce [README.md](README.md) dosyasına bak (Sheets sekmesi `Queue`, başlıklar `sheet-template/Queue-header.csv`, Google OAuth credential'ları, iki workflow'u içe aktar, kimlikleri yaz, `PROFILE` ve `DAILY_CAP` ayarlarını düzenle).

Bilgisayar kapalıyken çalıştırmak için: [docs/DEPLOY.md](docs/DEPLOY.md).

## Sınırlamalar ve öğrenilenler
- LLM gerçek İK adreslerini bilmez, tahmin eder. MX kontrolü yalnızca alan adının mail aldığını gösterir, adresin var olduğunu değil. Bu yüzden bounce takibi ve koruma var.
- Büyük kurumlar genelde CV'yi mail yerine kariyer portalından alır.
- Hunter/Apollo gibi servisler Türk KOBİ'lerinde zayıf kaldı.
- Kişisel Gmail'den çok sayıda soğuk mail itibarı düşürür; düşük hacimle başla.

## Sorumlu kullanım
Sadece gerçek iş başvuruları için, kurumsal rol adreslerine, düşük hacimle kullan; KVKK/GDPR kurallarına uy.

## Lisans
MIT
