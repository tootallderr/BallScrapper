# ESPN Sports Data Scraper

![ESPN Logo](https://upload.wikimedia.org/wikipedia/commons/thumb/3/3e/ESPN_wordmark.svg/512px-ESPN_wordmark.svg.png)

**ESPN Sports Data Scraper** is a comprehensive Python-based tool designed to fetch, process, and store a wide range of sports data from ESPN's public APIs. It supports multiple major sports leagues, including NFL, NBA, MLB, NHL, WNBA, and College Football. The scraper efficiently handles data retrieval, caching, rate limiting, and data normalization to ensure reliable and up-to-date information for analysis, reporting, or integration into other applications.

## Table of Contents

- [Features](#features)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Configuration](#configuration)
- [Usage](#usage)
- [Project Structure](#project-structure)
- [Logging](#logging)
- [Caching](#caching)
- [Rate Limiting](#rate-limiting)
- [Data Processing](#data-processing)
- [Error Handling](#error-handling)
- [Contributing](#contributing)
- [License](#license)
- [Acknowledgements](#acknowledgements)

## Features

- **Multi-League Support**: Fetch data for NFL, NBA, MLB, NHL, WNBA, and College Football.
- **Comprehensive Data Retrieval**: Includes events, game details, odds, play-by-play actions, team statistics, depth charts, and injuries.
- **Efficient Caching**: Implements caching mechanisms to reduce redundant API calls and enhance performance.
- **Rate Limiting**: Ensures compliance with ESPN's API rate limits through controlled request pacing and exponential backoff strategies.
- **Robust Logging**: Maintains detailed logs for monitoring, debugging, and auditing purposes.
- **Concurrent Processing**: Utilizes multithreading to expedite data fetching and processing.
- **Flexible Configuration**: Easily adjustable settings for excluded keys, rate limits, and other parameters.
- **Data Normalization**: Cleans and structures JSON data into flat CSV files for seamless integration and analysis.

## Prerequisites

- **Python 3.7 or higher**: Ensure you have Python installed. You can download it from [python.org](https://www.python.org/downloads/).
- **pip**: Python package installer, typically included with Python installations.

## Installation

1. **Clone the Repository**

   ```bash
   git clone https://github.com/yourusername/espn-sports-data-scraper.git
   cd espn-sports-data-scraper
   ```

2. **Create a Virtual Environment (Optional but Recommended)**

   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. **Install Required Dependencies**

   ```bash
   pip install -r requirements.txt
   ```

   *If a `requirements.txt` file is not provided, install the necessary packages manually:*

   ```bash
   pip install calendar os re threading time json logging requests pandas urllib3 tqdm
   ```

   **Note**: Some modules like `os`, `re`, `threading`, `time`, `json`, and `logging` are part of Python's standard library and do not require installation.

## Configuration

The script includes configurable parameters for excluded keys, rate limiting, and logging. These settings are defined at the beginning of the script:

```python
# ----------------------------
# Configuration
# ----------------------------

# Keys to exclude from all endpoints
EXCLUDED_KEYS = {
    "logo",
    "logos",
    "href",
    "link",
    "links",
    "ref",
    "tracking",
    "images",
    "news",
    "ads"
}

# Rate Limiting Configuration
MAX_CALLS_PER_MINUTE = 2000  # Adjust based on ESPN's API rate limits
RATE_LIMIT_WINDOW = 60  # seconds
BACKOFF_FACTOR = 1.5
MAX_BACKOFF = 60  # seconds

# Logging Configuration
logging.basicConfig(
    filename="data_fetch.log",
    level=logging.ERROR,  # Changed to INFO to capture more details
    format="%(asctime)s - %(levelname)s - %(message)s"
)
```

### Adjusting Configuration

- **Excluded Keys**: Modify the `EXCLUDED_KEYS` set to include or exclude additional keys as needed.
- **Rate Limiting**: Adjust `MAX_CALLS_PER_MINUTE`, `RATE_LIMIT_WINDOW`, `BACKOFF_FACTOR`, and `MAX_BACKOFF` to align with ESPN's API usage policies or your specific requirements.
- **Logging Level**: Change the `level` in `logging.basicConfig` to `INFO`, `DEBUG`, or `ERROR` based on the verbosity needed.

## Usage

1. **Run the Script**

   ```bash
   python your_script_name.py
   ```

2. **Select a League**

   Upon execution, you'll be prompted to select a league or sport to scrape:

   ```
   Select a league or sport to scrape:
   1. NFL
   2. NBA
   3. MLB
   4. NHL
   5. WNBA
   6. College Football
   7. Update All Leagues
   Enter the number of your choice:
   ```

   - **Single League**: Enter the corresponding number (e.g., `1` for NFL).
   - **All Leagues**: Enter `7` to update data for all supported leagues.

3. **Process Execution**

   The script will sequentially execute the following steps for the selected league(s):

   - **Scrape Event Data**: Fetches and stores event information.
   - **Process Game Details**: Retrieves detailed game information.
   - **Process Odds Data**: Collects betting odds.
   - **Process Play-by-Play Data**: Gathers in-game actions.
   - **Fetch and Save Team Statistics**: Retrieves seasonal team stats.
   - **Fetch and Save Depth Charts**: Obtains team depth charts.
   - **Fetch and Save Injuries Data**: Collects injury reports.
   - **Generate Headers Exclude CSV**: Updates exclusion settings for CSV headers.

4. **Output**

   Data is stored in structured CSV files organized within league-specific directories, further divided into `historical` and `upcoming` subdirectories.

   ```
   /NFL
     /cache
     /historical
       Events_Standard_Combined_Historical.csv
       Team_Box_Details_Historical.csv
       ODDS_CONSENSUS_HISTORICAL.csv
       Play_By_Play_Scoring_Historical.csv
       Team_Seasonal_Splits_Historical.csv
       Depth_Charts_Historical.csv
       Injuries_Details_Historical.csv
       Combined_Data_Historical.csv
     /upcoming
       Events_Standard_Combined_Upcoming.csv
       Team_Box_Details_Upcoming.csv
       ODDS_CONSENSUS_Upcoming.csv
       Play_By_Play_Scoring_Upcoming.csv
       Team_Seasonal_Splits_Upcoming.csv
       Depth_Charts_Upcoming.csv
       Injuries_Details_Upcoming.csv
       Combined_Data_Upcoming.csv
   ```

5. **Logs**

   Detailed logs are maintained in the `data_fetch.log` file, capturing informational messages, warnings, and errors for monitoring and troubleshooting.

## Project Structure

```
espn-sports-data-scraper/
├── NFL/
│   ├── cache/
│   ├── historical/
│   │   ├── Events_Standard_Combined_Historical.csv
│   │   ├── Team_Box_Details_Historical.csv
│   │   ├── ODDS_CONSENSUS_HISTORICAL.csv
│   │   ├── Play_By_Play_Scoring_Historical.csv
│   │   ├── Team_Seasonal_Splits_Historical.csv
│   │   ├── Depth_Charts_Historical.csv
│   │   ├── Injuries_Details_Historical.csv
│   │   └── Combined_Data_Historical.csv
│   ├── upcoming/
│   │   ├── Events_Standard_Combined_Upcoming.csv
│   │   ├── Team_Box_Details_Upcoming.csv
│   │   ├── ODDS_CONSENSUS_Upcoming.csv
│   │   ├── Play_By_Play_Scoring_Upcoming.csv
│   │   ├── Team_Seasonal_Splits_Upcoming.csv
│   │   ├── Depth_Charts_Upcoming.csv
│   │   ├── Injuries_Details_Upcoming.csv
│   │   └── Combined_Data_Upcoming.csv
├── data_fetch.log
├── your_script_name.py
└── requirements.txt
```

**Directories and Files:**

- **League Directories (e.g., `NFL/`)**: Contains all data related to a specific league.
  - **cache/**: Stores cached API responses to minimize redundant requests.
  - **historical/**: Holds CSV files with historical data.
  - **upcoming/**: Holds CSV files with upcoming/future data.
- **data_fetch.log**: Log file capturing the execution details.
- **your_script_name.py**: The main Python script to run.
- **requirements.txt**: Lists Python dependencies (if provided).

## Logging

The scraper employs Python's built-in `logging` module to track its operations. Logs are written to `data_fetch.log` with the following format:

```
2024-04-27 12:34:56,789 - INFO - Starting to scrape event data for NFL.
2024-04-27 12:35:00,123 - ERROR - Failed to fetch data from https://example.com/api: Timeout error
...
```

**Log Levels:**

- **INFO**: General operational messages indicating progress.
- **WARNING**: Indications of potential issues that don't halt execution.
- **ERROR**: Critical issues that prevent certain operations from completing.

**Adjusting Log Level:**

Modify the `level` parameter in the logging configuration to change verbosity:

```python
logging.basicConfig(
    filename="data_fetch.log",
    level=logging.INFO,  # Change to DEBUG, INFO, WARNING, ERROR as needed
    format="%(asctime)s - %(levelname)s - %(message)s"
)
```

## Caching

To enhance performance and reduce API load, the scraper implements a caching mechanism:

- **Cache Storage**: Cached responses are stored as JSON files within the `cache/` directory inside each league's folder.
- **Cache Validity**:
  - **Historical Data**: Cached indefinitely unless manually cleared.
  - **Upcoming Data**: Cached for 5 minutes (`TTL = 300` seconds) to ensure data freshness.
- **Cache Management**: The scraper automatically checks cache validity before making API requests and updates cache files accordingly.

## Rate Limiting

To comply with ESPN's API usage policies and prevent overloading the servers, the scraper incorporates a rate limiting strategy:

- **Parameters**:
  - **MAX_CALLS_PER_MINUTE**: Maximum number of API calls allowed per minute (default: 2000).
  - **RATE_LIMIT_WINDOW**: Time window for rate limiting in seconds (default: 60).
  - **BACKOFF_FACTOR**: Multiplier for exponential backoff (default: 1.5).
  - **MAX_BACKOFF**: Maximum backoff time in seconds (default: 60).
- **Mechanism**:
  - Tracks timestamps of API calls.
  - If the rate limit is reached, the scraper sleeps for an exponentially increasing duration before retrying.
  - Implements retries for transient HTTP errors (status codes 429, 500, 502, 503, 504).

**Adjusting Rate Limits**:

Modify the rate limiting configuration at the top of the script to suit your requirements or ESPN's updated policies.

## Data Processing

The scraper processes raw JSON data from ESPN's APIs into structured CSV files. Key processing steps include:

- **Data Cleaning**: Removes excluded keys to streamline data.
- **Flattening Nested Structures**: Converts nested JSON objects and lists into flat dictionaries suitable for CSV storage.
- **Dynamic Parsing**: Handles various data formats (e.g., scores, records) by parsing and structuring them into consistent formats.
- **Data Deduplication**: Checks existing CSV files to avoid duplicate entries.
- **Concurrent Execution**: Utilizes `ThreadPoolExecutor` for parallel data fetching, enhancing efficiency.

### Key Data Types and Their Processing:

- **Events**: General information about games/events.
- **Game Details**: Detailed statistics and information about each game.
- **Odds**: Betting odds and related information.
- **Play-by-Play**: Minute-by-minute or play-by-play actions within games.
- **Team Statistics**: Seasonal statistics for teams.
- **Depth Charts**: Player rankings and positions within teams.
- **Injuries**: Information about player injuries.

## Error Handling

The scraper is designed to handle various error scenarios gracefully:

- **HTTP Errors**: Implements retries with exponential backoff for transient errors.
- **Data Parsing Errors**: Logs and skips over malformed or unexpected data without halting execution.
- **File I/O Errors**: Logs issues related to reading from or writing to CSV files.
- **Invalid User Inputs**: Validates league selection input and logs warnings for invalid choices.

All errors and warnings are recorded in the `data_fetch.log` file for review and troubleshooting.

