<script lang="ts">
  import { onMount } from 'svelte';
  import MusicPlayer from './MusicPlayer.svelte';
  import Polaroid from './Polaroid.svelte';
  import Loader from './Loader.svelte';

  const logoFrames = ['/cat_eyes_open.png', '/cat_half_closed.png', '/cat_sleeping.png'] as const;

  const socialLinks = [
    { href: 'https://linkedin.com/in/lizziechai11', src: '/linkedin_icon.png', alt: 'LinkedIn',  external: true  },
    { href: 'https://instagram.com/lizziech41',     src: '/instagram_icon.png', alt: 'Instagram', external: true  },
    { href: 'https://github.com/echai-11',          src: '/github_icon.png',    alt: 'GitHub',    external: true  },
    { href: 'mailto:contact@lizziechai.com',        src: '/gmail_icon.png',     alt: 'Email',     external: false },
  ] as const;

  let logoFrame = $state(0);
  let logoInterval: ReturnType<typeof setInterval> | undefined;
  let isBlinking = $state(false);
  let loading = $state(true);

  onMount(() => {
    logoFrames.forEach(src => { const img = new Image(); img.src = src; });

    if (document.readyState === 'complete') {
      loading = false;
    } else {
      window.addEventListener('load', () => { loading = false; }, { once: true });
    }
  });

  function startBlink() {
    clearInterval(logoInterval);
    isBlinking = true;
    let i = 0;
    logoInterval = setInterval(() => {
      i++;
      logoFrame = i % logoFrames.length;
      if (i >= logoFrames.length - 1) {
        clearInterval(logoInterval);
        setTimeout(() => { logoFrame = 0; isBlinking = false; }, 50);
      }
    }, 150);
  }

  function stopBlink() {
    clearInterval(logoInterval);
    logoFrame = 0;
    isBlinking = false;
  }
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
  <div class="social-icons">
    {#each socialLinks as { href, src, alt, external }}
      <a {href} target={external ? '_blank' : undefined} rel={external ? 'noopener noreferrer' : undefined}>
        <img {src} {alt}>
      </a>
    {/each}
  </div>

  <div class="logo-section" role="img" aria-label="Lizzie Chai logo" onmouseenter={startBlink} onmouseleave={stopBlink}>
    <img src={logoFrames[logoFrame]} alt="Lizzie Chai Logo" class:wobbling={isBlinking}>
    <span class="name-text covered-by-your-grace-regular">LIZZIE CHAI</span>
  </div>

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

  .covered-by-your-grace-regular {
    font-family: "Covered By Your Grace", cursive;
    font-weight: 400;
    font-style: normal;
  }

  /* Social Icons */
  .social-icons {
    position: absolute;
    left: 10%;
    top: 10.4%;
    display: flex;
    flex-direction: column;
    gap: 3vh;
    align-items: center;
    width: 14.6vw;
  }

  .social-icons img {
    width: 90px;
    height: 90px;
    object-fit: contain;
    display: block;
    transition: transform 0.2s;
  }

  .social-icons a:first-child  { margin-left: 39px; }
  .social-icons a:nth-child(3) { margin-left: 9px; }
  .social-icons a:nth-child(4) { margin-left: 16px; }

  @keyframes wobble {
    0%   { transform: scale(1) rotate(0deg); }
    20%  { transform: scale(1.12) rotate(-7deg); }
    40%  { transform: scale(1.10) rotate(6deg); }
    60%  { transform: scale(1.11) rotate(-4deg); }
    80%  { transform: scale(1.09) rotate(2deg); }
    100% { transform: scale(1.08) rotate(0deg); }
  }

  @media (hover: hover) {
    .social-icons a:hover img {
      animation: wobble 0.5s ease forwards;
    }
  }

  /* Logo */
  .logo-section {
    position: absolute;
    left: 33.3%;
    top: 13%;
    transform: rotate(-13.7deg);
    display: flex;
    flex-direction: column;
    align-items: center;
    cursor: pointer;
  }

  .logo-section img {
    width: 250px;
    height: 250px;
    object-fit: contain;
    display: block;
  }

  @keyframes logo-wobble {
    0%   { transform: rotate(0deg); }
    25%  { transform: rotate(-2.5deg); }
    50%  { transform: rotate(0deg); }
    75%  { transform: rotate(2.5deg); }
    100% { transform: rotate(0deg); }
  }

  .logo-section img.wobbling {
    animation: logo-wobble 0.5s ease-in-out infinite;
  }

  .name-text {
    font-size: 30px;
    color: #1a1a1a;
    letter-spacing: -1px;
    line-height: 1;
    text-align: right;
    align-self: end;
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

  /* Responsive */
  @media (max-width: 1200px) {
    .social-icons { left: 10%; top: 13.4%; }
    .logo-section { left: 33%; top: 16%; }
  }

  @media (max-width: 1000px) {
    .social-icons     { left: 10%; top: 16.4%; gap: 2vh; }
    .social-icons img { width: 80px; height: 80px; }
    .logo-section     { left: 33%; top: 19%; }
    .logo-section img { width: 240px; height: 240px; }
    .name-text        { font-size: 25px; letter-spacing: 0; }
    .bottom-text      { font-size: 20px; }
  }

  @media (max-width: 865px) {
    .social-icons     { left: 10%; top: 16%; }
    .social-icons img { width: 75px; height: 75px; }
    .logo-section     { left: 33%; top: 19%; }
    .logo-section img { width: 210px; height: 210px; }
    .name-text        { font-size: 23px; }
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

    .logo-section {
      position: relative;
      left: auto;
      top: auto;
      order: 1;
    }

    .logo-section img { width: 180px; height: 180px; }
    .name-text        { font-size: 22px; }

    .social-icons {
      position: relative;
      left: auto;
      top: auto;
      order: 2;
      margin-top: -27px;
      flex-direction: row;
      justify-content: center;
      align-items: flex-start;
      width: 100%;
      gap: 5vw;
    }

    .social-icons img            { width: 65px; height: 65px; }
    .social-icons a:first-child  { margin-left: 0; margin-top: 0; }
    .social-icons a:nth-child(2) { margin-top: 18px; }
    .social-icons a:nth-child(3) { margin-left: 0; margin-top: 8px; }
    .social-icons a:nth-child(4) { margin-left: 0; margin-top: 15px; }

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
    .page         { gap: 3vh; }
    .social-icons { margin-top: -14px; }
  }

  @media (max-width: 325px) {
    .logo-section img { width: 150px; height: 150px; }
    .social-icons     { gap: 15px; }
    .social-icons img { width: 55px; height: 55px; }
    .bottom-text      { font-size: 12px; }
  }
</style>
