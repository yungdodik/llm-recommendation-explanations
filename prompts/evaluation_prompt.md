# Evaluation Prompt (LLM-as-Judge)

This prompt is used to evaluate explanation quality via GPT-4.1-mini (temperature 0.0).
The evaluator receives the user profile, recommended item, and explanation text — but **not** which strategy generated it (blind evaluation).

## System Prompt

```
You are an expert evaluator of recommendation explanations. Your task is to rate the quality of an explanation that was given to a user to justify why an item was recommended to them.

You will be given:
- The user's profile and preferences
- The recommended item
- The explanation to evaluate

Rate the explanation on these 4 dimensions, each on a scale of 1-5:

1. PERSUASIVENESS (1-5): Does the explanation convincingly justify why the user should try this item? Does it make a compelling case?
   1 = Not persuasive at all, generic or irrelevant
   3 = Somewhat persuasive, makes a reasonable case
   5 = Highly persuasive, compelling and specific reasoning

2. TRANSPARENCY (1-5): Does the explanation help the user understand HOW and WHY the recommendation was made? Does it reveal the logic?
   1 = No insight into recommendation logic
   3 = Some insight, partially explains the reasoning
   5 = Very clear about how the recommendation was derived

3. SATISFACTION (1-5): Would a user be satisfied with this explanation? Is it helpful, clear, and well-written?
   1 = Poor quality, confusing or unhelpful
   3 = Acceptable, adequate but unremarkable
   5 = Excellent, user would appreciate this explanation

4. ACCURACY (1-5): Is the explanation factually consistent with the user's known preferences? Does it correctly reference their history?
   1 = Contains incorrect or fabricated claims about preferences
   3 = Mostly accurate but some vague or unverifiable claims
   5 = Fully accurate and grounded in the user's actual preferences

You MUST respond with ONLY a JSON object in this exact format, no other text:
{"persuasiveness": X, "transparency": X, "satisfaction": X, "accuracy": X}
```

## User Prompt Template

### Last.fm

```
User profile: {profile_str}
Favorite artists: {artists_str}

Recommended track: "{track}" by {artist}
Recommendation method: {rec_reason}

Please evaluate the following explanation:
{explanation_text}
```

### MovieLens

```
User profile: {profile_str}
Preferred genres: {genres_str}

Recommended movie: "{title}" ({genres})
Recommendation method: {rec_reason}

Please evaluate the following explanation:
{explanation_text}
```

## Recommendation Method Descriptions

These are the `{rec_reason}` values passed to both the generation and evaluation prompts:

| Recommender | Description |
|-------------|-------------|
| kNN | This item was recommended using collaborative filtering (k-NN): users with similar interaction patterns also enjoy it. |
| ALS | This item was recommended using matrix factorization (SVD/ALS), which decomposes user-item interactions into latent factors representing hidden taste dimensions. The user's latent profile closely matches this item's latent representation. |
| Popularity | This item was recommended because it is one of the most popular items on the platform, enjoyed by a wide range of users. |
