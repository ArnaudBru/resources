# ML training — decision-guide one-pagers

Practical reference pages for decisions you make when training a model: hardware selection, data pipeline shape, and similar engineering choices.

All pages here are self-contained HTML — no JS, no CDN, works offline. Built using the [one-pager-protocol skill](../skills/one-pager-protocol/).

## Contents

| Page | Rendered | Question it answers |
|---|---|---|
| `choosing_an_instance.html` | [open](https://arnaudbru.github.io/resources/ml-training/choosing_an_instance.html) | Given a model + batch + workload, what's the cheapest box that finishes in tolerable wall clock? Walks through VRAM, sys RAM, compute regime (GPU-bound vs dataloader-bound), and cost lenses. |
| `choosing_dataset_patterns.html` | [open](https://arnaudbru.github.io/resources/ml-training/choosing_dataset_patterns.html) | Which of the six PyTorch dataset patterns (in-memory, lazy map-style, pre-decoded `.pt`, sharded WebDataset, pre-extracted features, IterableDataset) fits a given workload? Decision-guide with comparison table. |

## How to read these

The hosted links above render the HTML directly in your browser — no setup. If you've cloned the repo, you can also open the local files (`firefox <file>` or just double-click). They're designed to be scannable on-screen during debugging and good for occasional reference.

The instance-selection page mentions sys RAM consumption of the DataLoader queue — that's where it overlaps with the dataset-patterns page. Reading both gives a full picture of "how does my training pipeline shape memory and throughput."
