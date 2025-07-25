# SkyCast - Find My Weather
## Date:25/07/2025
## Objective:
To build a responsive single-page application using React that allows users to enter a city name and retrieve real-time weather information using the OpenWeatherMap API. This project demonstrates the use of Axios for API calls, React Router for navigation, React Hooks for state management, controlled components with validation, and basic styling with CSS.
## Tasks:

#### 1. Project Setup
Initialize React app.

Install necessary dependencies: npm install axios react-router-dom

#### 2. Routing
Set up BrowserRouter in App.js.

Create two routes:

/ – Home page with input form.

/weather – Page to display weather results.

#### 3. Home Page (City Input)
Create a controlled input field for the city name.

Add validation to ensure the input is not empty.

On valid form submission, navigate to /weather and store the city name.

#### 4. Weather Page (API Integration)
Use Axios to fetch data from the OpenWeatherMap API using the city name.

Show temperature, humidity, wind speed, and weather condition.

Convert and display temperature in both Celsius and Fahrenheit using useMemo.

#### 5. React Hooks
Use useState for managing city, weather data, and loading state.

Use useEffect to trigger the Axios call on page load.

Use useCallback to optimize form submit handler.

Use useMemo for temperature conversion logic.

#### 6. UI Styling (CSS)
Create a responsive and clean layout using CSS.

Style form, buttons, weather display cards, and navigation links.

## Programs:

Home.jsx
```
import React, { useState, useCallback } from 'react';
import { useNavigate } from 'react-router-dom';

const Home = () => {
  const [city, setCity] = useState('');
  const [error, setError] = useState('');
  const navigate = useNavigate();

  const handleSubmit = useCallback((e) => {
    e.preventDefault();
    if (!city.trim()) {
      setError('City name is required.');
      return;
    }
    setError('');
    navigate('/weather', { state: { city } });
  }, [city, navigate]);

  return (
    <div className="home">
      <h1>SkyCast 🌤️</h1>
      <form onSubmit={handleSubmit}>
        <input
          type="text"
          placeholder="Enter city name"
          value={city}
          onChange={(e) => setCity(e.target.value)}
        />
        <button type="submit">Get Weather</button>
      </form>
      {error && <p className="error">{error}</p>}
    </div>
  );
};

export default Home;
```
Weather.jsx
```

import React, { useEffect, useState, useMemo } from 'react';
import axios from 'axios';
import { useLocation, useNavigate } from 'react-router-dom';
import './Weather.css';

const Weather = () => {
  const location = useLocation();
  const navigate = useNavigate();
  const city = location.state?.city;

  const [weatherData, setWeatherData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState('');
  const apiKey = 'cd47a859eea74758b1684113251107'

  useEffect(() => {
    if (!city) {
      navigate('/');
      return;
    }

    const fetchWeather = async () => {
      try {
        const res = await axios.get(
          `https://api.weatherapi.com/v1/current.json?key=${apiKey}&q=${city}`
        );
        setWeatherData(res.data);
      } catch (err) {
        setError('Failed to fetch weather data');
      } finally {
        setLoading(false);
      }
    };

    fetchWeather();
  }, [city, navigate]);

  const celsius = weatherData?.current?.temp_c;
  const fahrenheit = useMemo(() => (celsius * 9) / 5 + 32, [celsius]);

  if (loading) return <p>Loading weather...</p>;
  if (error) return <p>{error}</p>;

  return (
    <>
      <div className="weather-container">
        <h2>Weather in {weatherData.location.name}</h2>
        <p>Temperature: {celsius}°C / {fahrenheit.toFixed(1)}°F</p>
        <p>Humidity: {weatherData.current.humidity}%</p>
        <p>Wind Speed: {weatherData.current.wind_kph} kph</p>
        <p>Condition: {weatherData.current.condition.text}</p>
        <img src={weatherData.current.condition.icon} alt="weather icon" />

      </div>
      <footer>
        <p>Varsha G - 212222230166</p>
      </footer>
    </>
  );
};

export default Weather;
```

App.jsx
```

import { Routes, Route } from 'react-router-dom';
import Home from './Home';
import Weather from './Weather';
import './app.css'; 

const App = () => {
  return (
    <Routes>
      <Route path="/" element={<Home />} />
      <Route path="/weather" element={<Weather />} />
    </Routes>
  );
};

export default App;
```

## Output:
<img width="1916" height="1032" alt="image" src="https://github.com/user-attachments/assets/d2c62ff9-9cf3-4c90-acd5-9314b3af7bb0" />
<img width="1915" height="1027" alt="image" src="https://github.com/user-attachments/assets/1f8b5575-795a-49ab-b8d0-6da827557e49" />



## Result:
A responsive single-page application using React that allows users to enter a city name and retrieve real-time weather information using the OpenWeatherMap API has been built successfully. 
