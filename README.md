# LIUS Cookbook

> **Daoism-Qwen3.5-9B + 95k Knowledge RAG + Public API — Official "How-We-Use" Walkthroughs**
> Maintained by **Dingren Daoxue Lab** · License: **CC0 1.0**

This repo contains the official recipes for using the [Daoism-Qwen3.5-9B](https://huggingface.co/lius-cc/Daoism-Qwen3.5-9B) model, the [daoism-knowledge-rag](https://huggingface.co/datasets/lius-cc/daoism-knowledge-rag) dataset (95,919 nodes), and the public RAG API at [lius.cc/api/llm-rag](https://lius.cc/api/llm-rag) (free, 30 req/min).

Every notebook here mirrors a public demo on [lius.cc/llm/demos](https://lius.cc/llm/demos). Clone the recipe, change one line of prompt, and you've built a new research tool.

> **v0.1 preprint — not peer-reviewed.** Academic citation requires the upcoming Zenodo DOI (planned D30) and subsequent journal-version review.

---

## Notebooks

| # | Notebook | Live Demo | Topic |
|---|---|---|---|
| 00 | [how-we-use-rag-api.ipynb](notebooks/how-we-use-rag-api.ipynb) | — | RAG API: 30 lines of Python |
| 01 | [how-we-use-model.ipynb](notebooks/how-we-use-model.ipynb) | — | Local model: LM Studio / Ollama / vLLM |
| 02 | [how-we-use-dataset.ipynb](notebooks/how-we-use-dataset.ipynb) | — | Offline RAG with bge-m3 + FAISS |
| 03 | [how-we-read-canon.ipynb](notebooks/how-we-read-canon.ipynb) | [▶ #5](https://lius.cc/llm/demos/canon-reader) | Dao De Jing tri-column reader |
| 04 | [how-we-build-literature-map.ipynb](notebooks/how-we-build-literature-map.ipynb) | [▶ B](https://lius.cc/llm/demos/literature-map) | 9,200 papers + BibTeX export |
| 05 | [how-we-build-deity-guide.ipynb](notebooks/how-we-build-deity-guide.ipynb) | [▶ C](https://lius.cc/llm/demos/deity-guide) | Deity functions + sacred sites |
| 06 | [how-we-compare-terms.ipynb](notebooks/how-we-compare-terms.ipynb) | [▶ D](https://lius.cc/llm/demos/term-compare) | Cross-sect terminology diff |
| 07 | [how-we-trace-citations.ipynb](notebooks/how-we-trace-citations.ipynb) | [▶ E](https://lius.cc/llm/demos/citation-trace) | Citation trace + ABCD grading |
| 08 | [how-we-build-ritual-flow.ipynb](notebooks/how-we-build-ritual-flow.ipynb) | [▶ A5](https://lius.cc/llm/demos/ritual-flow) | Ritual flow + sect difference |
| 09 | [how-we-build-temple-bot.ipynb](notebooks/how-we-build-temple-bot.ipynb) | [▶ A1](https://lius.cc/llm/demos/temple-bot) | Temple chat bot + red-line guard |
| 10 | [how-we-build-worship-etiquette.ipynb](notebooks/how-we-build-worship-etiquette.ipynb) | [▶ C1](https://lius.cc/llm/demos/worship-etiquette) | Curated worship lookup (no AI) |
| 11 | [how-we-build-deity-mbti.ipynb](notebooks/how-we-build-deity-mbti.ipynb) | [▶ C4](https://lius.cc/llm/demos/deity-mbti) | 12-Q personality test → 16 deity outcomes (no AI) |
| 12 | [how-we-build-daily-deity.ipynb](notebooks/how-we-build-daily-deity.ipynb) | [▶ C2](https://lius.cc/llm/demos/daily-deity) | Deterministic daily deity + quote card (no server) |

Each notebook has an **Open in Colab** button at the top.

---

## 30-second Quickstart

```python
import requests

r = requests.post(
    "https://lius.cc/api/llm-rag",
    json={"q": "三朝醮", "n": 5},
    timeout=10,
).json()

for hit in r["hits"]:
    print(f"[{hit['type']}] {hit['name']} — score {hit['score']}")
    print(f"  {hit['url']}")
    print(f"  {hit['summary'][:80]}…")
```

---

## How to cite

```bibtex
@misc{lius-cookbook-2026,
  author = {Liu, Chi-Ying and Dingren Daoxue Lab},
  title  = {LIUS Cookbook: Official How-We-Use Walkthroughs for the Open-Source Daoism LLM},
  year   = {2026},
  publisher = {GitHub},
  url    = {https://github.com/lius-cc/lius-cookbook},
  license = {CC0-1.0}
}

@misc{daoism-qwen3-9b-2026,
  author = {Liu, Chi-Ying and Dingren Daoxue Lab},
  title  = {Daoism-Qwen3.5-9B: An Open-Source Taoist Knowledge Language Model},
  year   = {2026},
  publisher = {HuggingFace},
  url    = {https://huggingface.co/lius-cc/Daoism-Qwen3.5-9B}
}
```

---

## Public-first evidence stack

This work is released under a "**publish first, monopolize through evidence**" strategy:

- **GitHub commits** — immutable timestamps (this repo)
- **Zenodo DOI** — citable archive (planned D30)
- **Wayback Machine** — weekly snapshots of lius.cc/llm pages

Formal peer-reviewed version will follow via journal submission (Religion & Computing / Journal of Chinese Religions / DHQ).

See [DaoEval Whitepaper v0.1](https://lius.cc/llm/whitepaper) for the full evaluation framework, 500-sample JSONL schema, and ABCD confidence grading.

---

## License

- **Code & notebooks**: [CC0 1.0](LICENSE) (public domain dedication)
- **Underlying model**: Apache 2.0
- **Underlying dataset**: Apache 2.0 (metadata CC0)
- **API**: 30 req/min, free, attribution appreciated

---

## Contact

- 🐛 Issues: [GitHub Discussions](https://github.com/lius-cc/lius-cookbook/discussions) or [HF Discussions](https://huggingface.co/lius-cc/Daoism-Qwen3.5-9B/discussions)
- 📧 chiyingliu@gmail.com
