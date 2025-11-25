# 🌤 Moonlight Weather API – Developer Documentation

Moonlight Weather API is a simple, JSON-based service that lets developers fetch:

- current weather  
- short-term forecasts  

for cities around the world.

This is a **mock API** designed for learning and portfolio purposes.

---

## 🔗 Base URL

```text
https://api.moonlightweather.com/v1
All endpoints described below are relative to this base URL.

🔐 Authentication
Moonlight Weather API uses a simple API key.

Include your key in the Authorization header for every request:

http
Copy code
Authorization: Bearer YOUR_API_KEY
If the header is missing or invalid, the API returns:

json
Copy code
HTTP 401 Unauthorized
{
  "error": "invalid_api_key",
  "message": "A valid API key is required in the Authorization header."
}
🌍 Common Concepts
Units
You can choose metric or imperial units:

metric → °C, m/s

imperial → °F, mph

If not provided, metric is used by default.

Time Format
All timestamps are returned in ISO 8601 format in UTC, for example:

text
Copy code
2025-11-24T19:30:00Z
📘 Endpoint Reference
1. Get Current Weather
GET /weather/current

Returns the current weather for a given location.

Query Parameters
Name	Type	Required	Description
city	string	no*	City name (e.g., London).
lat	number	no*	Latitude in decimal degrees.
lon	number	no*	Longitude in decimal degrees.
units	string	no	metric or imperial (default: metric).

* You must provide either city or lat+lon.
If both are provided, lat+lon take priority.

Example Request (by city)
http
Copy code
GET /v1/weather/current?city=London&units=metric
Authorization: Bearer YOUR_API_KEY
Example Response
json
Copy code
{
  "location": {
    "city": "London",
    "country": "GB",
    "lat": 51.5074,
    "lon": -0.1278
  },
  "current": {
    "timestamp": "2025-11-24T19:00:00Z",
    "temperature": 12.3,
    "feels_like": 10.8,
    "humidity": 82,
    "pressure": 1013,
    "wind_speed": 3.4,
    "wind_direction": 250,
    "conditions": "light rain",
    "icon": "rain"
  },
  "units": "metric"
}
Possible Errors
400 Bad Request – missing or invalid parameters

401 Unauthorized – missing/invalid API key

404 Not Found – location not found

json
Copy code
{
  "error": "location_not_found",
  "message": "We could not find weather data for the specified location."
}
2. Get 5-Day Forecast
GET /weather/forecast

Returns a 5-day forecast in 3-hour intervals.

Query Parameters
Name	Type	Required	Description
city	string	no*	City name (e.g., Cairo).
lat	number	no*	Latitude in decimal degrees.
lon	number	no*	Longitude in decimal degrees.
units	string	no	metric or imperial (default: metric).
limit	int	no	Max number of forecast entries (default: 40, max 40)

* Provide either city or lat+lon.

Example Request
http
Copy code
GET /v1/weather/forecast?city=Cairo&units=metric&limit=8
Authorization: Bearer YOUR_API_KEY
Example Response
json
Copy code
{
  "location": {
    "city": "Cairo",
    "country": "EG",
    "lat": 30.0444,
    "lon": 31.2357
  },
  "units": "metric",
  "forecast": [
    {
      "timestamp": "2025-11-24T21:00:00Z",
      "temperature": 22.0,
      "feels_like": 21.3,
      "humidity": 55,
      "pressure": 1010,
      "wind_speed": 2.1,
      "wind_direction": 310,
      "conditions": "clear sky",
      "icon": "clear"
    },
    {
      "timestamp": "2025-11-25T00:00:00Z",
      "temperature": 20.1,
      "feels_like": 19.5,
      "humidity": 60,
      "pressure": 1011,
      "wind_speed": 1.8,
      "wind_direction": 290,
      "conditions": "few clouds",
      "icon": "partly_cloudy"
    }
    // ... up to `limit` entries
  ]
}
Possible Errors
Same as /weather/current:

400 Bad Request

401 Unauthorized

404 Not Found

3. API Health Check
GET /health

Simple endpoint to confirm the API is running.

Example Request
http
Copy code
GET /v1/health
Example Response
json
Copy code
{
  "status": "ok",
  "time": "2025-11-24T19:05:00Z"
}
🚦 Rate Limiting
For this mock API, assume:

Free tier: 60 requests / minute

If the limit is exceeded, the API returns:

json
Copy code
HTTP 429 Too Many Requests
{
  "error": "rate_limit_exceeded",
  "message": "You have exceeded the rate limit of 60 requests per minute. Please wait and try again."
}
🧪 Example Usage (Python)
python
Copy code
import requests

BASE_URL = "https://api.moonlightweather.com/v1"
API_KEY = "YOUR_API_KEY"

headers = {
    "Authorization": f"Bearer {API_KEY}"
}

params = {
    "city": "Istanbul",
    "units": "metric"
}

resp = requests.get(f"{BASE_URL}/weather/current", headers=headers, params=params)
data = resp.json()

print(data["location"]["city"], data["current"]["temperature"])
yaml
Copy code

Then save the file (`Ctrl + S`).

Congratulations — that’s already a **complete technical writing project**.

---

## 🧾 Step 4 – Initialize Git and commit (optional but recommended)

Open the **terminal** in VS Code (bottom panel):

1. Make sure you’re inside `moonlight-weather-api-docs` folder.
2. Run:

   ```bash
   git init
   git add README.md
   git commit -m "Add Weather API documentation"
If Git complains about username/email, we can fix that, but you’ve done this before for your other repos, so you might be set.

🌐 Step 5 – Create a GitHub repo and push
Go to GitHub → click New Repository

Name it something like:

text
Copy code
moonlight-weather-api-docs
Leave “Initialize with README” unchecked (you already have one).

Click Create repository.

GitHub will show you instructions. In your VS Code terminal, run:

bash
Copy code
git remote add origin https://github.com/YOUR_USERNAME/moonlight-weather-api-docs.git
git branch -M main
git push -u origin main