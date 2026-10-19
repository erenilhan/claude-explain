# /explain

[English](README.md)

Claude Code için bir skill. "Bu nasıl çalışıyor?" sorusunun cevabını terminalde uzun bir yazı olarak değil, tarayıcıda açılan şemalı bir sayfa olarak verir. Sayfadaki her iddia dayandığı `dosya:satır` ile birlikte yazılır.

## Ne yapar

1. Çalıştığın projenin kodunu okur. Ezberden veya dosya adlarından anlatmaz.
2. Projenin gerçek sınıf, rota ve tablo adlarıyla bir sayfa hazırlar: kısa özet, akış şeması, açılır adımlar.
3. Sayfanın sonuna **İddialar / Kaynaklar** tablosu koyar. Her iddianın yanında dayandığı `dosya:satır` yazar. Kodu okuyarak çıkardığı ama test etmediği şeyleri ayrıca işaretler.
4. Sayfayı claude.ai Artifact olarak yayınlar ve linki verir. Hesabında Artifact yoksa HTML dosyasını `~/explainers/` klasörüne kaydedip tarayıcıda açar.

Projeye dosya yazmaz.

## Kurulum

Claude Code içinde:

```
/plugin marketplace add erenilhan/claude-explain
/plugin install explain@claude-explain
```

Plugin olarak kurmak istemezsen `skills/explain/SKILL.md` dosyasını `~/.claude/skills/explain/SKILL.md` yoluna kopyalaman yeterli.

## Kullanım

```
/explain <konu>              # varsayılan: sayfa
/explain sayfa <konu>        # etkileşimli HTML sayfa
/explain diyagram <konu>     # tek bir şema
/explain video <konu>        # seslendirmeli açıklama videosu
```

Örnek: `/explain hata bildirimi widget'tan GitHub'a nasıl gidiyor`

Plugin olarak kurulduğunda komut `/explain:explain` adıyla da görünebilir. "Bu akışı bana anlat" gibi bir istekle de çalışır. Sayfa, soruyu hangi dilde sorduysan o dilde yazılır.

## Video

`/explain video <konu>` aynı kod okumasından 60–90 saniyelik seslendirmeli bir video üretir. Bunun için [showtime](https://github.com/FavioVazquez/showtime) plugin'i gerekir. showtime yerel çalışır ve API anahtarı istemez. showtime kurulu değilse skill sayfa önerir.

showtime'ın hazır seslerinde Türkçe yok. Türkçe seslendirme için skill [Antalia-2 Mini](https://huggingface.co/cloud0day3/antalia-mini) modelini kullanır. Model Apache-2.0 lisanslıdır, 7.6M parametrelidir, CPU'da çalışır ve tek bir yapay erkek sesi vardır. Kurulum: `uv tool install "antalia-mini[hub]"`. Model lisansı istediği için video, sesin yapay olduğunu belirtir.

Video taranamaz. Bu yüzden skill, videonun söylediklerini kontrol etmen için iddia dosyasını da verir.

## Kaynaklar

Skill'in çıkış noktası [Andrej Karpathy'nin 2 Ekim 2026 tarihli paylaşımı](https://x.com/karpathy/status/2105819303471976479) ve altındaki yanıtlar. 4 Ekim 2026'da 229 yanıt okundu: X'in gösterdiği 194 yanıtın hepsi, X'in olası spam olarak işaretlediği 21 yanıt ve fxtwitter API'nin döndürdüğü 14 yanıt daha. Yanıtlara verilen yanıtlar okunmadı.

[docs/SOURCES.md](docs/SOURCES.md) dosyasında her tweet linki ve tarihiyle birlikte listeleniyor:
- **Prior art:** Benzer işi yapan hazır araçlar (`/explain-better`, `/show-me`, `/explain-to-me`, lucidiff, showtime). Kurmadan önce bunlara da bakmanızı öneririz.
- **Replies that shaped the design:** Hangi tasarım kararının hangi tweet'ten geldiği.
- **Ideas not used yet:** Henüz uygulanmayan fikirler.
- **Trade-offs:** Yanıtlarda dile getirilen itirazlar.

## Lisans

MIT
