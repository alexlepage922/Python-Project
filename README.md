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

