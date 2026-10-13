# manim skill

An agent skill that helps you write animations with Manim Community Edition (ManimCE).
It covers install, render commands, scenes, Mobjects, animations, layout, updaters, and camera control.
It does not cover ManimGL.

## Install

Clone the repository into your skills folder:

```bash
git clone https://github.com/<owner>/manim-skill ~/.claude/skills/manim
```

Or use `npx skills`:

```bash
npx skills add <owner>/manim-skill
```

## Requirements

- Python 3.9 or newer
- `pip install manim`
- ffmpeg
- A LaTeX distribution (TeX Live, MacTeX, or MiKTeX) for `MathTex` and `Tex`

## Use

Ask your agent to "animate" an idea with manim. See [examples/prompts.md](examples/prompts.md) for sample prompts.

## License

MIT. See [LICENSE](LICENSE).
