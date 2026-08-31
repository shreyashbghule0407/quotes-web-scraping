# 📜 Quotes to Scrape — Web Scraping Project

A beginner-friendly web scraping project that extracts quotes, authors, tags, and author profile links from [quotes.toscrape.com](https://quotes.toscrape.com/) using **Python, Requests, and BeautifulSoup**, and structures the data into a clean CSV using **Pandas**.

The project is built as a progressive series of notebooks — starting from a raw HTML fetch and ending with a fully paginated, structured dataset of 100 quotes.

---

## 📁 Project Structure

```
Quotes-Web-Scraping/
│
├── Page_Scrapper.ipynb                        # Step 1: Fetch raw HTML and save locally
├── Scrapping_Quotes.ipynb                      # Step 2: Parse HTML, extract quote text
├── Scrapping_Quotes_with_Author_Details.ipynb  # Step 3: Extract quote + author + tags -> DataFrame
├── Scrapping_Data_from_multiple_Pages.ipynb    # Step 4: Loop across all pages -> final dataset
├── main.html                                   # Saved raw HTML snapshot of page 1
├── Quotes.csv                                  # Final structured dataset (100 quotes)
├── requirements.txt
├── .gitignore
└── README.md
```

## 🔄 Project Flow

| Step | Notebook | What it does |
|------|----------|---------------|
| 1 | `Page_Scrapper.ipynb` | Sends a `requests.get()` call to the site and writes the raw HTML to `main.html` — useful for offline inspection of page structure. |
| 2 | `Scrapping_Quotes.ipynb` | Parses the HTML with `BeautifulSoup` and extracts just the quote text from all `<span class="text">` elements on page 1. |
| 3 | `Scrapping_Quotes_with_Author_Details.ipynb` | Extends the scrape to pull `quote`, `author`, `author_id` (profile link), and `tags` for each quote on page 1, and loads them into a Pandas DataFrame. |
| 4 | `Scrapping_Data_from_multiple_Pages.ipynb` | Loops through all 10 pages (`/page/1` → `/page/10`) using `tqdm` for a progress bar, collects every quote, adds a full `author_link` column, and exports everything to `Quotes.csv`. |

## 📊 Dataset (`Quotes.csv`)

| Column | Description |
|---|---|
| `quote` | The quote text |
| `author` | Author's name |
| `author_id` | Relative URL path to the author's profile (e.g. `/author/Albert-Einstein`) |
| `tags` | Comma-separated tags associated with the quote |
| `author_link` | Full URL to the author's profile page |

**Size:** 100 rows × 5 columns, spanning 50 unique authors across 10 pages.

## 🛠️ Tech Stack

- **Python 3**
- [`requests`](https://pypi.org/project/requests/) — HTTP requests
- [`beautifulsoup4`](https://pypi.org/project/beautifulsoup4/) — HTML parsing
- [`pandas`](https://pypi.org/project/pandas/) — data structuring & CSV export
- [`tqdm`](https://pypi.org/project/tqdm/) — progress bars for the multi-page loop
- Jupyter Notebook

## ▶️ How to Run

1. Clone the repo (see GitHub flow below).
2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```
3. Open any notebook in Jupyter:
   ```bash
   jupyter notebook
   ```
4. Run `Scrapping_Data_from_multiple_Pages.ipynb` top to bottom to regenerate `Quotes.csv` from scratch, or explore the earlier notebooks to see how the approach evolved step by step.

## 🌐 Target Website

This project scrapes **[quotes.toscrape.com](https://quotes.toscrape.com/)**, a sandbox site built by Zyte specifically for practicing web scraping — it's free to scrape and designed for exactly this kind of learning project.

---

## 🚀 Uploading to GitHub

From your project folder (with all the notebooks, `main.html`, `Quotes.csv`, `README.md`, `requirements.txt`, `.gitignore` inside it):

```bash
# 1. Initialize git in the project folder
git init

# 2. Add a remote (create the empty repo on GitHub first, then copy its URL)
git remote add origin https://github.com/<your-username>/<repo-name>.git

# 3. Stage all files
git add .

# 4. Commit
git commit -m "Initial commit: Quotes web scraping project"

# 5. Rename branch to main (if needed) and push
git branch -M main
git push -u origin main
```

**Day-to-day updates after that:**
```bash
git add .
git commit -m "Describe what changed"
git push
```

**Tip:** create the GitHub repo first via [github.com/new](https://github.com/new) (don't initialize it with a README there, to avoid a merge conflict with your local one), then run the commands above.

---

## 💡 Suggestions to Level This Project Up

1. **Merge into one clean pipeline.** Right now you have 4 separate notebooks showing your learning progression — that's great for a portfolio narrative, but also add a single `scraper.py` script (or one final notebook) that does the whole job end-to-end. Recruiters/reviewers often want the "final" clean version, not just the journey.
2. **Add error handling & retries.** Wrap `requests.get()` calls in try/except with `res.raise_for_status()`, and add a retry/backoff (e.g. via `requests` `Session` + `urllib3.Retry`) in case a page fails to load.
3. **Auto-detect the last page** instead of hardcoding `range(1, 11)` — check for the "Next" button/`<li class="next">` and stop when it disappears. This makes the scraper resilient if the site adds/removes pages.
4. **Be a polite scraper.** Add a small `time.sleep(0.5–1)` between requests and a custom `User-Agent` header, even though this is a scraping sandbox — it's good practice you'll want for real-world targets.
5. **Fix the CSV index issue.** `to_csv('Quotes.csv', index = 'false')` — note `'false'` is a *string*, not the boolean `False`, so pandas still writes the index column (visible as the unnamed first column in your CSV). Use `index=False` instead.
6. **Split `author_id` cleanup.** You could parse the author slug out (e.g. `Albert-Einstein`) into its own column for easier grouping/joins.
7. **Explode tags for analysis.** Since `tags` is a comma-joined string, consider also saving a "long" version of the data (one row per quote-tag pair) so you can easily do tag-frequency analysis or visualizations (e.g. top tags bar chart, which the site itself surfaces as "Top Ten tags").
8. **Add a scraper for author bios too.** Since you already collect `author_link`, a natural next step is scraping each author's `/author/<name>` page (born date, bio) and joining it to this dataset — the site is built to support exactly that.
9. **Add tests / data validation.** A quick check like "no nulls in `quote`/`author`", "author_link starts with the base domain", etc., protects against silent scraping breakage if the site's HTML structure changes.
10. **requirements.txt + .gitignore** are included below so the repo is reproducible and doesn't track notebook checkpoints or `__pycache__`.