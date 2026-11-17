# colab-experiments — DeOldify colourisation notebook export

A holding repo for standalone Google Colab experiments. Its only content is a Colab-exported script,
`deoldify.py`, that installs [DeOldify](https://github.com/jantic/DeOldify) (jantic), downloads the
"artistic" colouriser weights, and colourises one image given by URL inside a Colab session. There is no
package, no tests, and no data; the output is an image shown in the notebook.

> Status: archived snapshot (notebook export only). `deoldify.py` is a verbatim Colab `.ipynb` → `.py` export
> and contains `!pip` / `!git clone` / `%cd` cell magics, so it is **not** runnable as a plain Python script —
> it only works pasted back into a Colab notebook.

<!-- how-to-use-with-ai -->
> **New here?** See [How to use this repo with AI (Claude, Gemini, ChatGPT)](HOW_TO_USE_WITH_AI.md) — clone it, paste it into an AI tool, and ask in plain English.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/herrrickshaw/colab-experiments/blob/main/notebooks/colab_test.ipynb)

## What it does
- Clones `jantic/DeOldify` and installs its `colab_requirements.txt` plus the `deoldify` package (and, for the "local" variant, pinned `fastai==1.0.61`, `fastprogress==0.2.7`, `torch==1.7.0`, `torchvision==0.8.1`).
- Warns if no CUDA GPU is available (`torch.cuda.is_available()`).
- Patches `PIL.PILLOW_VERSION` (removed in modern Pillow) so DeOldify's import does not fail.
- Downloads `ColorizeArtistic_gen.pth` from `https://data.deepai.org/deoldify/` into `./models/`.
- Builds `get_image_colorizer(artistic=True)` and calls `plot_transformed_image_from_url()` with Colab form parameters `source_url`, `render_factor` (7–40, default 21) and `watermarked` (default `False`), showing a side-by-side comparison.

## Quick start
Open the Colab notebook that the original export came from (link in the header of `deoldify.py`), or paste the
cells of `deoldify.py` into a new Colab notebook with a GPU runtime, set `source_url` to a direct image URL,
and run all cells. There is nothing to install locally.

`notebooks/colab_test.ipynb` is a generic generated scaffold (clone repo → install → `py_compile` every `.py` →
list entry points). Because `deoldify.py` contains cell magics, its compile step reports a syntax error; that is
expected and not a defect in DeOldify.

## Repository layout
| Path | Purpose |
|---|---|
| `deoldify.py` | Colab export of the DeOldify colourisation notebook (not plain-Python runnable) |
| `from-global-market-data/deoldify.py` | identical copy imported from the `global-market-data` repo history during a 2026-09 declutter (differs only by a trailing newline) |
| `notebooks/colab_test.ipynb` | generated Colab smoke-test scaffold (see above) |
| `HOW_TO_USE_WITH_AI.md` | plain-language guide for non-programmers |

## Data sources
| Source | What is taken | Access route | Licence / caveat |
|---|---|---|---|
| `jantic/DeOldify` on GitHub | code + `colab_requirements.txt` | `git clone` at runtime | MIT (DeOldify) |
| `https://data.deepai.org/deoldify/ColorizeArtistic_gen.pth` | pretrained artistic generator weights | `wget` at runtime | third-party host; availability not guaranteed |

## Configuration
Three Colab form fields at the bottom of `deoldify.py`: `source_url` (string), `render_factor` (slider 7–40),
`watermarked` (bool). No environment variables, no secrets.

## Limitations and known issues
- The committed default `source_url` is a YouTube playlist link, not an image; replace it with a direct image URL or the colouriser will fail.
- Pins (`torch==1.7.0`, `fastai==1.0.61`) are from 2020-era DeOldify and will not install on current Colab Python versions without changes; the Colab-path `colab_requirements.txt` install is the one that is expected to work.
- Two copies of the same script exist (see layout); keep them in sync or delete one.
- No `requirements.txt`, tests, or CI.

## Related repositories
- `global-market-data` (same account) — original home of `deoldify.py` before the 2026-09 declutter.

## Licence
No licence file yet — all rights reserved for this repo's own files; DeOldify itself is MIT-licensed.
