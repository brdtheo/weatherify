<script>
  import { createEventDispatcher } from "svelte";
  const dispatch = createEventDispatcher();

  function closeErrorBar() {
    dispatch("setError", false);
  }
</script>

<div class="error-bar" on:click={closeErrorBar}>
  <span class="error-message">error: your request could not be resolved</span>
  <div class="progress-bar" />
</div>

<style>
  .error-bar {
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 15px;
    color: #fff;
    padding: 6px 10px 8px;
    background: #9d2933;
    position: fixed;
    bottom: 0;
    left: 0;
    width: 100%;
    font-weight: 300;
    letter-spacing: 1px;
    cursor: pointer;
  }

  .progress-bar {
    width: 100%;
    height: 3px;
    background: white;
    position: absolute;
    bottom: 0;
    left: 0;
    animation: progress 4s linear;
  }

  @keyframes progress {
    from {
      width: 0%;
    }

    to {
      width: 100%;
    }
  }
</style>
