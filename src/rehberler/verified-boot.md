# Boot Güvenliğini Anlamak | Verified Boot Mechanism

> **Yazar:** [SiyanWare](https://github.com/faydini065)

!!! note

    Bu makalede daha çok ChromiumOS'taki ve Android'deki Verified Boot düzenine temel olarak değinilmiştir. Zaman zaman Apple Trusted Boot ve EDK2 Secure Boot'a da değinilse de, temel olarak Google'ın sunduğu açık kaynak Verified Boot sistemleri öne çıkmaktadır.

Verified Boot, genel sistem bölümlerini korumak ve kötü amaçlı yazılımların (özellikle rootkitlerin) sistemin derinliklerine sızmasını önlemek için uygulanan bir güvenlik çözümüdür. Verified Boot'un temel amacı, başlatılan sistem dosyalarının saldırgandan değil, üreticiden (veya son kullanıcının kendisinden) geldiğinden kriptografik olarak emin olmaktır.

Verified Boot her saldırıyı engelleyemeyeceğini bilmenizle beraber; temel hedefi cihazın modifiye edilip edilmediğinden emin olmak ve saldırganlara karşı güçlü bir caydırıcı unsur olarak yer almaktır. Kullanıcı verilerini doğrudan şifrelemez ve bir Uygulama Sandbox'ı (korumalı alan) sağlamaz.

Verified Boot'un mevcut olduğu cihazlara örnek olarak MacBook'lar, iPhone'lar, genel Android cihazları, Windows 11 Secure Core PC destekleyen bilgisayarlar ve Chromebook'lar verilebilir.

### Temel Çalışma Mantığı

Verified Boot, genel olarak cihaz firmware'ındaki read-only (sadece okunabilir) bir bölümün kodu kriptografik olarak doğrulanması ile başlar. Ardından genellikle kernel, bootloader veya MacBook'lardaki LLB gibi bir bileşen doğrulanarak başlatılır ve bu işlem sırayla tüm sistem partitionlarını doğrulamaya kadar gider.

## Android Verified Boot

Genel Android cihazlarında, verified boot döngüsü Boot ROM'un başlatıcıyı (genellikle bootloader) doğrulaması ile başlar. Ardından bootloader kernel'i doğrular ve geri kalan doğrulama işlemlerini kernele devreder.

Android'de bu döngünün temel yapı taşı **vbmeta** partitionudur. Bu partition, altında bulunan diğer partitionların kriptografik doğrulama verilerini (hash ve hashtree metadata) barındırır. Yapı genel olarak şöyledir:

```text
vbmeta (hash for boot - hashtree metadata for "system" - hashtree metadata for "vendor")

boot (payload)
system (payload hashtree)
vendor (payload hashtree)
other partitions (payload hashtree)
```

Ayrıca Android, A/B partition sistemini kullandığı için bu yapı `vbmeta_a`, `system_a` ve `vbmeta_b`, `system_b`, `vendor_b` gibi şekillerde ikiye ayrılabilir. Bazı Android sürümlerinde veya üreticilere göre `vbmeta_system_a` gibi ek ayrılımlar da görülebilir.

Android, bu hashtree'leri doğrulamak için Linux kernelindeki `dm-verity` modülünü kullanır. Teknik olarak siz de kendi ayırdığınız partitionları bu modülle koruyabilirsiniz; fakat geleneksel Linux masaüstü yapısı buna pek elverişli olmadığından, masaüstü Linux için `fs-verity` daha dost canlısı ve pratik bir çözümdür.

!!! note

    Masaüstü Linux'ta vbmeta benzeri bir yapıya gerçekten gerek yoktur. Çünkü vbmeta, farklı üreticilerin (örneğin kernel'i imzalayan Google ile driver'ları yapan Samsung veya Huawei gibi) birbirlerinin imzalarını bilmeden Verified Boot sistemini rahatça yönetebilmesi için tasarlanmış karmaşık bir zincirleme imza sistemidir.

### Rollback Index (Geri Dönüş Engelleme)

İmzalı ancak daha sonra bulunan bir güvenlik açığına sahip (vulnerable) sistem dosyalarının önyüklenmesini engellemek için Android, sistem dosyalarına bir "rollback index" değeri atar. Bu sayede saldırgan, cihazı eski ve açığı bulunan bir sistem sürümüne düşüremez.

### Bootloader Unlock ve Özel Anahtarlar (Avb_custom_key)

Verified Boot ekosisteminde bulunmasına rağmen her üreticinin izin vermediği iki önemli özellik vardır:

- **Bootloader Unlock (Açık Bootloader):** Bootloader kilidi açıldığında sistem "Orange State" (Turuncu Durum) moduna girer. Yazma kilitleri kaldırılır ve bootloader artık kernel'i doğrulamaz. Fakat işletim sistemi düzeyinde kernel, hala `dm-verity` ile `system`, `vendor` gibi partitionları doğrulamaya devam eder.
- **Avb_custom_key (Özel Anahtar):** Kullanıcı kendi ürettiği anahtarla sistemleri imzalayıp vbmeta'ya eklediğinde sistem "Yellow State" (Sarı Durum) moduna girer. Kullanıcı tarafından imzalanmış şeyler boot edilebilir, fakat saldırgan kullanıcının özel anahtarına erişemediği sürece sistem korumalı ve güvenli kalmaya devam eder. GrapheneOS gibi gizlilik odaklı işletim sistemleri bu yöntemi kullanır.

## ChromeOS Verified Boot

ChromeOS'ta verified boot çözümü, `vboot` adı verilen bir coreboot payload'ının bootloader'ı doğrulaması ile başlar. Ardından bootloader kernel'i doğrular ve Linux kernel'i `ROOT-A`, `ROOT-B` ve diğer doğrulanması gereken partitionları kontrol eder. Eğer doğrulama eşleşmezse sistemi kesinlikle boot etmez.

Android'e göre yapısal olarak daha sade ve karmaşık olmayan bir mimariye sahiptir. Temel bir koruma katmanı gibi görünse de, güvenlik uzmanları tarafından consumer (son kullanıcı) cihazlarda endüstri standartlarını belirleyen, oldukça sağlam ve başarılı bir mimari olarak değerlendirilir.

---

## Kaynakça

- [Android AVB README (Google Source)](https://android.googlesource.com/platform/external/avb/+/android16-qpr2-release/README.md#the-vbmeta-digest)
- [Chromium OS Design Docs - Verified Boot](https://www.chromium.org/chromium-os/chromiumos-design-docs/verified-boot/)
- [Android Security - Verified Boot](https://source.android.com/docs/security/features/verifiedboot/avb?hl=tr)

---
SPDX-License-Identifier: CC0-1.0
