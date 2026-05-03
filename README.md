# StatsAgent
## Multi-Agent System for Statistical Data Analysis and Clinical Trials 


## Clone this repository:
```bash
git clone git@github.com:dylan-devs/stats-agent.git
cd stats-agent
```

## Prerequisites 
We use `uv` for Python environment + dependency management. Install uv [here](https://docs.astral.sh/uv/getting-started/installation/).

## Langgraph Agent


1. Navigate to the `agent` directory:
```bash
cd agent
```

2. Create a `.env` file.

```bash
cp .env.example .env
```

### Enter API Keys
Open `.env` and enter API keys as needed

#### Anthropic

To use Anthropic's chat models:

1. Sign up for an [Anthropic API key](https://console.anthropic.com/) if you haven't already.
2. Once you have your API key, add it to your `.env` file:

```env
ANTHROPIC_API_KEY=your-api-key
```
#### OpenAI

To use OpenAI's chat models:

1. Sign up for an [OpenAI API key](https://platform.openai.com/signup).
2. Once you have your API key, add it to your `.env` file:
```env
OPENAI_API_KEY=your-api-key
```

#### Tavily Search

To use the search tool:
1. Sign up for an [Tavily API key](https://app.tavily.com/sign-in).
2. Once you have your API key, add it to your `.env` file:
```
TAVILY_API_KEY=your-api-key
```

#### E2B

To use the code execution tool:
1. Sign up for an [E2B API key](https://e2b.dev/sign-in).
2. Once you have your API key, add it to your `.env` file:
```
E2B_API_KEY=your-api-key
```

### Configure LLM Providers and Models:

#### Supported Providers:

- openai
- anthropic
- ollama
- unsloth

#### Default Provider/Model:

The current __default__ model is:

```
openai/gpt-5-nano-2025-08-07
```

#### Update Provider/Model 

To __switch__ the model, update the `.env` file:

```
MODEL={provider}/{model}
```

For example, to use an local model served via `ollama`:

```
MODEL=ollama/qwen3:8b
```

#### Using Local Models:

This project supports local models via `ollama` and `llama.cpp`

The `unsloth` provider tag is used for locally hosted models served via llama.cpp.

Remember not all ollama models have tool capabilities!
Check the [Ollama Documentation](https://docs.ollama.com/) for details.


### Start the development agent server

1. Install the packages
```bash
uv sync
```

2. Start the server
```bash 
uv run langgraph dev
```

## Firebase for File Storage
1. Install Node: [Link](https://nodejs.org/en/download)
2. Install Firebase CLI (recommend using NPM): [Link](https://firebase.google.com/docs/cli)
3. Log in: 
```
firebase login
```
4. Install dependencies (need to be in `services/functions`):
```
cd services/functions
```

```
npm install
```
5. Build project
```
npm run build
```

6. Start emulators (need to be `services`):
```
cd ..
```
```
firebase emulators:start
```

## Frontend (Flutter Web)

### 1. Install Flutter 
Install flutter by following the instructions [here](https://docs.flutter.dev/install)

Verify installation:
```bash
flutter doctor
```

### 2. Run the Web App
1. Navigate to the `app` directory:
```bash
cd app
```

2. Create a `.env` file.

```bash
cp .env.example .env
```

3. Install packages
```bash
flutter pub get
```

4. Run the Flutter app and target the Chrome device (web).
```bash
flutter run -d chrome
```
This launches the Flutter web app and connects to the local agent server.

## TableBench evaluation

We evaluate the agent on the **TableBench** *direct-prompt* (`DP`) split using the workflow from the [official TableBench repository](https://github.com/TableBench/TableBench).

1. Download the dataset (from the **project root**, so files land in `./datasets/`):

```bash
uvx hf download Multilingual-Multimodal-NLP/TableBench TableBench_DP.jsonl --repo-type dataset --local-dir ./datasets
```

If that Hub path or filename changes, use the dataset linked from TableBench's dataset card on HuggingFace.

2. Run our inference script (writes JSONL with `model_name` and `prediction`):

```bash
cd agent
uv sync
uv run tablebench-inference
```

Use `--input` / `--output` to override paths. Defaults: input `../datasets/TableBench_DP.jsonl`, output `../datasets/tablebench_inference_<model>.jsonl`.

3. Clone TableBench and install its dependencies:

```bash
git clone https://github.com/TableBench/TableBench.git
cd TableBench
uv pip install -r requirements.txt
```

4. Copy your inference JSONL into the TableBench repository and run their parse and evaluation scripts as described in their README.
