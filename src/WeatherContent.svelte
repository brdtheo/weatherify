<script>
  import { onMount } from "svelte";
  import { format } from "date-fns";

  export let temperature = 0;
  export let city = "city";
  export let state = "state";
  export let stateIcon;
  let date = format(new Date(), "hh:mm - cccc, d LLL yy") + "'";

  onMount(() => {
    const updateDate = setInterval(() => {
      date = format(new Date(), "hh:mm - cccc, d LLL yy") + "'";
    }, 1000);
  });

  $: {
    const a = state.toLowerCase();

    if (a.includes("sunny" || "clear")) {
      stateIcon = "sunny-outline";
    } else if (a.includes("rain")) {
      stateIcon = "rainy-outline";
    } else if (a.includes("cloudy" || "overcast")) {
      stateIcon = "cloudy-outline";
    } else if (a.includes("thunderstorm")) {
      stateIcon = "thunderstorm-outline";
    } else if (a.includes("snow")) {
      stateIcon = "snow-outline";
    } else {
      stateIcon = null;
    }
  }
</script>

<div class="weather-content">
  <div class="weather-website-name">the.weather</div>
  <div class="bottom-content">
    <div class="weather-temperature">{temperature}</div>
    <div class="weather-city-date">
      <h1 id="current-city">{city}</h1>
      <span class="weather-date">{date}</span>
    </div>
    <div class="weather-state">
      <span class="state-icon"><ion-icon name={stateIcon} /></span>
      {state}
    </div>
  </div>
</div>

<style>
  .weather-content {
    width: 100%;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    padding: 50px 80px 100px;
    color: #fff;
  }

  .weather-website-name {
    font-weight: 500;
    font-size: 15px;
    font-family: "Lato";
  }

  .bottom-content {
    line-height: 1;
    display: flex;
    align-items: flex-end;
  }

  .weather-temperature {
    font-size: 150px;
  }

  .weather-temperature::after {
    content: "°";
  }

  .weather-city-date {
    padding: 0 40px 0 10px;
  }

  .weather-city-date h1 {
    font-weight: 400;
    font-size: 80px;
  }

  .weather-state {
    display: flex;
    flex-direction: column;
  }

  .weather-city-date,
  .weather-state {
    padding-bottom: 20px;
  }

  .state-icon {
    font-size: 50px;
  }

  .weather-date,
  .weather-state {
    font-weight: 300;
    font-family: "Lato", sans-serif;
  }
</style>
