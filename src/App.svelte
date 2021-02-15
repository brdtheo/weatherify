<script>
  import { onMount } from "svelte";
  import Loading from "./Loading.svelte";
  import Sidebar from "./Sidebar.svelte";
  import WeatherContent from "./WeatherContent.svelte";
  import ErrorBar from "./ErrorBar.svelte";

  let loading = true;
  let backgroundImage;
  let temperature;
  let city;
  let state;
  let weather = {};
  let forecastWeather = [];
  let error = false;
  $: isBackgroundClouds =
    backgroundImage === "images/clouds.jpeg" ? true : false;

  $: {
    console.log(state);

    let accentColor = null;

    switch (state) {
      case "Clouds":
        accentColor = "#d66d32";
        break;
      case "Rain":
        accentColor = "#829f9a";
        break;
      case "Drizzle":
        accentColor = "#829f9a";
        break;
      case "Clear":
        accentColor = "#a9cee1";
        break;
      case "Sunny":
        accentColor = "#a9cee1";
        break;
      case "Snow":
        accentColor = "#A6AAAA";
        break;
      case "Mist":
        accentColor = "#92A8B1";
        break;
      case "Haze":
        accentColor = "#92A8B1";
        break;
      default:
        accentColor = null;
    }

    if (!accentColor) {
      new Error("could not define accent color");
    } else {
      let root = document.documentElement;
      root.style.setProperty("--accent-color", accentColor);
    }
  }

  function setError(event) {
    error = event.detail;
    setTimeout(() => {
      error = false;
    }, 4000);
  }

  function updateProps(event) {
    temperature = event.detail.weather.temperature;
    city = event.detail.weather.city;
    state = event.detail.weather.state;
    setBackground();
  }

  function setLoading(event) {
    loading = event.detail;
  }

  function setBackground() {
    switch (state) {
      case "Clouds":
        backgroundImage = "images/clouds.jpeg";
        break;
      case "Rain":
        backgroundImage = "images/rain.jpeg";
        break;
      case "Drizzle":
        backgroundImage = "images/rain.jpeg";
        break;
      case "Sunny":
        backgroundImage = "images/clear.jpeg";
        break;
      case "Clear":
        backgroundImage = "images/clear.jpeg";
        break;
      case "Snow":
        backgroundImage = "images/snow.jpeg";
        break;
      case "Mist":
        backgroundImage = "images/mist.jpeg";
        break;
      case "Haze":
        backgroundImage = "images/mist.jpeg";
        break;
    }
  }

  onMount(async () => {
    const current = await fetch(
      `http://api.openweathermap.org/data/2.5/weather?q=London&appid=${WEATHER_API_KEY}&units=metric`
    );
    const forecast = await fetch(
      `http://api.openweathermap.org/data/2.5/forecast?q=London&appid=${WEATHER_API_KEY}&units=metric`
    );

    if (current.ok) {
      const currentJSON = await current.json();
      temperature = Math.round(currentJSON.main.temp);
      city = currentJSON.name;
      state = currentJSON.weather[0].main;
      weather.cloudy = currentJSON.clouds.all;
      weather.humidity = currentJSON.main.humidity;
      weather.windSpeed = Math.round(currentJSON.wind.speed);
    }

    if (forecast.ok) {
      const forecastJSON = await forecast.json();
      forecastWeather = [];
      for (let i = 0; i < forecastJSON.list.length; i += 8) {
        forecastWeather.push(forecastJSON.list[i]);
      }
    }

    if (current.ok && forecast.ok) {
      loading = false;
      setBackground();
    }
  });
</script>

<main>
  {#if backgroundImage}
    <div
      class="main-container {isBackgroundClouds ? 'background-top' : ''}"
      style={`background-image: url(${backgroundImage});`}
    >
      <div class="main-container-layout">
        {#if loading}
          <Loading />
        {/if}
        <WeatherContent {city} {temperature} {state} />
        <Sidebar
          on:setLoading={setLoading}
          on:fetchNewData={updateProps}
          on:setError={setError}
          {weather}
          {forecastWeather}
        />
      </div>
    </div>
  {/if}

  {#if error}
    <ErrorBar on:setError={setError} />
  {/if}
</main>

<style>
  .main-container {
    width: 100%;
    height: 100%;
    background-repeat: no-repeat;
    background-size: cover;
    background-position: center;
    background-color: rgba(10, 10, 10, 0.8);
    transition: background-image ease-in-out 300ms;
  }

  .main-container-layout {
    display: grid;
    grid-template-columns: 1fr 480px;
    height: 100%;
  }

  .background-top {
    background-position: 0 -150px;
  }

  @media screen and (max-width: 1200px) {
    .main-container-layout {
      grid-template-columns: 1fr 350px;
    }
  }

  @media screen and (max-width: 1000px) {
    .main-container {
      background-position: inherit;
    }
  }

  @media screen and (max-width: 768px) {
    .main-container {
      height: auto;
    }

    .main-container-layout {
      grid-template-columns: 1fr;
    }
  }
</style>
