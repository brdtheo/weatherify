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
        backgroundImage =
          "https://images.unsplash.com/photo-1504608524841-42fe6f032b4b?ixlib=rb-1.2.1&ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&auto=format&fit=crop&w=1601&q=80";
        break;
      case "Rain":
        backgroundImage =
          "https://images.unsplash.com/photo-1486016006115-74a41448aea2?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=1347&q=80";
        break;
      case "Sunny":
        backgroundImage =
          "https://images.unsplash.com/photo-1438129460879-8f5868d4a802?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=1350&q=80";
        break;
      case "Clear":
        backgroundImage =
          "https://images.unsplash.com/photo-1438129460879-8f5868d4a802?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=1350&q=80";
        break;
      case "Snow":
        backgroundImage =
          "https://images.unsplash.com/photo-1554417063-60e738613596?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=1340&q=80";
        break;
      case "Mist":
        backgroundImage =
          "https://images.unsplash.com/photo-1604896787809-5f3082b014de?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=1489&q=80";
        break;
    }
  }

  onMount(async () => {
    const current = await fetch(
      "http://api.openweathermap.org/data/2.5/weather?q=London&appid=bb3fdf7b1caa0a590ae5218d425c21a4&units=metric"
    );
    const forecast = await fetch(
      `http://api.openweathermap.org/data/2.5/forecast?q=London&appid=bb3fdf7b1caa0a590ae5218d425c21a4&units=metric`
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
      forecastWeather.shift();
    }

    if (current.ok && forecast.ok) {
      loading = false;
      setBackground();
    }
  });
</script>

<main>
  <div
    class="main-container {backgroundImage ===
    'https://images.unsplash.com/photo-1504608524841-42fe6f032b4b?ixlib=rb-1.2.1&ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&auto=format&fit=crop&w=1601&q=80'
      ? 'background-top'
      : ''}"
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
    transition: background-image ease-in-out 200ms;
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
