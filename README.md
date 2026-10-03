# /explain

Claude Code için bir skill. "Bu nasıl çalışıyor?" sorusunun cevabını terminalde uzun bir yazı olarak değil, tarayıcıda açılan şemalı bir sayfa olarak verir.

*A Claude Code skill that answers "how does this work?" with a visual HTML page grounded in your project's code. Every claim links to `file:line`.*

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
```

Örnek: `/explain hata bildirimi widget'tan GitHub'a nasıl gidiyor`

Plugin olarak kurulduğunda komut `/explain:explain` adıyla da görünebilir. Komutu yazmadan "bu akışı bana anlat" gibi bir istekle de çalışır.

## Video

Video modu henüz yok. Açıklayıcı video için [showtime](https://github.com/FavioVazquez/showtime) plugin'ini kurup Claude'dan doğrudan video isteyebilirsin. showtime yerel çalışır ve API key istemez.

## Fikir

Andrej Karpathy'nin [2 Ekim 2026 tarihli paylaşımı](https://x.com/karpathy/status/2105819303471976479): agent'lar işi giderek daha çok kendileri yapacak, bizim işimiz de onların ürettiğini anlamaya kayacak. Bu yüzden LLM çıktısını düz yazı yerine diyagram, HTML sayfa veya video olarak istemek daha verimli.

## Lisans

MIT
