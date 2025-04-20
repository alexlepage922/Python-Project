# Python-Project
golf-weather-caddy/
│
├── README.md
├── .gitignore
├── requirements.txt
├── .env (in your local, not committed)
└── main.py


# Load environment variables
load_dotenv()

WEATHER_API_KEY = os.getenv("WEATHER_API_KEY")
GOLF_API_KEY = os.getenv("GOLF_API_KEY")

def get_weather(city):
    url = f'http://api.openweathermap.org/data/2.5/weather?q={city}&appid={WEATHER_API_KEY}'
    response = requests.get(url)
    if response.status_code == 200:
        data = response.json()
        return data
    else:
        print("Error fetching weather data.")
        return None

def main():
    city = input("Enter the city for weather information: ")
    weather_data = get_weather(city)
    if weather_data:
        print(f"Weather in {city}: {weather_data['weather'][0]['description']}")
        
        # Add more details to display as needed



if __name__ == "__main__":
    main()

# Load environment variables
load_dotenv()

WEATHER_API_KEY = os.getenv("WEATHER_API_KEY")

session = requests.Session()

def get_weather(city):
    if not WEATHER_API_KEY:
        raise ValueError("Missing WEATHER_API_KEY in environment variables.")

    url = f"http://api.openweathermap.org/data/2.5/weather"
    params = {
        'q': city,
        'appid': WEATHER_API_KEY,
        'units': 'metric'
    }

    try:
        response = session.get(url, params=params, timeout=5)
        response.raise_for_status()
        return response.json()
    except requests.RequestException as e:
        print(f"Error fetching weather data: {e}")
        return None

def display_weather(data, city):
    weather_desc = data['weather'][0]['description'].capitalize()
    temp = data['main']['temp']
    wind = data['wind']['speed']
    print(f"\nWeather in {city}: {weather_desc}")
    print(f"Temperature: {temp}°C")
    print(f"Wind Speed: {wind} m/s")

    if 'rain' not in weather_desc.lower() and temp >= 10 and wind < 10:
        print("✅ Great weather for golf!")
    else:
        print("⛔ Might not be the best golfing day.")

def main():
    city = input("Enter the city for weather information: ").strip()
    if not city:
        print("City name cannot be empty.")
        return
    data = get_weather(city)
    if data:
        display_weather(data, city)

if __name__ == "__main__":
    main()
