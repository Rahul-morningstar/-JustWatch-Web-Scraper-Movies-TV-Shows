🎬 JustWatch Web Scraper — Movies & TV Shows

A Python-based web scraping project that extracts movie and TV show data from JustWatch, performs data analysis using Pandas, and exports filtered results to CSV.


📌 Project Overview
This project was built as part of a Numerical Programming in Python assignment focused on real-world web scraping and data analysis. It collects structured data on 50+ movies and 50+ TV shows from JustWatch India, filters content based on recency and rating, and performs insightful analysis on genres and streaming platforms.

🗂️ Table of Contents

Project Overview
Tech Stack
Project Structure
Features
How It Works
Data Fields Collected
Data Analysis
Output Files
Setup & Usage
Notes & Limitations


🛠️ Tech Stack
LibraryPurposerequestsHTTP requests to JustWatch pagesBeautifulSoupHTML parsing and data extractionpandasData manipulation, filtering, exportnumpyNumerical utilitiesmatplotlibData visualizationsreRegex for pattern-based extractionjsonParsing JSON-LD structured datacollectionsCounter for genre/service frequency

📁 Project Structure
justwatch-scraper/
│
├── Numerical_Programming_in_Python_Web_Scraping_Solution.ipynb   # Main notebook
├── Final_Data.csv        # All scraped movies + TV shows
├── Filter_Data.csv       # Filtered: last 2 years + IMDb ≥ 7.0
└── README.md

✨ Features

Multi-page scraping — Iterates across listing pages to collect 50+ URLs for both movies and TV shows
Polite crawling — Random delays (1.5–3s) between requests to avoid rate limiting
Robust extraction — Multiple fallback strategies (CSS selectors → regex → JSON-LD structured data)
Unified dataset — Combines movies and TV shows into a single master DataFrame
Data filtering — Applies year and IMDb rating filters for quality data
Visualizations — Bar charts for ratings, genres, and streaming service distribution


⚙️ How It Works
Task 1 — Web Scraping
Movies

get_movie_urls() — Collects movie page URLs from /in/movies?release_year_from=2000
scrape_single_movie() — Visits each URL and extracts full detail

TV Shows

get_tvshow_urls() — Collects TV show page URLs from /in/tv-shows?release_year_from=2000
scrape_single_tvshow() — Visits each URL and extracts full detail

Each detail scraper uses dedicated functions:
FunctionWhat It Extractsscrape_movie_title() / scrape_tvshow_title()Title from <h1> or og:title metascrape_release_year()4-digit year via regex + JSON-LD fallbackscrape_genres()Genre links (/genre/ href) + JSON-LDscrape_imdb_rating()IMDb score via class match + page regex + JSON-LDscrape_runtime() / scrape_tvshow_duration()Duration pattern (e.g., 1h 45min)scrape_age_rating()Ratings like U/A, PG-13, TV-MAscrape_production_country()Country from JSON-LD countryOfOriginscrape_streaming_services()Matched against a known services list

Task 2 — Data Filtering & Analysis
After combining both DataFrames:
python# Filter 1: Released in last 2 years (2024 or 2025 as of 2026)
filtered_by_year = full_df[full_df['release_year'] >= (current_year - 2)]

# Filter 2: IMDb rating ≥ 7.0
filtered_df = filtered_by_year[filtered_by_year['imdb_rating'] >= 7.0]

📊 Data Analysis
1. Average IMDb Ratings
Calculated for:

All content combined
Movies only
TV shows only
Filtered dataset (recent + high-rated)

2. Top 5 Genres
Genre column is split and exploded to count individual genre occurrences across all titles. Displayed as a horizontal bar chart.
3. Predominant Streaming Service
Streaming services column is split and counted. Top 10 services are visualized. The service with the highest number of available titles is identified.

📋 Data Fields Collected
ColumnDescriptiontitleMovie / TV show namerelease_yearYear of releasegenreComma-separated genresimdb_ratingIMDb score (float)durationRuntime or episode lengthage_ratingContent rating (PG, U/A, TV-MA, etc.)production_countryCountry of originstreaming_servicesPlatforms where availableurlJustWatch page URLtypeMovie or TV Show

💾 Output Files
FileDescriptionFinal_Data.csvComplete scraped dataset (movies + TV shows)Filter_Data.csvFiltered: last 2 years + IMDb ≥ 7.0 only

🚀 Setup & Usage
1. Clone the Repository
bashgit clone https://github.com/your-username/justwatch-scraper.git
cd justwatch-scraper
2. Install Dependencies
bashpip install requests beautifulsoup4 pandas numpy matplotlib
3. Run the Notebook
Open the .ipynb file in Google Colab or Jupyter Notebook and run all cells top to bottom. The notebook is structured to execute end-to-end in a single run.

⚠️ JustWatch uses dynamic content on some pages. If you encounter empty results, try increasing num_pages or adding longer delays in the scraping functions.


⚠️ Notes & Limitations

Dynamic content: JustWatch renders parts of the page via JavaScript. This scraper works on the static HTML; some fields may return N/A for pages that are JS-heavy.
Rate limiting: Polite delays are included. Avoid reducing sleep times to prevent IP blocks.
Data accuracy: Genre, streaming service, and rating fields depend on JustWatch's HTML structure, which may change over time.
Scope: Scraping is limited to the JustWatch India (/in/) domain.


👤 Author
Rahul

Data Analyst | Python & Web Scraping Enthusiast
Tools: Python, SQL, Power BI, Tableau, Excel


📄 License
This project is intended for educational purposes only. Please review JustWatch's Terms of Service before use.
