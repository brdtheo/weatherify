<script>
  import { createEventDispatcher } from "svelte";
  const dispatch = createEventDispatcher();

  $: input = "";
  $: weather = {
    cityName: "Reims",
    temperature: 0,
    cloudy: 0,
    humidity: 0,
    windSpeed: 0,
  };

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
      if (!(query.toLowerCase() === currentCity.toLowerCase())) {
        const res = await fetch(
          `http://api.weatherstack.com/current?access_key=bc78ee92848d943acdb1658e8acdc877&query=${query}&units=m`
        );
        if (res.ok) {
          const json = await res.json();
          weather.cloudy = json.current.cloudcover;
          weather.humidity = json.current.humidity;
          weather.windSpeed = json.current.wind_speed;

          dispatch("fetchNewData", {
            weather: {
              temperature: json.current.temperature,
              city: json.location.name,
              state: json.current.weather_descriptions[0],
            },
          });
          //console.log(json);
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
      <div class="sidebar-list-item">Monday</div>
      <div class="sidebar-list-item">Tuesday</div>
      <div class="sidebar-list-item">Wednesday</div>
      <div class="sidebar-list-item">Thursday</div>
      <div class="sidebar-list-item">Saturday</div>
      <div class="sidebar-list-item">Sunday</div>
    </div>
  </section>
</div>

<style>
  .sidebar {
    backdrop-filter: blur(18px);
    background: rgba(255, 255, 255, 0.05);
    width: 480px;
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
    flex: 1;
    margin: 0 50px;
    background: none;
    border: inherit;
    border-bottom: solid rgba(255, 255, 255, 0.4) 1px;
    padding-bottom: 8px;
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
    width: 80px;
    height: 80px;
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
</style>
