<script lang="ts">
  import { onMount } from 'svelte';
  import MusicPlayer from './MusicPlayer.svelte';
  import Polaroid from './Polaroid.svelte';
  import Loader from './Loader.svelte';
  import Logo from './Logo.svelte';
  import SocialIcons from './SocialIcons.svelte';

  let loading = $state(true);

  onMount(() => {
    if (document.readyState === 'complete') {
      loading = false;
    } else {
      window.addEventListener('load', () => { loading = false; }, { once: true });
    }
  });
</script>

<svelte:head>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin="anonymous">
  <link href="https://fonts.googleapis.com/css2?family=Covered+By+Your+Grace&family=Lacquer&display=swap" rel="stylesheet">
</svelte:head>

{#if loading}
  <Loader />
{/if}

<div class="page">
  <SocialIcons />

  <Logo />

  <Polaroid />

  <MusicPlayer />

  <div class="bottom-text lacquer-regular">
    <p>i created this page on</p>
    <p>june 8, 2026</p>
    <p class="footnote">{">> claude helped me ;P"}</p>
  </div>
</div>

<style>
  :global(body) {
    margin: 0;
    min-height: 100vh;
    background-image: url('/web_background.png');
    background-size: 1366px auto;
    background-position: top left;
    background-repeat: repeat;
    background-attachment: fixed;
    overflow: hidden;
  }

  .page {
    position: relative;
    width: 100vw;
    height: 100vh;
    overflow: hidden;
  }

  .lacquer-regular {
    font-family: "Lacquer", system-ui;
    font-weight: 400;
    font-style: normal;
  }

  /* Bottom Text */
  .bottom-text {
    position: absolute;
    bottom: 2vh;
    right: 2vw;
    font-size: 22px;
    color: #2a2a2a;
  }

  .bottom-text p { margin: 1px; }

  .footnote { font-size: 14px; }

  @media (max-width: 1000px) {
    .bottom-text { font-size: 20px; }
  }

  @media (max-width: 679px) {
    :global(body) { overflow-y: auto; overflow-x: auto; }

    .page {
      display: flex;
      flex-direction: column;
      align-items: center;
      height: auto;
      min-height: 100vh;
      min-width: 300px;
      width: 100%;
      overflow: visible;
      padding: 5vh 20px 2vh;
      gap: 6vh;
      box-sizing: border-box;
    }

    .bottom-text {
      position: relative;
      bottom: 0;
      right: 5vw;
      order: 5;
      text-align: left;
      font-size: 16px;
      padding-top: 20px;
      padding-bottom: 10px;
      align-self: end;
    }

    .footnote { font-size: 12px; }
  }

  @media (max-width: 400px) {
    .page { gap: 3vh; }
  }

  @media (max-width: 325px) {
    .bottom-text { font-size: 12px; }
  }
</style>
