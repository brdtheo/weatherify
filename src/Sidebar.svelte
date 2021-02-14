<script>
  import { format } from "date-fns";
  import { createEventDispatcher } from "svelte";
  const dispatch = createEventDispatcher();

  export let weather = {};
  export let forecastWeather = [];

  $: input = "";

  function pressEnter(key) {
    if (key.keyCode === 13) {
      getWeather();
    }
  }

  function setCity(e) {
    if (e.type === "click") {
      input = e.target.innerText;
      getWeather();
    }
  }

  async function getWeather() {
    const query = input;
    const currentCity = document.getElementById("current-city").innerText;

    if (query) {
      dispatch("setError", false);
      if (!(query.toLowerCase() === currentCity.toLowerCase())) {
        dispatch("setLoading", true);
        const current = await fetch(
          `http://api.openweathermap.org/data/2.5/weather?q=${query}&appid=bb3fdf7b1caa0a590ae5218d425c21a4&units=metric`
        );
        const forecast = await fetch(
          `http://api.openweathermap.org/data/2.5/forecast?q=${query}&appid=bb3fdf7b1caa0a590ae5218d425c21a4&units=metric`
        );

        if (current.ok) {
          const currentJSON = await current.json();

          weather.cloudy = currentJSON.clouds.all;
          weather.humidity = currentJSON.main.humidity;
          weather.windSpeed = Math.round(currentJSON.wind.speed);

          dispatch("fetchNewData", {
            weather: {
              temperature: Math.round(currentJSON.main.temp),
              city: currentJSON.name,
              state: currentJSON.weather[0].main,
            },
          });
          dispatch("setLoading", false);
        } else if (!current.ok) {
          dispatch("setLoading", false);
          dispatch("setError", true);
        }

        if (forecast.ok) {
          const forecastJSON = await forecast.json();
          forecastWeather = [];
          for (let i = 0; i < forecastJSON.list.length; i += 8) {
            forecastWeather.push(forecastJSON.list[i]);
          }
        }
      }
    }
  }
</script>

<div class="sidebar">
  <section>
    <div class="sidebar-search">
      <input
        type="text"
        placeholder="Another location"
        bind:value={input}
        on:keyup={pressEnter}
      />
      <button
        class="search-button"
        type="button"
        role="button"
        on:click={getWeather}
      >
        <ion-icon name="search-outline" />
      </button>
    </div>
    <div class="sidebar-list">
      <button type="button" class="sidebar-list-item" on:click={setCity}>
        Birmingham
      </button>
      <button type="button" class="sidebar-list-item" on:click={setCity}>
        Manchester
      </button>
      <button type="button" class="sidebar-list-item" on:click={setCity}>
        New York
      </button>
      <button type="button" class="sidebar-list-item" on:click={setCity}>
        California
      </button>
    </div>
  </section>
  <section>
    <h2>Weather Details</h2>
    <div class="sidebar-list">
      <div class="sidebar-list-item">
        <span>Cloudy</span>
        <span>{weather.cloudy}%</span>
      </div>
      <div class="sidebar-list-item">
        <span>Humidity</span>
        <span>{weather.humidity}%</span>
      </div>
      <div class="sidebar-list-item">
        <span>Wind</span>
        <span>{weather.windSpeed}km/h</span>
      </div>
    </div>
  </section>
  <section>
    <h2>Next Days</h2>
    <div class="sidebar-list">
      {#each forecastWeather as day}
        <div class="sidebar-list-item">
          <span>{format(new Date(day.dt_txt), "MMMM d")}</span>
          <span>{Math.round(day.main.temp)}° - {day.weather[0].main}</span>
        </div>
      {/each}
    </div>
  </section>
</div>

<style>
  .sidebar {
    backdrop-filter: blur(18px);
    -webkit-backdrop-filter: blur(18px);
    -moz-backdrop-filter: blur(18px);
    -ms-backdrop-filter: blur(18px);
    background: rgba(70, 70, 70, 0.5);
    width: 100%;
    box-shadow: 0 -5px 16px 10px rgba(0, 0, 0, 0.1);
    font-family: "Lato", sans-serif;
    font-size: 18px;
    font-weight: 300;
    color: rgba(255, 255, 255, 0.9);
    display: flex;
    flex-direction: column;
    overflow-y: scroll;
    -ms-overflow-style: none;
    scrollbar-width: none;
  }

  .sidebar::-webkit-scrollbar {
    display: none;
  }

  section {
    margin: 0 50px;
    border-top: solid rgba(255, 255, 255, 0.3) 1px;
    color: rgba(255, 255, 255, 0.4);
  }

  section h2 {
    margin-top: 55px;
    color: #fff;
    font-size: inherit;
    font-weight: 400;
  }

  section:first-of-type {
    margin: 0;
    border-top: none;
    display: flex;
    flex-direction: column;
  }

  section:first-of-type .sidebar-list {
    margin: 0 50px;
  }

  .sidebar-search {
    display: inline-flex;
    align-items: flex-end;
  }

  input {
    display: flex;
    width: 100%;
    margin: 0 50px;
    background: none;
    border: inherit;
    border-bottom: solid rgba(255, 255, 255, 0.4) 1px;
    padding-bottom: 8px;
    border-radius: 0;
    font: inherit;
    color: inherit;
    transition: border-bottom-color ease-in-out 100ms;
  }

  input:focus {
    border-bottom-color: #d66d32;
    outline: none;
  }

  button {
    font-size: inherit;
    background: inherit;
    border: inherit;
    color: inherit;
    font: inherit;
  }

  .search-button {
    background: var(--orange);
    color: #000;
    border: none;
    min-width: 80px;
    min-height: 80px;
    display: inline-flex;
    justify-content: center;
    align-items: center;
    font-size: 25px;
  }

  .search-button:focus {
    outline: none;
    border: dotted #fff 2px;
  }

  .sidebar-list {
    padding: 55px 0;
  }

  .sidebar-list-item {
    display: flex;
    justify-content: space-between;
    width: 100%;
    margin-bottom: 30px;
  }

  .sidebar-list-item:last-of-type {
    padding-bottom: 0;
  }

  button.sidebar-list-item {
    transition: color ease-in 100ms;
  }

  button.sidebar-list-item:focus,
  button.sidebar-list-item:hover {
    color: #fff;
    outline: none;
  }

  @media screen and (max-width: 768px) {
    section,
    section:first-of-type .sidebar-list,
    input {
      margin: 0 20px;
    }
  }
</style>
