# 🎵 Spotify Data Pipeline on AWS with Snowpipe & Snowflake

An end-to-end serverless data engineering pipeline that extracts Spotify playlist data through the Spotify Web API, processes it using AWS services, automatically ingests it into Snowflake via Snowpipe, and exposes the data for analytics in Power BI.

---

# 🚀 Overview

This project demonstrates a modern event-driven ETL pipeline built entirely on AWS and Snowflake.

The pipeline automatically extracts Spotify playlist data on a schedule, transforms the raw JSON into analytical datasets, loads the curated data into Snowflake using Snowpipe, and visualizes the results in Power BI.

---

# 🏗️ Architecture

<p align="center">
<img src="images/architecture.png" width="1000">
</p>

---

# 🔄 Pipeline Workflow

## 1. Extract

Amazon CloudWatch runs on a daily schedule and triggers an AWS Lambda function.

The extraction Lambda:

- Authenticates with Spotify Web API
- Retrieves playlist metadata
- Downloads track information
- Stores raw JSON into Amazon S3

```
Spotify API
        ↓
CloudWatch
        ↓
Extract Lambda
        ↓
Amazon S3 (Raw)
```

---

## 2. Transform

Uploading a raw JSON file to Amazon S3 generates an Object Created event.

The event is sent to Amazon SQS, which provides a reliable event queue.

Amazon EventBridge orchestrates the transformation workflow and invokes the transformation Lambda.

The transformation Lambda:

- Reads raw JSON
- Flattens nested Spotify objects
- Cleans and validates data
- Creates analytical tables

Output datasets include:

- Albums
- Artists
- Songs

The transformed files are written back to Amazon S3.

```
Amazon S3 (Raw)
        ↓
Amazon SQS
        ↓
Amazon EventBridge
        ↓
Transform Lambda
        ↓
Amazon S3 (Transformed)
```

---

## 3. Load

Snowpipe continuously monitors the transformed S3 bucket.

Whenever a new file arrives:

- Snowpipe automatically ingests data
- Data is loaded into Snowflake
- No manual COPY commands are required

```
Amazon S3
      ↓
Snowpipe
      ↓
Snowflake
```

---

## 4. Analytics

The curated Snowflake tables are connected directly to Power BI.

Power BI dashboards provide insights such as:

- Most popular artists
- Most popular albums
- Playlist trends
- Release year analysis
- Track popularity
- Explicit vs Non-explicit songs

---

# ⚙️ Technologies

| Category | Technology |
|------------|----------------|
| Language | Python |
| API | Spotify Web API |
| Cloud | AWS |
| Scheduler | Amazon CloudWatch |
| Compute | AWS Lambda |
| Queue | Amazon SQS |
| Orchestration | Amazon EventBridge |
| Storage | Amazon S3 |
| Data Warehouse | Snowflake |
| Auto Loading | Snowpipe |
| Visualization | Power BI |
| Version Control | Git & GitHub |

---

# 🗂️ Data Model

## Album

| Column |
|----------|
| album_id |
| album_name |
| release_date |
| total_tracks |
| external_url |

---

## Artist

| Column |
|----------|
| artist_id |
| artist_name |
| external_url |

---

## Song

| Column |
|----------|
| song_id |
| song_name |
| duration_ms |
| popularity |
| explicit |
| album_id |
| artist_id |

---

# 🔐 Environment Variables

Create a `.env` file.

```env
SPOTIPY_CLIENT_ID=YOUR_CLIENT_ID
SPOTIPY_CLIENT_SECRET=YOUR_CLIENT_SECRET
REDIRECT_URI=http://127.0.0.1:9090/callback
```

---

# ▶️ Running the Project

Clone the repository

```bash
git clone https://github.com/jasonbo9118/Spotify-Data-Pipeline---Snowpipe-and-Snowflake-on-AWS.git
```

Install dependencies

```bash
pip install -r requirements.txt
```

Run locally

```bash
python main.py
```

Deploy AWS Lambda functions and configure:

- CloudWatch Schedule
- Amazon S3
- Amazon SQS
- EventBridge
- Snowpipe
- Snowflake

---

# 📈 Skills Demonstrated

- Python
- REST APIs
- JSON Processing
- ETL Design
- Event-Driven Architecture
- Serverless Computing
- AWS Lambda
- Amazon CloudWatch
- Amazon S3
- Amazon SQS
- Amazon EventBridge
- Snowpipe
- Snowflake
- SQL
- Power BI
- Git
- GitHub

---

# 🚀 Future Improvements

- Incremental loading
- Change Data Capture (CDC)
- Data quality validation
- CloudWatch logging and monitoring
- Retry and dead-letter queues
- Infrastructure as Code (Terraform)
- CI/CD with GitHub Actions
- Unit and integration testing
- Cost optimization

---

# 👨‍💻 Author

**Jason Tonogbanua**

Data Engineering Portfolio Project

Built with Python, AWS, Snowflake, and the Spotify Web API.
