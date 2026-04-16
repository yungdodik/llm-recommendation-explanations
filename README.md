# LLM-Generated Explanations for Recommender Systems

Code and resources accompanying the paper:

> **Evaluating LLM-Generated Explanations for Recommender Systems: 
> A Multi-Strategy Prompt Comparison Across Domains**  
> Hlib Oshchepkov, Johannes Kepler University Linz, 2026

## Overview

This repository contains the prompts, evaluation framework, and 
analysis code used to compare four LLM prompting strategies for 
post-hoc recommendation explanation generation across two datasets 
(Last.fm 1K, MovieLens 100K) and three generator models 
(GPT-4.1-mini, Llama 3.1 8B, Gemma 2 9B).

## Repository Structure

- `prompts/` — All prompts used for generation and evaluation
- `code/` — Jupyter notebook with full pipeline
- `paper/` — LaTeX source of the paper

## Requirements

Install dependencies with:

```bash
pip install -r requirements.txt
```

For open-source model inference, install [Ollama](https://ollama.com/download/windows) and pull the models:

```bash
ollama pull llama3.1:8b
ollama pull gemma2:9b
```

## Datasets

- [Last.fm 1K](http://ocelma.net/MusicRecommendationDataset/lastfm-1K.html)
- [MovieLens 100K](https://grouplens.org/datasets/movielens/100k/)

## Models

Explanation generation uses the OpenAI API (GPT-4.1-mini) and 
Ollama for local inference (Llama 3.1 8B, Gemma 2 9B).
