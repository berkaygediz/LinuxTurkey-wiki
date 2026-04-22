# Immutable Linux Dağıtımları Nedir?

> **Yazar:** [SiyanWare](https://github.com/faydini065)

!!! note

    Bu yazı, immutable (değiştirilemez) Linux dağıtımlarına karşı önyargılı olan veya geleneksel Linux masaüstü anlayışını savunanlara yönelik olarak hazırlanmıştır. Tartışmanın merkezindeki "Immutable" kavramının ne anlama geldiğini teknik gerçekleriyle açıklamak en doğrusudur.

## Immutable Linux Dağıtımları Nedir?

Immutable Linux dağıtımları; Fedora Atomic (CoreOS, Silverblue, Kinoite vb.), AlmaLinux CoreOS, RHEL Bootc, Fedora Bootc, Bazzite, SteamOS, Ubuntu Core, **ChromeOS** ve **Android** gibi dağıtımlardır.

Bu sistemler genellikle sadece-okunur (read-only) bir temel imaj üzerine kurulur. Yeni paketler, bu temel imajın üzerine katmanlar (layer) eklenerek sistemize entegre edilir (Android ve ChromeOS'un kendi özel mimarileri vardır, ancak bu yazıda daha çok RPM-OSTREE tabanlı masaüstü dağıtımlarına odaklanılacaktır).

Sisteme paket eklemek için Distrobox gibi araçlara bağımlı değilsiniz; örneğin Fedora'da `rpm-ostree` kullanarak doğrudan ek katmanlar oluşturabilirsiniz. Ayrıca `/etc` (yapılandırma dosyaları) ve `/var` (kalıcı veriler) dizinleri tamamen değiştirilebilirdir. Bu yüzden "Immutable" kelimesi sistemdeki *her şeye* dokunulamaz anlamına gelmez; yalnızca temel sistemin bozulmadan korunmasını ifade eder. Ayrıca bu dağıtımlar, grafiksel uygulama kurulumları için genellikle Flatpak ve Snap gibi sandboxes paket formatlarına öncelik verir.

## Peki Neden Immutable Distrolar?

Immutable dağıtımlar, geleneksel kullanıcı alışkanlıklarından bağımsız olarak son kullanıcılar için oldukça güvenli ve verimli bir alternatiftir. Paket yönetim sistemlerinin bu yeni yapıya tam olarak adapte olmasıyla, immutable dağıtımlardaki günlük kullanım sorunlarının büyük çoğunluğu ortadan kalkacaktır.

Bu yapıyı önemli kılan temel unsurlar şunlardır:

- **Stabilite:** Temel sistem, dağıtım geliştiricileri tarafından yoğun bir şekilde test edilip sunulur. Bu sayede, Arch Linux gibi stabilitesinden ödün veren dağıtımları temel alan SteamOS gibi immutable sistemler, üst katmandaki değişikliklerden etkilenmeden stabilitesini korur.
- **Güvenlik:** Immutable dağıtımların çoğunda önyükleme (boot) güvenliği varsayılan olarak kapalı gelse de, Linux kernelinin `dm-verity` ve `fs-verity` gibi modülleri kullanılarak yazılım seviyesinde tam bir bütünlük kontrolü sağlanabilir. Bu durum son kullanıcı için fazlasıyla yeterli bir güvenlik katmanı oluşturur.

## Gelecek Immutable Distrolarda Mıdır?

Linux ekosistemindeki değişim ani olmasa da, 10 yıl önceki immutable dağıtımlara bakış açımız ile bugünkü bakış açımız aynı değildir. Günümüzde Linux masaüstünün eskide kalmış anlayıştan çıkıp yeni teknolojilere adapte olması, hatta bunu bir standart haline getirmesi gerekmektedir.

Bu devrim sadece dosya sisteminde değil, temel bileşenlerde de yaşanmalıdır. Örneğin geleneksel olarak kullanılan `glibc` ve `GRUB` gibi eski araçların yerini, daha modern ve güvenli alternatiflere bırakması; boot süreçlerinde `systemd-boot` (veya ihtiyaca göre `limine`) gibi araçlara geçiş yapılması Linux'un geleceği için önemli bir adımdır. Immutable mimari, bu modernizasyonun en büyük yapı taşı olacaktır.

---
SPDX-License-Identifier: CC0-1.0
