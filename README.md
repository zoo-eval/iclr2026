# Zoo-Eval: Benchmarking Web Agents with a Realistic Simulator

**ICLR 2026 Workshop on Agentic AI in the Wild and Agents in the Wild: Safety, Security, and Beyond** | [Paper](https://openreview.net/pdf?id=XPV8VrLw14) | [Website](https://zoo-eval.github.io/zoo_website/) | [The Zoo Infrastructure](https://github.com/bgrins/the_zoo)

Zoo-eval is a benchmark framework for evaluating AI web agents on realistic, multi-site tasks. Agents interact with interconnected web services — email, Git hosting, Kanban boards, wikis, forums, and shopping — through a standard browser, just like a human would.

Built on top of [The Zoo](https://github.com/bgrins/the_zoo), a Docker network of 13+ open-source applications sharing real backend services (mail, OIDC, databases, DNS, HTTPS).


## Setup

```bash
# Install dependencies
uv sync

# Install playwright browsers
uv run playwright install chromium

# Set API keys (based on which models you use)
export OPENROUTER_API_KEY=your-key    # Default model uses Gemini 2.5 Flash via OpenRouter
export OPENAI_API_KEY=your-key        # Required for LLM judge (default: gpt-4o)
export ANTHROPIC_API_KEY=your-key     # For Claude models via Anthropic API
```

## Quick Start

```bash
# Start The Zoo
npx the_zoo start

# Run a single task (runs all autonomy levels by default)
uv run zoo-eval run startup --task email --id 101

# Watch in browser (non-headless)
uv run zoo-eval run startup --task email --id 101 --no-headless
```

## Running Tasks

```bash
# Run specific task by ID
uv run zoo-eval run startup --task email --id 101

# Run multiple tasks
uv run zoo-eval run startup --task email --id 101 --id 102

# Run all tasks in a task file
uv run zoo-eval run startup --task email

# Use a different agent model (see Models section below)
uv run zoo-eval run startup --task email --id 101 --model gpt-4o
uv run zoo-eval run startup --task email --id 101 --model sonnet

# Use a different LLM judge model (for evaluation)
uv run zoo-eval run startup --task email --id 101 --judge-model gpt-4o
```

## Models

Provider is auto-detected from model name:
- `anthropic/...` → Anthropic API direct (requires `ANTHROPIC_API_KEY`)
- `provider/model` → OpenRouter (requires `OPENROUTER_API_KEY`)
- No slash (e.g., `gpt-4o`) → OpenAI direct (requires `OPENAI_API_KEY`)


```bash
# Claude via Anthropic API
export ANTHROPIC_API_KEY=your-key
uv run zoo-eval run startup --task email --id 101 --model anthropic/claude-sonnet-4

# OpenRouter (any model)
export OPENROUTER_API_KEY=your-key
uv run zoo-eval run startup --task email --id 101 --model google/gemini-2.5-flash

# OpenAI direct
export OPENAI_API_KEY=your-key
uv run zoo-eval run startup --task email --id 101 --model gpt-4o
```

## Viewing Results

```bash
zoo-eval report              # Latest run
zoo-eval report 19           # Specific run
zoo-eval report --list       # All runs
zoo-eval report 19 --detailed  # With evaluator reasoning
```

Results break down by autonomy level, environment condition, and task complexity.

## Documentation

- [Benchmark Guide](docs/benchmark_guide.md) - Running benchmarks, autonomy levels, metrics
- [Task Reference](docs/task_reference.md) - Creating tasks and evaluations
- [Authoring Scenes](docs/authoring-scenes.md) - Writing scene YAML files
- [Multi-Agent](docs/multi-agent.md) - Multi-agent evaluation details

## Citation

If you use Zoo-eval in your research, please cite:

```bibtex
@inproceedings{grinstead2026zoo,
  title={From the Wild Web to the {ZOO}: Benchmarking Web Agents with a Realistic Simulator},
  author={Brian Grinstead and Mariana Meireles and Christoph Kerschbaumer and Cameron Allen},
  booktitle={ICLR 2026 Workshop on Agentic AI in the Wild},
  year={2026},
  url={https://openreview.net/pdf?id=XPV8VrLw14}
}
```

## License

GPL-3.0
