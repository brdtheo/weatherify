<script>
  import Sidebar from "./Sidebar.svelte";
  import WeatherContent from "./WeatherContent.svelte";
</script>

<main>
  <div class="main-container">
    <div class="main-container-layout">
      <WeatherContent />
      <Sidebar />
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
    background-position: top;
  }

  .main-container-layout {
    display: flex;
    height: 100%;
  }
</style>
