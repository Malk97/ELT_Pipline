
# 🌦️ Weather ETL Pipeline with Apache Airflow

This project implements a Weather ETL (Extract, Transform, Load) pipeline using Apache Airflow, Open-Meteo API, and PostgreSQL. The pipeline retrieves current weather data for a specific location (London 🌍), transforms the data, and stores it in a PostgreSQL database.

## 📁 Project Structure

### Airflow DAG
The DAG (Directed Acyclic Graph) defines the ETL workflow with three main tasks:

- **extract**: Fetches weather data from the Open-Meteo API.
- **transform**: Processes and formats the fetched data.
- **load_weather_data**: Saves the transformed data into a PostgreSQL database.

### Docker Compose
A docker-compose configuration is provided to set up the PostgreSQL database.

## 🛠️ Setup Instructions

### 1. Prerequisites
- Install Apache Airflow and its dependencies.
- Install Docker and Docker Compose.

### 2. Start PostgreSQL Database
Run the following command to start the PostgreSQL database:

```bash
docker-compose up -d
```

### 3. Configure Airflow Connections

**Add API Connection:**

- Go to Airflow `Admin > Connections`.
- Create a new connection with:
  - **ID**: `open_meteo_api`
  - **Type**: `HTTP`
  - **Host**: `https://api.open-meteo.com`

**PostgreSQL Connection:**

- Ensure a default connection is available with:
  - **ID**: `postgres_default`

### 4. Run the DAG

- Place the DAG in your Airflow `dags` folder.
- Start Airflow:

```bash
airflow standalone
```

- Enable and trigger the `weather_etl_pipeline` DAG from the Airflow UI.

## 📂 Data Pipeline Workflow

### Extract:
- Uses the `HttpHook` to fetch weather data from the Open-Meteo API.
- Returns weather data in JSON format.

### Transform:
- Extracts the current weather details from the API response.
- Reformats the data into a structured dictionary.

### Load:
- Creates a PostgreSQL table (`weather_data`) if it doesn't exist.
- Inserts the transformed weather data into the database.

## 📊 Database Schema

The weather data is stored in a PostgreSQL table named `weather_data` with the following schema:

| Column          | Data Type | Description                   |
|------------------|-----------|-------------------------------|
| latitude         | FLOAT     | Latitude of the location      |
| longitude        | FLOAT     | Longitude of the location     |
| temperature      | FLOAT     | Current temperature (°C)      |
| windspeed        | FLOAT     | Wind speed (km/h)             |
| winddirection    | FLOAT     | Wind direction (degrees)      |
| weathercode      | INT       | Weather condition code        |
| timestamp        | TIMESTAMP | Timestamp of data insertion   |

## 📌 Notes

- The latitude and longitude are hardcoded for London. Modify the `LATITUDE` and `LONGITUDE` constants in the code to change the location.
- Use `@daily` as the schedule interval for daily weather updates. You can adjust the frequency as needed.

## 🔧 Troubleshooting

### Common Issues:

- **HTTP Connection Error:**
  - Verify the `open_meteo_api` connection in Airflow.

- **Database Connection Issues:**
  - Ensure the PostgreSQL database is running (`docker ps`).
  - Check the `postgres_default` connection in Airflow.

- **Airflow DAG Errors:**
  - Check the Airflow logs for detailed error messages.

## 🚀 Future Enhancements

- Add support for multiple locations.
- Store weather data in a time-series database for better analytics.
- Implement data visualization using tools like Grafana.

Enjoy working with your weather ETL pipeline! 🌟
