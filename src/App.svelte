<script lang="ts">
  let mainElement: HTMLElement;
  let orientation = $state(screen.orientation.type);
  let fullscreen = $state(false);

  screen.orientation.addEventListener('change', () => {
    orientation = screen.orientation.type;
  });
</script>

<main bind:this={mainElement}>
  <button
    onclick={() => {
      if (document.fullscreenElement) {
        document.exitFullscreen();
        fullscreen = false;
      } else {
        mainElement.requestFullscreen();
        fullscreen = true;
      }
    }}
  >
    {fullscreen ? 'Exit Fullscreen' : 'Fullscreen'}
  </button>
  <div>
    <button
      onclick={() => {
        // @ts-expect-error
        screen.orientation.lock('landscape');
      }}
    >
      Landscape
    </button>
    <button
      onclick={() => {
        // @ts-expect-error
        screen.orientation.lock('portrait');
      }}
    >
      Portrait
    </button>
    <p>Current Orientation: <b>{orientation}</b></p>
  </div>
</main>

<style>
  main {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    gap: 8px;
  }
</style>
