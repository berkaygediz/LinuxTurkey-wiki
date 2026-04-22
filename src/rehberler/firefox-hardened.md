# Firefox'u Hardened Yapma Rehberi

> **Yazar:** [SiyanWare](https://github.com/faydini065)

!!! note

    Linux Kullanıyorsanız ve Firefox'u kaynaktan derlediyseniz, sisteminizde otomatik olarak AppArmor veya SELinux profili oluşmayacaktır. Bu yüzden https://support.mozilla.org/tr/kb/linux-security-warning bu makaleyi okumanızı şiddetle öneririm. Aksi takdirde altta yaptığımız yapılandırmalar güvenlik açısından zayıf kalabilir. Paket yöneticilerinden (.deb, rpm vb.), Flatpak veya Snap üzerinden kurduysanız bu profiller genellikle hazır geldiği için makaleye bakmanıza gerek yoktur.

!!! note

    Geçmişte Firefox'un sandboxing (korumalı alan) mekanizması Chromium'dan geridedi. Site yalıtımı (Fission) gibi büyük güncellemelerle bu fark büyük ölçüde kapatılmış olsa da, en üst düzey güvenlik ve izolasyon istiyorsanız [bu makaleye](https://github.com/RKNF404/chromium-hardening-guide) göre Chromium'u hardened yapmak veya Fedora tabanlı bir dağıtım kullanıyorsanız Trivalent kullanmak gibi seçeneklere başvurabilirsiniz.

### about:config ayarları

Burada uzun uzun about:config ayarlarını ayarlamayı anlatsam makale sığmayacağı için size en iyisinden bir user.js öneriyorum: <https://github.com/arkenfox/user.js/>

### **Nasıl Kurulur?**

Arama çubuğunuza `about:support` yazın. Çıkan sayfada "Profil Dizini" kısmının yanındaki "Klasörü Aç" butonuna tıklayın. Varsayılan profiliniz genelde `xxxxxxxx.default-release` şeklindedir. Açılan bu klasörün içine, Github linkinden indirdiğiniz `user.js` dosyasını atın. Firefox'u yeniden başlattığınızda boş bir sayfa geliyorsa yapılandırma başarıyla tamamlanmış demektir.

**Değiştirmek İsteyeceğiniz Bazı Şeyler**

- **Firefox'u kapatınca gezinti verilerinin silinmesi:** Arkenfox varsayılan olarak bu özelliği açar (Şifreler varsayılan olarak bu silme işleminin dışında tutulur ancak yine de dikkatli olunmalıdır). Kendi şifre yöneticinizi kullanıyorsanız ve Firefox'un verileri silmesini istemiyorsanız, user.js dosyasını bir metin editörüyle (Kate, Nano, Vim, Gnome Text Editor, VSCode, VSCodium vb.) açıp `privacy.sanitize.sanitizeOnShutdown` değerini `false` yapmak veya satırın başına `#` (diyez) koymak yeterlidir.

- **Adres Çubuğu Önerileri:** Arama verileriniz varsayılan arama motorunuza gönderildiği için bu öneriler kapatılmıştır. Tekrar açmak için yine bir metin editörü kullanarak `browser.urlbar.suggest.history` ve `browser.urlbar.suggest.bookmark` değerlerini `true` yapmanız gereklidir.

- **Otomatik Şifre ve Form Doldurma:** Firefox'un kendi şifre yöneticisini kapatır. Eğer Firefox'un şifrelerinizi hatırlamasını istiyorsanız `signon.autofillForms` ve `browser.formfill.enable` değerlerini `true` yapmanız gereklidir.

- **WebRTC Görüntülü Görüşme Ayarları:** Arkenfox, gerçek IP adresinizin sızmasını önlemek için WebRTC'yi kısıtlar, fakat bu durum bazı görüntülü görüşme sitelerinin çalışmamasına sebebiyet verebilir. Görüşme yapacaksanız `media.peerconnection.enabled` ayarını `true` yapmalısınız.

- **Tarayıcıyı başlattığınızda açılacak sekme ve yeni sekme:** Arkenfox'ta varsayılan olarak bunlar boş sayfa olarak gelir. Kendi ayarlarınızı (anasayfanızı) belirlemek istiyorsanız dosyanın 93. ve 97. satırlarının başına `#` işareti koymanız gerekmektedir.

*Not: Yukarıda bahsettiğim şeyleri mümkün olduğunca değiştirmemeniz güvenlik ve mahremiyetiniz için en iyisi olacaktır.*

### İşinize Yarayabilecek Bazı Eklentiler

!!! note

    Gizliliğinizi korumak için eklentileri minimumda tutmanız önerilir. Çünkü her eklenti tarayıcınızın parmak izini (fingerprint) değiştirir ve benzersiz bir parmak izi, web sitelerinin sizi daha agresif bir şekilde takip etmesi demektir.

[uBlock Origin](https://addons.mozilla.org/tr/firefox/addon/ublock-origin/): Reklam ve bazı takip çerezlerini önlemek için çokça tercih edilen, Firefox'ta en çok kullanılan eklentilerden biridir.

[NoScript](https://addons.mozilla.org/tr/firefox/addon/noscript/): JavaScript'i kısıtlamak için kullanılan eklenti. İşinde başarılı olsa da temel düzeyde JavaScript engellemek uBlock Origin ile de yapılabilir. **Yani güvenlik katmanı tekrarını (redundancy) sevmediğiniz sürece uBlock Origin ile beraber bunu kullanmak mantıksızdır.**

[ClearURLs](https://addons.mozilla.org/tr/firefox/addon/clearurls/): Bazı takip parametrelerini URL'den temizler. Fakat uBlock Origin'e Adguard URL Tracking Protection gibi özel bir filtre listesi eklerseniz bu işi eklentisiz halletmiş olursunuz. Eklentileri minimumda tutmak gizlilik ve güvenliği artırdığı için **uBlock Origin ile beraber bunu kullanmak mantıksızdır.**

[Decentraleyes](https://addons.mozilla.org/tr/firefox/addon/decentraleyes/) / [LocalCDN](https://addons.mozilla.org/tr/firefox/addon/localcdn-fork-of-decentraleyes/): Google gibi CDN kütüphaneleri yerine yerel çözümler sunarak takip edilmenin önüne geçer (LocalCDN daha günceldir ve daha fazla kütüphane destekler). Sitelerin hızını biraz yavaşlatabilir. [Arkenfox'un açıklamasına](https://github.com/arkenfox/user.js/wiki/4.1-Extensions) göre Firefox'un ilk taraf yalıtımı (partitioning) özelliği sayesinde bu CDN'lerin sizi siteler arası takip etmesi zaten engelleniyormuş. Bu yüzden gizlilik açısından kullanılmasına gerek yokmuş.

### Bazı Ayarlar

**Gelişmiş İzlenme Koruması:** Ayarlar > Gizlilik ve Güvenlik > Gelişmiş İzlenme Koruması altındaki ayarı "Sıkı/Strict" yapmanız önerilir. Sitede dolaşırken herhangi bir sıkıntı yaşamamak için "Önemli site sorunlarını düzelt" ve "Küçük site sorunlarını düzelt" seçeneklerini işaretlemeniz faydalı olacaktır.

**Diğer Gizlilik Ayarları:** Ayarlar > Gizlilik ve Güvenlik menünüzde yer alan şu ayarları yapmanız da önerilir:

- Tehlikeli ve Aldatıcı İçerikleri Engelle kısmındaki korumayı açın.
- DNS over HTTPS (DoH) ayarını artırılmış korumayı seçecek şekilde etkinleştirin ve kendinize uygun bir DNS sağlayıcı (örneğin NextDNS) seçin.
- "Yalnızca HTTPS modunu tüm pencerelerde etkinleştir" seçeneğini işaretleyin.

---
SPDX-License-Identifier: CC0-1.0
