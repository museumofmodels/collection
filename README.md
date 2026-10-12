---
license: cc-by-4.0
pretty_name: Museum of Models collection
language:
- en
tags:
- ai-history
- model-outputs
- language-models
- retired-models
- svg
- html
size_categories:
- 10K<n<100K
configs:
- config_name: answers
  data_files: data/answers/*.jsonl
  default: true
- config_name: models
  data_files: data/models.jsonl
- config_name: questions
  data_files: data/questions.jsonl
---

# Museum of Models collection

Every answer every model gave to the same questions, kept after the models are gone.

https://github.com/user-attachments/assets/f1b7d068-54f7-4304-8636-9427068b2274

18,262 answers from 378 models to 100 questions, asked between 2024-05-23 and 2026-10-12. 101 of the models are retired: OpenRouter no longer serves them, and unless their weights were published nobody can ask them anything again. Their answers are here.

This is the collection behind [museumofmodels.com](https://museumofmodels.com), as published there. Version 1.2.0. DOI [10.5281/zenodo.23305275](https://doi.org/10.5281/zenodo.23305275) for every version, resolving to the newest; each version has its own on Zenodo.

## What is in it

| Kind | Answers | Kept as |
|---|---:|---|
| Drawings | 2,145 | the model's full reply, which holds an SVG |
| Websites | 5,025 | the model's HTML, plus a screenshot |
| Images | 1,023 | the image file the model returned |
| Writing | 9,799 | the model's text, usually Markdown |
| Code | 186 | the model's text |
| Voices and music | 84 | the audio file the model returned |

The questions are things like: draw a pelican riding a bicycle as an SVG, build a landing page in one HTML file, tell five jokes, name a favourite film. Each was put to every model the same way. The collection is meant for looking at how models answered over time, not for ranking them. There are no scores in it.

418 answers are empty or hold no content (mostly reasoning models that spent their whole budget thinking). They are kept and marked `empty`.

Every answer is kept as the model returned it, except:

- 1 answer was stored with the API's response record around it (`gpt-4.1-futuristic-prediction`); only the answer is kept.
- 19 PNG images from Tencent carried a content label naming the account that asked; that metadata was removed. The images themselves are unchanged.

## Files

```
data/questions.jsonl          one line per question
data/models.jsonl             one line per model
data/answers/<question>.jsonl one line per answer, oldest first
media/images/, media/audio/   the image and audio answers, named by answer id
screenshots/                  each website answer rendered at 1280x800, named by answer id
SHA256SUMS                    a checksum for every file
```

**Answer** (`data/answers/*.jsonl`)

| Field | |
|---|---|
| `id` | the answer's id, stable across versions |
| `question`, `model` | ids into the other two files |
| `model_name`, `lab` | repeated here for convenience |
| `type` | `svg`, `website`, `image`, `text`, `code` or `audio` |
| `prompt` | the exact prompt this answer was given. A few questions were worded slightly differently over time; each answer keeps its own |
| `prompt_note` | set when the original request could not be verified |
| `date` | the day it was asked, as the museum shows it |
| `recorded_date` | the date stamped on the record. A few older answers were stamped before their model existed; for those `date` is the model's release date |
| `content` | the answer as given, for every type except image and audio. Drawings are the full reply: the SVG is inside it, sometimes with prose around it, and some older records store it with literal `\"` and `\n` escapes |
| `file` | the image or audio file, for those types |
| `screenshot` | the website answer rendered in Chrome at 1280x800, light mode, after the page settled. Pages that need a click to draw anything come out blank |
| `empty` | true when the answer holds nothing |
| `url` | the answer on museumofmodels.com |

**Model** (`data/models.jsonl`): `id`, `name`, `lab`, `code_name` (OpenRouter's stealth models, released under an invented name), `revealed_as` (what a code name turned out to be, where its maker said so), `released`, `status`, `last_seen`, `answers`, `url`.

`status` is `retired` when OpenRouter no longer lists the model under any name, with `last_seen` the last day it did; `live` when it still does; `unknown` when the model was asked through its lab's own API, or appeared after the last availability check.

**Question** (`data/questions.jsonl`): `id`, `title`, `prompt` (the usual wording), `type` (what most answers are), `answers`, `url`.

```python
from datasets import load_dataset

answers = load_dataset("json", data_files="data/answers/*.jsonl", split="train")
```

## How the answers were asked

The answers were collected with the asking method Rival published at [rival.tips/methodology](https://rival.tips/methodology), summarised here so the record does not depend on that page:

- **The same question, word for word.** Each model got the prompt as a single user message, with no system prompt and no conversation before it.
- **One attempt.** The first answer a model gave is the one kept. Nothing was asked again, picked or corrected.
- **Ordinary settings.** Through OpenRouter or the lab's own API, at a temperature of 0.7 (a few models only run at their default), top_p 1, no penalties, no seed, and room for an answer of any length.
- **Dated the day it was asked.**

Questions answered by fewer than five models are left out.

## License

[Creative Commons Attribution 4.0 International](LICENSE) (CC BY 4.0). You may copy, share, adapt and build on the collection for any purpose, commercial ones included, as long as you credit it (below), link to the license, and say if you changed anything.

The license covers what the museum made: the questions, the selection and arrangement of the collection, the metadata and dates, the screenshots and this documentation. The answers were made by the models. Whether anyone holds copyright in machine-made text and images is unsettled and differs from country to country. To the extent the museum holds any rights in the answers, it releases them under the same license. The terms of the provider that served a model may still apply to how its outputs are used.

Answers are kept as they were given. Some are wrong, some are odd, a few may be offensive. Keeping them is not endorsing them. The questions contain no personal data.

## How to cite

Museum of Models. (2026). *Museum of Models collection* (Version 1.2.0) [Data set]. https://doi.org/10.5281/zenodo.23305275

```bibtex
@misc{museum_of_models_collection,
  title        = {Museum of Models collection},
  author       = {{Museum of Models}},
  year         = {2026},
  version      = {1.2.0},
  doi          = {10.5281/zenodo.23305275},
  url          = {https://doi.org/10.5281/zenodo.23305275},
  note         = {CC BY 4.0. https://museumofmodels.com}
}
```

## Changes

Each version is a snapshot of the collection on the day it was published. Answer ids do not change between versions; new models and questions are added, and corrections are listed here.

- **1.2.0** (2026-10-12): 2354 new answers; 18,262 answers from 378 models in all. Thirteen new questions bring the collection to 100: a self-portrait, a clock at 3:45 and the Mona Lisa as SVG, a playable piano, and who made you, today's date, the best AI model, a letter to its successor, the first line of a novel, letters in strawberry, the farmer and the goat, a random number and a favourite painting. They were asked on 2026-10-12 of every text model OpenRouter served that day; models retired before then never answered them.
- **1.1.0** (2026-10-12): 103 new answers and 1 new model (GPT-4 Turbo); 15,908 answers from 378 models in all. OpenAI removes GPT-4, GPT-4 Turbo, GPT-3.5 Turbo, o1, o3-mini, o4-mini and GPT-4.1 nano from its API on 2026-10-23. Before that, each was asked the current questions it had not answered, and GPT-4 Turbo was added. DOI 10.5281/zenodo.23310442.
- **1.0.1** (2026-10-11): Moved to github.com/museumofmodels/collection, the museum's own organization; the README and citation files carry the DOI. The data is unchanged. DOI 10.5281/zenodo.23306049.
- **1.0.0** (2026-10-11): First public version. DOI 10.5281/zenodo.23305276.
