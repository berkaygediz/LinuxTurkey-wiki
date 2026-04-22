# Flatpak ile Web Tarayıcılarını Kullanmayın

> **Yazar:** [SiyanWare](https://github.com/faydini065)

Flatpak, yeni nesil bir paket yönetim sistemidir. Güvenlik ve bağımlılık yönetimi için **bubblewrap** kullanır ve kendi izolasyon sistemiyle uygulamaları çalıştırır. Normal (native) paketleri sandboxladığı için arka planda gözle görülmeyen bir yavaşlama mevcut olsa da, genel olarak sistem paketlerinden daha güvenli bir yapıya sahiptir.

Fakat, konu web tarayıcılarına gelince durum değişir. Flatpak sandbox'u ile Chromium ve Firefox'un kendine has güvenlik sandbox'ları birbiriyle çakışır. Uygulamanın çalışmasını sağlamak için Flatpak paketleyicileri, tarayıcının üzerine modifiye edilmiş bir **bubblewrap** (örneğin zypak) sandbox'u atarlar ve tarayıcının kendi yerleşik sistem sandbox'unu büyük ölçüde devre dışı bırakırlar. Bu nedenle, web tarayıcılarını Flatpak ile kullanmak genel olarak **güvensiz kabul edilir**.

Bu güvenlik zafiyeti nedeniyle, [Vivaldi geliştiricileri](https://forum.vivaldi.net/topic/33411/flatpak-support?page=10) ve [Chromium geliştiricileri](https://issues.chromium.org/issues/40928753#comment5) Flatpak'ı resmi olarak desteklemeyeceklerini açıkça belirtmişlerdir. Bu sorunu "çözmek" için kullanılan zypak gibi yöntemler veya kaynak koda yapılan yamalar aslında birer bypass (geçiş) çözümüdür, temel sorunu gizlerler.

!!! note

    **Epiphany (GNOME Web) İstisnası:** Epiphany'nin sandbox yapısı Flatpak'ın mimarisine uygun olarak tasarlandığı için bu çakışma sorunu Epiphany için geçerli değildir.

!!! note

    Bu durum, Chromium ve Firefox tabanlı **tüm** tarayıcılar için geçerlidir (Brave, Microsoft Edge, LibreWolf, Mullvad Browser vb.).

---
SPDX-License-Identifier: CC0-1.0
