<script lang="ts">
  import { onMount } from 'svelte';
  import phoneIcon from './assets/phone.svg';

  const orientationMap = {
    'portrait-primary': 0,
    'landscape-primary': -90,
    'portrait-secondary': -180,
    'landscape-secondary': -270,
  };

  let mainElement: HTMLElement;
  let orientation = $state(screen.orientation.type);
  let fullscreen = $state(false);

  onMount(() => {
    screen.orientation.addEventListener('change', () => {
      orientation = screen.orientation.type;
    });
    document.addEventListener('fullscreenchange', () => {
      orientation = screen.orientation.type;
      fullscreen = document.fullscreenElement ? true : false;
    });
    document.addEventListener('focusin', () => {
      orientation = screen.orientation.type;
      fullscreen = document.fullscreenElement ? true : false;
    });
  });

  function handleOrientationChange(
    e: MouseEvent & {
      currentTarget: EventTarget & HTMLButtonElement;
    },
  ) {
    const orientation = e.currentTarget.dataset
      .orientation as keyof typeof orientationMap;
    screen.orientation.lock(orientation);
  }
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
  <div class="container">
    <button
      onclick={handleOrientationChange}
      aria-label="Change orientation to portrait primary"
      data-orientation="portrait-primary"
    >
      <img
        src={phoneIcon}
        alt=""
        aria-hidden="true"
        style="rotate: {orientationMap['portrait-primary']}deg"
      />
    </button>
    <button
      onclick={handleOrientationChange}
      aria-label="Change orientation to landscape primary"
      data-orientation="landscape-primary"
    >
      <img
        src={phoneIcon}
        alt=""
        aria-hidden="true"
        style="rotate: {orientationMap['landscape-primary']}deg"
      />
    </button>
    <button
      onclick={handleOrientationChange}
      aria-label="Change orientation to portrait secondary"
      data-orientation="portrait-secondary"
    >
      <img
        src={phoneIcon}
        alt=""
        aria-hidden="true"
        style="rotate: {orientationMap['portrait-secondary']}deg"
      />
    </button>
    <button
      onclick={handleOrientationChange}
      aria-label="Change orientation to landscape secondary"
      data-orientation="landscape-secondary"
    >
      <img
        src={phoneIcon}
        alt=""
        aria-hidden="true"
        style="rotate: {orientationMap['landscape-secondary']}deg"
      />
    </button>
  </div>
  <div class="info">
    <p class="title">Current Orientation</p>
    <img
      src={phoneIcon}
      alt={orientation}
      style="rotate: {orientationMap[orientation]}deg"
    />
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

  .container {
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 8px;
  }

  .container button {
    width: 70px;
    height: 70px;
    padding: 0;
  }

  .container button img,
  .info img {
    width: 48px;
    height: 48px;
  }

  .info {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }

  .info .title {
    font-size: 20px;
    font-weight: 700;
  }

  @media (prefers-color-scheme: dark) {
    .container button img,
    .info img {
      filter: invert(1);
    }
  }
</style>
