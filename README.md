# API-INTEGRATION-AND-DATA-VISUALIZATION
import requests
import matplotlib.pyplot as plt
import datetime

# OpenWeatherMap API details
API_KEY = "a482f980da9e92e4fe3aaa9298e52732"
BASE_URL = "https://api.openweathermap.org/data/2.5/forecast"

# Define the city and parameters
CITY = "Andhra Pradesh"
PARAMS = {
    "q": CITY,
    "appid": API_KEY,
    "units": "metric"
}

# Fetch data from the API
response = requests.get(BASE_URL, params=PARAMS)
data = response.json()

if response.status_code == 200:
    # Parse the forecast data
    dates = []
    temperatures = []
    humidity = []
    
    for entry in data['list']:
        dates.append(datetime.datetime.fromtimestamp(entry['dt']))
        temperatures.append(entry['main']['temp'])
        humidity.append(entry['main']['humidity'])
    
    # Plot temperature trend
    plt.figure(figsize=(10, 5))
    plt.plot(dates, temperatures, color='red', marker='o', linestyle='-', label='Temperature (°C)')
    plt.title(f"Temperature Trend for {CITY}")
    plt.xlabel("Date & Time")
    plt.ylabel("Temperature (°C)")
    plt.xticks(rotation=45)
    plt.grid(True)
    plt.legend()
    plt.tight_layout()
    plt.show()

    # Plot humidity trend
    plt.figure(figsize=(10, 5))
    plt.plot(dates, humidity, color='green', marker='o', linestyle='--', label='Humidity (%)')
    plt.title(f"Humidity Trend for {CITY}")
    plt.xlabel("Date & Time")
    plt.ylabel("Humidity (%)")
    plt.xticks(rotation=40)
    plt.grid(True)
    plt.legend()
    plt.tight_layout()
    plt.show()

else:
    print(f"Failed to fetch data: {data.get('message', 'Unknown error')}")
