# Linux Sisteminde Grafik Arayüzlü Başlatma: Plymouth

> **Yazar:** [SiyanWare](https://github.com/faydini065)

Plymouth, özellikle Fedora gibi modern dağıtımlarda açılış sürecini kullanıcı dostu hale getiren bir grafik arayüz araçtır. İçinde `bgrt`, `spinner` gibi çeşitli temalar barındırır.

Çalışma prensibi şöyledir: Çekirdek (kernel) ve initramfs üzerinden temel grafik sürücülerinin (DRM/KMS) yüklenmesini sağlar, initramfs içine kendi dosyalarını da yerleştirir. Böylece sistem açılırken ekrana akan logları, seçtiğiniz bir grafik tema arkasında gizleyebilirsiniz.

Bu rehberde Plymouth yapılandırmasını nasıl yapacağınızı anlatacağım.

### GRUB Ayarları

Bu rehberde sadece GRUB bootloader'ına değineceğim, fakat Limine veya systemd-boot kullanan sistemlerde de benzer kernel parametreleri ile aynı sonuç elde edilebilir.

!!! note

    Eğer `sudo` yerine `doas` veya `run0` gibi bir araç kullanıyorsanız, aşağıdaki komutlardaki `sudo` yazan yerleri kullandığınız araçla değiştirin.

- **/etc/default/grub:** Bu dosyada bazı değerleri değiştireceksiniz. `sudo nano /etc/default/grub` komutuyla (veya tercih ettiğiniz başka bir metin editörüyle) dosyayı açın ve aşağıdaki düzenlemeleri yapın:

```bash
GRUB_TIMEOUT_STYLE=hidden

GRUB_CMDLINE_LINUX_DEFAULT="quiet splash vt.global_cursor_default=0 loglevel=3 rd.udev.log_priority=3 udev.log_priority=3"

GRUB_TIMEOUT=3
```

*Not: `GRUB_TIMEOUT=3` ayarı sistemin açılışını 3 saniye geciktirmez. Sistem açılırken menüye girmek isterseniz size 3 saniyelik bir tuş basma hakkı tanır (varsayılanı 5'tir). İsterseniz bunu `0` yapabilirsiniz, ancak herhangi bir kernel panic veya hata durumunda GRUB menüsüne girip sistemi kurtarmak için uğraşmak zorunda kalabilirsiniz.*

- **/etc/grub.d/10_linux:** (Özellikle Fedora tabanlı sistemlerde) GRUB menüsünde "quiet" (sessiz) boot yaparken fazladan log satırlarının görünmesini engellemek için bu dosyada küçük bir değişiklik yapabilirsiniz. `sudo nano /etc/grub.d/10_linux` komutuyla dosyayı açın ve `quiet_boot=0` değerini `quiet_boot=1` olarak değiştirin.

### Plymouth Ayarları

Sisteminizde Plymouth yüklü değilse önce kurmanız gerekir. Kurulum komutu dağıtımdan dağıtıma değişebilir. Örneğin Debian tabanlı dağıtımlarda bu komut `sudo apt install plymouth plymouth-themes -y` şeklindedir.

Plymouth'u kurduktan sonra bir tema seçin. UEFI tabanlı bir sistem kullanıyorsanız (donanımınız oldukça yeni ise öyledir) üreticinin logosunu gösteren `bgrt` temasını, Legacy BIOS kullanıyorsanız dönen bir yükleme çubuğu olan `spinner` temasını öneririm.

Tema seçmek için terminalde şu komutlardan birini kullanın (komuttaki tema adını kendi seçiminize göre değiştirin):

```bash
sudo plymouth-set-default-theme -R bgrt
# veya
sudo plymouth-set-default-theme -R spinner
```

### Son Ayarlar

Yaptığınız tüm GRUB değişikliklerinin diske kaydedilmesi için GRUB'u güncellemeniz gerekir.

!!! warning

    Debian/Ubuntu tabanlı dağıtımlarda `sudo update-grub` komutu çalışırken, **Fedora/RHEL tabanlı dağıtımlarda bu komut bulunmaz.** Fedora kullanıyorsanız şu komutu uygulamalısınız:
    `sudo grub2-mkconfig -o /boot/grub2/grub.cfg` (UEFI kullanıyorsanız `/boot/efi/EFI/fedora/grub.cfg` olabilir, ancak genellikle ilk komut sembolik link olarak işinizi görür).

Ayarları kaydettikten sonra sistemi yeniden başlatarak Plymouth temasının başarıyla çalışıp çalışmadığını kontrol edebilirsiniz.

---
SPDX-License-Identifier: CC0-1.0
