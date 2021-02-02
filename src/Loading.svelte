<script>
  import { fade } from "svelte/transition";
</script>

<div class="loading-container" transition:fade={{ delay: 250, duration: 300 }}>
  <div class="blur-container">
    <div class="loading-content">
      <h1>Weatherify</h1>
      <div class="loading-thumb-container">
        <div class="loading-thumb" />
      </div>
      <p>loading data...</p>
    </div>
  </div>
</div>

<style>
  .loading-container {
    background-color: rgb(45, 45, 45);
    background-image: url("https://images.unsplash.com/photo-1592210454359-9043f067919b?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=1050&q=80");
    position: absolute;
    background-position: center;
    background-size: cover;
    background-repeat: no-repeat;

    width: 100%;
    height: 100%;
    top: 0;
    left: 0;
    z-index: 5;
  }

  .blur-container {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
    height: 100%;
    background: red;
    backdrop-filter: blur(10px);
    background-color: rgba(255, 255, 255, 0.5);
  }

  .loading-container {
    font-family: "Lato", sans-serif;
  }

  h1 {
    margin-bottom: 50px;
    font-size: 48px;
    font-weight: 300;
  }

  p {
    text-align: center;
    margin: 10px 0 0;
    font-weight: 300;
    font-size: 14px;
  }

  .loading-thumb-container {
    background: rgba(40, 40, 40, 0.2);
    width: 100%;
    height: 5px;
    overflow: hidden;
    position: relative;
  }

  .loading-thumb {
    position: absolute;
    left: 0;
    top: 0;
    background: rgba(255, 255, 255, 0.9);
    width: 50px;
    height: 100%;
    animation: loading 1200ms infinite;
  }

  @keyframes loading {
    0% {
      left: -25%;
    }

    100% {
      left: 105%;
    }
  }
</style>
