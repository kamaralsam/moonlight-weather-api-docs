
**🌤 Moonlight Weather API – Developer Documentation**
Moonlight Weather API is a simple, JSON-based service that lets developers fetch:

current weather

short-term forecasts

for cities anywhere in the world.

This is a mock API project designed for learning and portfolio building.
It shows how to write professional, industry-standard REST API documentation.

**🔗 Base URL**
https://api.moonlightweather.com/v1


All endpoints below are relative to this base URL.

**🔐 Authentication**

Moonlight Weather API uses API-key authentication.

Include your key in the Authorization header of every request:

Authorization: Bearer YOUR_API_KEY


If the header is missing or invalid:

Status: 401 Unauthorized

{
  "error": "invalid_api_key",
  "message": "A valid API key is required in the Authorization header."
}

**🌍 Common Concepts**
Units

Choose the measurement system:

metric → °C, m/s

imperial → °F, mph

If omitted, metric is the default.

Time Format

All timestamps follow:

ISO 8601 format

UTC timezone

Example:

2025-11-24T19:30:00Z

**📘 Endpoint Reference**
**1. Get Current Weather**

GET /weather/current

Returns the current weather for a specified location.

Query Parameters
Name	Type	Required	Description
city	string	no*	City name (e.g., London).
lat	number	no*	Latitude in decimal degrees.
lon	number	no*	Longitude in decimal degrees.
units	string	no	metric or imperial (default: metric).

* You must provide either city OR lat+lon.
If both are provided, lat + lon take priority.

Example Request (City Name)
GET /v1/weather/current?city=London&units=metric
Authorization: Bearer YOUR_API_KEY

Example Response
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

Errors

400 Bad Request

{
  "error": "bad_request",
  "message": "Missing or invalid parameters."
}


404 Not Found

{
  "error": "location_not_found",
  "message": "We could not find weather data for the specified location."
}

**2. Get 5-Day Forecast**

GET /weather/forecast

Returns a 5-day forecast in 3-hour intervals.

Query Parameters
Name	Type	Required	Description
city	string	no*	City name (e.g., Cairo)
lat	number	no*	Latitude
lon	number	no*	Longitude
units	string	no	metric or imperial (default: metric)
limit	int	no	Max forecast items (default: 40, max: 40)

* Provide either city OR lat+lon.

Example Request
GET /v1/weather/forecast?city=Cairo&units=metric&limit=8
Authorization: Bearer YOUR_API_KEY

Example Response
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
  ]
}

**3. API Health Check**

GET /health

Returns service health status.

Example Response
{
  "status": "ok",
  "time": "2025-11-24T19:05:00Z"
}

**🚦 Rate Limiting**

Free Tier: 60 requests per minute

If exceeded:

429 Too Many Requests

{
  "error": "rate_limit_exceeded",
  "message": "You have exceeded the rate limit of 60 requests per minute. Please wait and try again."
}

**🧪 Example Usage (Python)**
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

response = requests.get(f"{BASE_URL}/weather/current", headers=headers, params=params)
data = response.json()

print(data["location"]["city"], data["current"]["temperature"])
