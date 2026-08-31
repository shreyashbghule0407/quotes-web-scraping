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
