# /explain

A Claude Code skill that answers "how does this work?" with a visual page grounded in your project's own code, instead of a wall of terminal text. Every claim on the page links to the `file:line` it came from.

[Türkçe açıklama aşağıda.](#türkçe)

## What it does

1. Reads the code of the project you are in. It does not explain from memory or from file names.
2. Builds a page with the project's real class, route and table names: a short summary, a flow diagram, and expandable steps.
3. Ends the page with a **Claims / Sources** table. Each claim lists the `file:line` it rests on. Anything inferred rather than read is marked as such.
4. Publishes the page as a claude.ai Artifact and gives you the link. If your account has no Artifact tool, it saves the HTML to `~/explainers/` and opens it in your browser.

It never writes files into your project.

## Install

In Claude Code:

```
/plugin marketplace add erenilhan/claude-explain
/plugin install explain@claude-explain
```

Without the plugin system: copy `skills/explain/SKILL.md` to `~/.claude/skills/explain/SKILL.md`.

## Usage

```
/explain <topic>              # default: page
/explain page <topic>         # interactive HTML page
/explain diagram <topic>      # one focused diagram
```

Example: `/explain how a bug report travels from the widget to GitHub`

When installed as a plugin, the command may also appear as `/explain:explain`. A plain request such as "walk me through this flow" triggers it too. The page is written in the language you ask in. Turkish mode names (`sayfa`, `diyagram`) also work.

## Video

There is no video mode yet. For narrated explainer videos, install [showtime](https://github.com/FavioVazquez/showtime) and ask Claude for a video directly. It renders locally and needs no API keys.

## Sources

This skill came from one post and the replies under it. The design choices below are traced to the tweet that prompted them. Replies were read on 2026-10-03 (the first 34 replies returned by the fxtwitter API, not the full thread).

### The original post

| Post | What it says |
|---|---|
| [Andrej Karpathy (@karpathy), 2026-10-02](https://x.com/karpathy/status/2105819303471976479) | As LLMs do more work on their own, more of our time goes into understanding their output. He ranks output formats by how well they help: writing in ASD-STE100, then diagrams, then HTML pages, then custom explainer videos. Since code is now cheap, ask for large, disposable artifacts that never made sense to build before. |

### Replies that shaped the design

| Reply | What it says | What we did |
|---|---|---|
| [@akshaymarch7, 2026-10-02](https://x.com/akshaymarch7/status/2105864047510061223) | The biggest teaching opportunity is building the explanation around the learner's exact code, so they do not have to map a generic example back to their own. | The skill reads the current project first and uses its real names. Generic examples count as a failure. |
| [@somi_ai, 2026-10-02](https://x.com/somi_ai/status/2105828320713863598) | A wrong claim narrated over a polished animation is harder to catch than a wrong sentence. | Every page ends with a Claims / Sources table, and inferred claims are marked. |
| [@eliebakouch, 2026-10-02](https://x.com/eliebakouch/status/2105866163800637585) | Interactive HTML is probably one of the most time-efficient ways to understand model output. | HTML page and diagram modes were built first. |
| [@elliotarledge, 2026-10-02](https://x.com/elliotarledge/status/2105828285213233166) | Used the approach for a week and found it tiring; it lasts a week or two. | No video pipeline was built before the cheaper formats proved useful. |
| [@FavioVaz, 2026-10-03](https://x.com/FavioVaz/status/2106245948502319366) | Made a 3Blue1Brown-style explainer with Manim and a local voice, no API keys, using [showtime](https://github.com/FavioVazquez/showtime). | We point to showtime for video instead of writing our own pipeline. |
| [@alik_huseyn0v, 2026-10-02](https://x.com/alik_huseyn0v/status/2105989790017454106) | Links an existing ASD-STE100 skill: [danyuchn/asd-ste100-skill](https://github.com/danyuchn/asd-ste100-skill). | Not used. The skill writes in the user's language with a few plain-style rules instead, because ASD-STE100 is English-only. |
| [@originell, 2026-10-02](https://x.com/originell/status/2105879773012520966) | For code, mixing ASD-STE100 with Google's Developer Documentation Style Guide works better. | Informed the short writing rules in `SKILL.md`. |

### Other ideas from the replies, not used yet

| Reply | Idea |
|---|---|
| [@ljupc0, 2026-10-02](https://x.com/ljupc0/status/2105886509601579285) | Ask for a storyboard when you have half-forgotten how something works. |
| [@phinance99, 2026-10-02](https://x.com/phinance99/status/2105991384775999733) | Technical reports as PDFs via TeX. |
| [@gonzalo_io, 2026-10-02](https://x.com/gonzalo_io/status/2106146490259533832) | Excalidraw diagrams. |
| [@pawanpoolla, 2026-10-02](https://x.com/pawanpoolla/status/2105875977356312693) | ASCII diagrams for simple topics, HTML for richer ones. |

Summaries above are paraphrased. Follow the links for the original wording.

## License

MIT

---

## Türkçe

Claude Code için bir skill. "Bu nasıl çalışıyor?" sorusunun cevabını terminalde uzun bir yazı olarak değil, tarayıcıda açılan şemalı bir sayfa olarak verir. Sayfadaki her iddia dayandığı `dosya:satır` ile birlikte yazılır.

### Ne yapar

1. Çalıştığın projenin kodunu okur. Ezberden veya dosya adlarından anlatmaz.
2. Projenin gerçek sınıf, rota ve tablo adlarıyla bir sayfa hazırlar: kısa özet, akış şeması, açılır adımlar.
3. Sayfanın sonuna **İddialar / Kaynaklar** tablosu koyar. Her iddianın yanında dayandığı `dosya:satır` yazar. Kodu okuyarak çıkardığı ama test etmediği şeyleri ayrıca işaretler.
4. Sayfayı claude.ai Artifact olarak yayınlar ve linki verir. Hesabında Artifact yoksa HTML dosyasını `~/explainers/` klasörüne kaydedip tarayıcıda açar.

Projeye dosya yazmaz.

### Kurulum

Claude Code içinde:

```
/plugin marketplace add erenilhan/claude-explain
/plugin install explain@claude-explain
```

Plugin olarak kurmak istemezsen `skills/explain/SKILL.md` dosyasını `~/.claude/skills/explain/SKILL.md` yoluna kopyalaman yeterli.

### Kullanım

```
/explain <konu>              # varsayılan: sayfa
/explain sayfa <konu>        # etkileşimli HTML sayfa
/explain diyagram <konu>     # tek bir şema
```

Örnek: `/explain hata bildirimi widget'tan GitHub'a nasıl gidiyor`

Plugin olarak kurulduğunda komut `/explain:explain` adıyla da görünebilir. "Bu akışı bana anlat" gibi bir istekle de çalışır. Sayfa, soruyu hangi dilde sorduysan o dilde yazılır.

### Video

Video modu henüz yok. Seslendirmeli açıklayıcı video için [showtime](https://github.com/FavioVazquez/showtime) plugin'ini kurup Claude'dan doğrudan video isteyebilirsin. Yerel çalışır, API key istemez.

### Kaynaklar

Skill'in çıkış noktası [Andrej Karpathy'nin 2 Ekim 2026 tarihli paylaşımı](https://x.com/karpathy/status/2105819303471976479) ve altındaki yanıtlar. Hangi tasarım kararının hangi tweet'ten geldiği yukarıdaki **Sources** bölümünde, her tweet'in linki ve tarihiyle birlikte listeleniyor. Yanıtlar 3 Ekim 2026'da okundu. fxtwitter API'nin döndürdüğü ilk 34 yanıt okundu, tüm thread değil.
