<script>
  import { onMount } from "svelte";
  import Loading from "./Loading.svelte";
  import Sidebar from "./Sidebar.svelte";
  import WeatherContent from "./WeatherContent.svelte";

  let loading = true;
  let temperature;
  let city;
  let state;
  let weather = {};

  function updateProps(event) {
    temperature = event.detail.weather.temperature;
    city = event.detail.weather.city;
    state = event.detail.weather.state;
  }

  function setLoading(event) {
    loading = event.detail;
  }

  onMount(async () => {
    const res = await fetch(
      "http://api.openweathermap.org/data/2.5/weather?q=London&appid=bb3fdf7b1caa0a590ae5218d425c21a4"
    );

    if (res.ok) {
      const json = await res.json();
      temperature = Math.round(json.main.temp - 273.15);
      city = json.name;
      state = json.weather[0].main;
      weather.cloudy = json.clouds.all;
      weather.humidity = json.main.humidity;
      weather.windSpeed = Math.round(json.wind.speed);
      loading = false;
    }
  });
</script>

<main>
  <div class="main-container">
    <div class="main-container-layout">
      {#if loading}
        <Loading />
      {/if}
      <WeatherContent {city} {temperature} {state} />
      <Sidebar
        on:setLoading={setLoading}
        on:fetchNewData={updateProps}
        {weather}
      />
    </div>
  </div>
</main>

<style>
  .main-container {
    width: 100%;
    height: 100%;
    background: url("https://images.unsplash.com/photo-1504608524841-42fe6f032b4b?ixlib=rb-1.2.1&ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&auto=format&fit=crop&w=1601&q=80");
    background-repeat: no-repeat;
    background-size: cover;
    background-position: 0 -150px;
  }

  .main-container-layout {
    display: flex;
    height: 100%;
  }
</style>
