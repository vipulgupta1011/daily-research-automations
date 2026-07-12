Scrape the following Reddit subreddits for top AI posts:

- r/MachineLearning — top posts

- r/artificial — top posts

- r/LocalLLaMA — hot posts

- r/OpenAI — hot posts



Use the mcp__reddit__scrape_subreddit and mcp__reddit__get_top_posts tools to fetch top 5–8 posts per subreddit by score.



---



## RECENCY FILTER — Apply this before anything else



After fetching posts, **discard any post whose `created_utc` is older than 4 days from today's date**. Only posts published within the last 4 days are eligible for the digest. It is perfectly fine to have fewer posts or skip a subreddit entirely if nothing recent and noteworthy remains — quality over quantity.



---



## DEDUPLICATION — Read this before composing the digest



**Step 1: Load sent-post history**

Check if `{{AI_DIGEST_HISTORY_PATH}}` exists. If it does, read it. It contains a JSON object like:

```json

{

  "2026-03-19": ["post_id_1", "post_id_2", ...],

  "2026-03-18": ["post_id_3", ...],

  "2026-03-17": ["post_id_4", ...]

}

```

Collect all post IDs from the **last 7 days** (i.e., the 7 most recent date keys). These are the IDs to EXCLUDE from today's digest.



If the file does not exist, the exclusion list is empty — proceed normally.



**Step 2: Filter scraped posts**

From all fetched Reddit posts, apply BOTH filters:

1. Remove any post whose `created_utc` is older than 4 days from today.

2. Remove any post whose `id` field appears in the exclusion list from Step 1.



Only posts that pass both filters are eligible.



**Step 3: Compose the digest**

Using only the filtered (recent, non-duplicate) posts, select the most noteworthy ones:

- 2–3 bullets per subreddit max, one line each

- Only include genuinely noteworthy posts (breaking news, major releases, high-engagement discussions)

- Skip low-quality or low-score posts entirely

- It's fine to skip a subreddit entirely if nothing new and noteworthy remains after filtering

- It's fine to send a short digest if only a few good posts are available — do not pad with old or mediocre posts



Use this email format:

- Subject: 🤖 Daily AI Digest — [Today's Date]

- Plain, clean HTML. No long summaries. Each bullet is one sentence max.

- Group by subreddit with a small header per section.



**Step 4: Send the email**

Use the available Gmail send-email tool to send the digest to {{RECIPIENT_EMAIL}}.



**Step 5: Save today's post IDs to history**

After sending, write the updated history back to `{{AI_DIGEST_HISTORY_PATH}}`.

- Add a new key for today's date (YYYY-MM-DD) containing an array of all post IDs included in today's digest.

- Keep only the most recent 7 date keys (drop anything older). This caps the file size.

- Write the updated JSON back to the file.



Example of what to save:

```json

{

  "2026-03-20": ["1rwl37x", "1rvhxqv", "1rughk5"],

  "2026-03-19": ["abc123", "def456"],

  "2026-03-18": ["xyz789"]

}

```
