<script></script>

<div class="sidebar">
  <section>
    <div class="sidebar-search">
      <input type="text" placeholder="Another location" />
      <button type="button" role="button">
        <ion-icon name="search-outline" />
      </button>
    </div>
    <div class="sidebar-list">
      <div class="sidebar-list-item">Birmingham</div>
      <div class="sidebar-list-item">Manchester</div>
      <div class="sidebar-list-item">New York</div>
      <div class="sidebar-list-item">California</div>
    </div>
  </section>
  <section>
    <h2>Weather Details</h2>
    <div class="sidebar-list">
      <div class="sidebar-list-item">
        <span>Cloudy</span>
        <span>86%</span>
      </div>
      <div class="sidebar-list-item">
        <span>Humidity</span>
        <span>62%</span>
      </div>
      <div class="sidebar-list-item">
        <span>Wind</span>
        <span>8km/h</span>
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
  }

  input:focus {
    border-bottom-color: #d66d32;
    outline: none;
  }

  button {
    background: #d66d32;
    border: none;
    width: 80px;
    height: 80px;
    display: inline-flex;
    justify-content: center;
    align-items: center;
    font-size: 30px;
  }

  button:focus {
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
    padding-bottom: 30px;
  }

  .sidebar-list-item:last-of-type {
    padding-bottom: 0;
  }
</style>
