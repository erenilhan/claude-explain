# /explain

A Claude Code skill that answers "how does this work?" with a visual page grounded in your project's own code, instead of a wall of terminal text. Every claim on the page links to the `file:line` it came from.

[Türkçe](README.tr.md)

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
/explain video <topic>        # narrated explainer video
```

Example: `/explain how a bug report travels from the widget to GitHub`

When installed as a plugin, the command may also appear as `/explain:explain`. A plain request such as "walk me through this flow" triggers it too. The page is written in the language you ask in. Turkish mode names (`sayfa`, `diyagram`) also work.

## Video

`/explain video <topic>` makes a 60–90 s narrated video from the same code reading. It needs the [showtime](https://github.com/FavioVazquez/showtime) plugin, which renders locally and needs no API keys. Without showtime, the skill offers a page instead.

showtime's built-in voices have no Turkish. For Turkish narration the skill uses [Antalia-2 Mini](https://huggingface.co/cloud0day3/antalia-mini) (Apache-2.0, 7.6M parameters, runs on CPU, one synthetic male voice): `uv tool install "antalia-mini[hub]"`. The video credits the voice as synthetic, as the model's licence asks.

A video cannot be scanned, so the skill also gives you its claims file to check what the video says.

## Similar tools

Other skills do a similar job, for example `/explain-better`, `/show-me` and lucidiff. [docs/SOURCES.md](docs/SOURCES.md) compares them, and shows the post and replies behind each design choice.

## License

MIT
