<script></script>

<div class="weather-content">
  <div class="weather-website-name">the.weather</div>
  <div class="bottom-content">
    <div class="weather-temperature">16</div>
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
    font-family: 'Lato';
  }

  .bottom-content {
    line-height: 1;
  }

  .weather-temperature {
    font-size: 100px;
  }

  .weather-temperature::after {
    content: '°'
  }
</style>
