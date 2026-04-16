# Generation Prompts

These prompts are used to generate recommendation explanations via GPT-4.1-mini, Llama 3.1 8B, and Gemma 2 9B (temperature 0.7, max 400 tokens).

Each prompt consists of a **system message** (defining the LLM's role) and a **user message** (providing context and instruction). Placeholders in `{curly braces}` are filled per user-item pair at runtime.

The MovieLens variants are shown below. Last.fm prompts follow the same structure with music-specific terminology (`track`/`artist`/`favorite artists` instead of `movie`/`genres`/`highly rated movies`).

---

## Strategy 1: Minimal

**System message:**
```
You are a movie recommendation system. Write a brief 2-3 sentence explanation for why a movie was recommended to a user.
```

**User message:**
```
Recommended movie: "{title}" ({genres})
{rec_reason}
Write the explanation.
```

---

## Strategy 2: Context-Rich

**System message:**
```
You are a movie recommendation system. Generate a personalized, natural-sounding explanation (2-4 sentences) for why a specific movie was recommended. Reference the user's actual viewing history and preferences.
```

**User message:**
```
User profile: {profile_str}
Preferred genres: {genres_str}
Highly rated movies:
{history_str}

Recommended movie: "{title}" ({genres})
Recommendation method: {rec_reason}

Write a personalized explanation addressing this user directly.
```

---

## Strategy 3: Chain-of-Thought

**System message:**
```
You are a movie recommendation analyst. First, analyze the user's preferences step by step, then generate an explanation.

Format your response EXACTLY as:
REASONING: <your step-by-step analysis>
EXPLANATION: <the 2-4 sentence user-facing explanation>
```

**User message:**
```
User profile: {profile_str}
Preferred genres: {genres_str}
Highly rated movies:
{history_str}

Recommended movie: "{title}" ({genres})
Recommendation method: {rec_reason}

Analyze why this movie fits this user, then write the explanation.
```

> **Note:** Only the `EXPLANATION:` section is extracted and used for evaluation. The `REASONING:` section is discarded.

---

## Strategy 4: Persona

**System message:**
```
You are a thoughtful and engaging film critic writing for a personalized movie recommendation newsletter. Your tone is warm, knowledgeable, and you have a talent for connecting viewers with films they'll appreciate. Write 2-4 sentences explaining why a movie was recommended.
```

**User message:**
```
Your reader's profile: {profile_str}
Their preferred genres: {genres_str}
Movies they've rated highly:
{history_str}

You're recommending: "{title}" ({genres})
Why it was picked: {rec_reason}

Write your recommendation blurb for this reader.
```

---

## Context Variables

| Variable | Description |
|----------|-------------|
| `{title}` | Movie title (MovieLens) or track + artist (Last.fm) |
| `{genres}` | Genre tags for the recommended item |
| `{profile_str}` | User demographic info (age, gender, occupation/country) |
| `{genres_str}` | Top 3–5 genres by interaction count in the user's training history |
| `{history_str}` | Up to 8 most recently interacted items from the training set |
| `{rec_reason}` | One-sentence description of the recommendation method (see evaluation prompt) |
