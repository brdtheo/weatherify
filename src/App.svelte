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

  onMount(async () => {
    
    const res = await fetch(
      "http://api.weatherstack.com/current?access_key=bc78ee92848d943acdb1658e8acdc877&query=London&units=m"
    );
    
    if (res.ok) {
      const json = await res.json();
      temperature = json.current.temperature;
      city = json.location.name;
      state = json.current.weather_descriptions[0];
      weather.cloudy = json.current.cloudcover;
      weather.humidity = json.current.humidity;
      weather.windSpeed = json.current.wind_speed;
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
      <Sidebar on:fetchNewData={updateProps} {weather} />
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
