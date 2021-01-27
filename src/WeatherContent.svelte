<script></script>

<div class="weather-content">
  <div class="weather-website-name">the.weather</div>
  <div class="bottom-content">
    <div class="weather-temperature">16</div>
    <div class="weather-city-date">
      <h1>London</h1>
      <span class="weather-date">06:09 - Monday, 9 Sep '19</span>
    </div>
    <div class="weather-state">
      <span class="state-icon"><ion-icon name="partly-sunny-outline" /></span>
      Cloudy
    </div>
  </div>
</div>

<style>
  .weather-content {
    flex: 1;
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
