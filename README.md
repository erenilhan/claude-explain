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

This skill came from one post and the replies under it. Each design choice below is traced to the post that prompted it, with a link and date, so you can check the reasoning against the original wording.

**What was read.** On 2026-10-04 we read 229 replies: all 194 replies X shows under the post, 21 replies X marks as probable spam, and 14 more returned earlier by the fxtwitter API that the X page did not show. X counts about 1,360 replies in total; that number also includes replies to replies, which were not read. Summaries below are paraphrased. Some replies were shown through X's automatic translation.

### The original post

| Post | What it says |
|---|---|
| [Andrej Karpathy (@karpathy), 2026-10-02](https://x.com/karpathy/status/2105819303471976479) | As LLMs do more work on their own, more of our time goes into understanding their output. He ranks output formats by how well they help: writing in ASD-STE100, then diagrams, then HTML pages, then custom explainer videos. Since code is now cheap, ask for large, disposable artifacts that never made sense to build before. |

### Prior art: similar tools found in the replies

These do the same or a similar job. We found them after building `/explain`. Look at them before choosing one.

| Reply | Tool | How it differs from `/explain` |
|---|---|---|
| [@rav4nn, 2026-10-03](https://x.com/rav4nn/status/2106282878661529836) | `/explain-better` Claude Code skill (`npx skills add rav4nn/skills -s explain-better -g`) | Picks the format for you (ASD-STE100 text, diagram, HTML or video). `/explain` always makes a page or diagram and is built around your project's code. |
| [@luongnv89, 2026-10-02](https://x.com/luongnv89/status/2105904029507211740) | Mentions two popular skills: `/show-me` and `/explain-to-me` | Not reviewed. |
| [@Yrishavjs, 2026-10-03](https://x.com/Yrishavjs/status/2106341541547798757) | lucidiff: pull request in, interactive HTML explainer out, with an ASD-STE100-style summary, flow diagram, cited risks and test checklist | Scoped to pull requests. `/explain` works on any part of a codebase. |
| [@alik_huseyn0v, 2026-10-02](https://x.com/alik_huseyn0v/status/2105989790017454106) | [danyuchn/asd-ste100-skill](https://github.com/danyuchn/asd-ste100-skill) | Writing style only. |
| [@Chrls_Hwrd, 2026-10-02](https://x.com/Chrls_Hwrd/status/2106036295415836994) | `asd-ste100-writing` skill in webrenew/skills | Writing style only. |
| [@FavioVaz, 2026-10-03](https://x.com/FavioVaz/status/2106245948502319366) | [showtime](https://github.com/FavioVazquez/showtime): local video studio for coding agents (Manim, local voice, no API keys) | Video. We point to it instead of building a video mode. |

### Replies that shaped the design

| Reply | What it says | What we did |
|---|---|---|
| [@akshaymarch7, 2026-10-02](https://x.com/akshaymarch7/status/2105864047510061223) | The biggest teaching opportunity is building the explanation around the learner's exact code, so they do not have to map a generic example back to their own. | The skill reads the current project first and uses its real names. Generic examples count as a failure. |
| [@steve_cook, 2026-10-02](https://x.com/steve_cook/status/2105839263111860358) | Says @bcherny described, in a keynote, asking Claude Code for interactive artifacts to understand a codebase or a flow. | Same use case: understanding a flow in your own codebase. |
| [@somi_ai, 2026-10-02](https://x.com/somi_ai/status/2105828320713863598) | A wrong claim narrated over a polished animation is harder to catch than a wrong sentence. | Every page ends with a Claims / Sources table, and inferred claims are marked. |
| [@ATMFL80, 2026-10-02](https://x.com/ATMFL80/status/2106098128034160651) | The clearer the output gets, the easier it is to nod along without checking it. A short plain summary next to the video helps. | Same reason for the Claims / Sources table and the short summary at the top of each page. |
| [@eliebakouch, 2026-10-02](https://x.com/eliebakouch/status/2105866163800637585) | Interactive HTML is probably one of the most time-efficient ways to understand model output. | HTML page and diagram modes were built first. |
| [@CPindil, 2026-10-03](https://x.com/CPindil/status/2106371391083655481) | Video is the least useful format for documentation: you cannot scan it, its speed is fixed, and things are hard to find inside it. | Supports keeping page and diagram as the main modes. |
| [@elliotarledge, 2026-10-02](https://x.com/elliotarledge/status/2105828285213233166) | Used the approach for a week and found it tiring. | No video pipeline was built before the cheaper formats proved useful. |
| [@originell, 2026-10-02](https://x.com/originell/status/2105879773012520966) | For code, mixing ASD-STE100 with Google's Developer Documentation Style Guide works better. | Informed the short writing rules in `SKILL.md`. |
| [@SIP200OK, 2026-10-02](https://x.com/SIP200OK/status/2105991831997526200) | Shares CLAUDE.md prose rules: Google's style guide, ASD-STE100-derived precision rules, and Zinsser's principles. | Same direction as our writing rules. |
| [@smooth_tim_, 2026-10-02](https://x.com/smooth_tim_/status/2106148099693654414) | Counterpoint: strict ASD-STE100 gives choppy, staccato sentences that are hard to read. | Our rules follow ASD-STE100 loosely, not strictly. |
| [@maxirodr_, 2026-10-02](https://x.com/maxirodr_/status/2106102689453297665) | ASD-STE100 is designed for English only; does the model keep the rules in other languages? | The skill writes in the user's language with a few general rules instead of the English-only standard. |
| [@cdruvv, 2026-10-03](https://x.com/cdruvv/status/2106477097476854083) | Several models ignored a request to write in ASD-STE100. | Another reason to state concrete rules instead of naming the standard. |

### Ideas from the replies, not used yet

| Reply | Idea |
|---|---|
| [@vl2edo, 2026-10-02](https://x.com/vl2edo/status/2105830212399407236), [@CGrajnish, 2026-10-02](https://x.com/CGrajnish/status/2105850990905979045), [@EasonZHANGZZC, 2026-10-03](https://x.com/EasonZHANGZZC/status/2106218137850949744) | Let the reader change an assumption and see what breaks, so the page becomes a small experiment. |
| [@ThisMightWrk, 2026-10-03](https://x.com/ThisMightWrk/status/2106196467950256619) | A "predict what happens next" pause, because a slick explainer can feel understood until you must answer a question. |
| [@chiraldevai, 2026-10-02](https://x.com/chiraldevai/status/2105824104175862030), [@MichaelMotorcy9, 2026-10-02](https://x.com/MichaelMotorcy9/status/2106063387356774765) | A "you lost me here" control that regenerates one section of a video more simply. |
| [@ruparel_amit, 2026-10-02](https://x.com/ruparel_amit/status/2105829118025867446) | HTML pages that store the reader's decisions so the agent can pick them up later. |
| [@thejoegardiner, 2026-10-02](https://x.com/thejoegardiner/status/2106137555519549443) | An interactive map of a whole project: what is built, maturity, test coverage, gaps. |
| [@sebastavar, 2026-10-02](https://x.com/sebastavar/status/2105829634055225517) | Enforce chosen writing rules with Vale instead of asking the model. |
| [@sukin_s, 2026-10-02](https://x.com/sukin_s/status/2105997218486554961) | Controlled language plus a diff diagram for agent approval briefs. |
| [@ljupc0, 2026-10-02](https://x.com/ljupc0/status/2105886509601579285) | Ask for a storyboard when you have half-forgotten how something works. |
| [@phinance99, 2026-10-02](https://x.com/phinance99/status/2105991384775999733) | Technical reports as PDFs via TeX. |
| [@gonzalo_io, 2026-10-02](https://x.com/gonzalo_io/status/2106146490259533832) | Excalidraw diagrams. |
| [@pawanpoolla, 2026-10-02](https://x.com/pawanpoolla/status/2105875977356312693) | ASCII diagrams for simple topics, HTML for richer ones. |
| [@humzaakhalid, 2026-10-02](https://x.com/humzaakhalid/status/2105882288550719903), [@lystic, 2026-10-03](https://x.com/lystic/status/2106244112835813799) | Manim plus local text-to-speech makes explainer videos free to produce. |

### Trade-offs raised in the replies

| Reply | Concern |
|---|---|
| [@Grady_Booch, 2026-10-02](https://x.com/Grady_Booch/status/2105864893887082857) | Diagrams for understanding software are not new; UML exists. |
| [@shibamufu, 2026-10-02](https://x.com/shibamufu/status/2105890983707816004) | HTML output costs many more tokens than text. |
| [@leopiney, 2026-10-02](https://x.com/leopiney/status/2106126337731711141) | You end up with many assets in different places that are harder to share. |

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

Skill'in çıkış noktası [Andrej Karpathy'nin 2 Ekim 2026 tarihli paylaşımı](https://x.com/karpathy/status/2105819303471976479) ve altındaki yanıtlar. 4 Ekim 2026'da 229 yanıt okundu: X'in gösterdiği 194 yanıtın hepsi, X'in olası spam olarak işaretlediği 21 yanıt ve fxtwitter API'nin döndürdüğü 14 yanıt daha. Yanıtlara verilen yanıtlar okunmadı.

Yukarıdaki **Sources** bölümünde her tweet linki ve tarihiyle birlikte listeleniyor:
- **Prior art:** Benzer işi yapan hazır araçlar (`/explain-better`, `/show-me`, `/explain-to-me`, lucidiff, showtime). Kurmadan önce bunlara da bakmanızı öneririz.
- **Replies that shaped the design:** Hangi tasarım kararının hangi tweet'ten geldiği.
- **Ideas not used yet:** Henüz uygulanmayan fikirler.
- **Trade-offs:** Yanıtlarda dile getirilen itirazlar.
