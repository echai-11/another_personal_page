<script lang="ts">
  import { onMount, onDestroy } from 'svelte';

  let player: any = null;
  let isPlaying = $state(false);
  let currentTime = $state(0);
  let duration = $state(0);
  let flipped = $state(false);
  let progressInterval: ReturnType<typeof setInterval> | undefined;

  onMount(() => {
    function initPlayer() {
      player = new (window as any).YT.Player('yt-player', {
        videoId: 'Qw0bebAzsYc',
        playerVars: {
          autoplay: 0,
          controls: 0,
          disablekb: 1,
          fs: 0,
          iv_load_policy: 3,
          modestbranding: 1,
          rel: 0,
          playsinline: 1,
        },
        events: {
          onReady: (e: any) => { duration = e.target.getDuration(); },
          onStateChange: onStateChange,
        }
      });
    }

    if ((window as any).YT?.Player) {
      initPlayer();
    } else {
      (window as any).onYouTubeIframeAPIReady = initPlayer;
      if (!document.querySelector('script[src="https://www.youtube.com/iframe_api"]')) {
        const tag = document.createElement('script');
        tag.src = 'https://www.youtube.com/iframe_api';
        document.head.appendChild(tag);
      }
    }
  });

  onDestroy(() => {
    clearInterval(progressInterval);
    player?.destroy();
    player = null;
  });

  function onStateChange(event: any) {
    if (event.data === 1) {
      isPlaying = true;
      progressInterval = setInterval(() => {
        currentTime = player.getCurrentTime();
        duration = player.getDuration();
      }, 250);
    } else {
      isPlaying = false;
      clearInterval(progressInterval);
      if (event.data === 0) currentTime = 0;
    }
  }

  function togglePlay() {
    if (!player) return;
    isPlaying ? player.pauseVideo() : player.playVideo();
  }

  function seek(event: MouseEvent) {
    if (!player || !duration) return;
    const rect = (event.currentTarget as HTMLElement).getBoundingClientRect();
    const pct = Math.max(0, Math.min(1, (event.clientX - rect.left) / rect.width));
    player.seekTo(pct * duration, true);
    currentTime = pct * duration;
  }

  const progress = $derived(duration ? (currentTime / duration) * 100 : 0);
</script>

<svelte:head>
 <link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin="anonymous">
<link href="https://fonts.googleapis.com/css2?family=Covered+By+Your+Grace&family=Lacquer&display=swap" rel="stylesheet">
</svelte:head>

<style>
  :global(body) {
    margin: 0;
    min-height: 100vh;
    background-image: url('/web_background.png');
    background-size: 100%;
    background-position: center;
    background-repeat: repeat-y;
    background-attachment: fixed;
    overflow:hidden;
  }

  .page {
    position: relative;
    width: 100vw;
    height: 100vh;
    overflow: hidden;
  }

  /* Fonts */
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


  .social-icons a:first-child{
    margin-left:39px;
  }


  .social-icons a:nth-child(3){
    margin-left:9px;
  }

   .social-icons a:nth-child(4){
    margin-left:16px;
  }


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

  /* Logo + Name */
  .logo-section {
    position: absolute;
    left: 33.3%;
    top: 13%;
    transform: rotate(-13.7deg);
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .logo-section img {
    width: 250px;
    height: auto;
  }

  .name-text {
    font-size: 30px;
    color: #1a1a1a;
    letter-spacing: -1px;
    line-height: 1;
    text-align:right;
    align-self:end;
  }

  /* Polaroid */
  .polaroid-wrapper {
    position: absolute;
    right: 15.2%;
    top: 23.8%;
    transform: rotate(11.4deg);
  }

  .polaroid-photo-container {
    width: 230px;
    height: 264px;
    perspective: 800px;
  }

  .flip-card {
    width: 100%;
    height: 100%;
    position: relative;
    transform-style: preserve-3d;
    transition: transform 0.7s ease;
    cursor: pointer;
  }

  .flip-card.flipped {
    transform: rotateY(180deg);
  }

  .flip-front,
  .flip-back {
    position: absolute;
    width: 100%;
    height: 100%;
    backface-visibility: hidden;
    -webkit-backface-visibility: hidden;
    overflow: hidden;
    display: flex;
    justify-content: center;
     box-shadow: 6px 8px 24px rgba(0, 0, 0, 0.25), 2px 3px 8px rgba(0, 0, 0, 0.15);
  }

  .flip-back {
    transform: rotateY(180deg);
  }

  .polaroid-photo {
    width: 272px;
    height: auto;
    object-fit: cover;
    flex-shrink: 0;
  }

  /* Hidden YouTube player */
  .yt-hidden {
    position: fixed;
    left: -9999px;
    width: 200px;
    height: 150px;
    pointer-events: none;
  }

  /* Music Player */
  .music-player {
    position: absolute;
    bottom: 9vh;
    left: 41%;
    background: transparent;
    border-radius: 18px;
    padding: 14px 20px 20px 20px;
    width: 320px;
    transform: rotate(-1.6deg);
  }

  .music-player::before {
    content: '';
    position: absolute;
    inset: 0;
    border: 2.5px solid #4a4030;
    border-radius: 18px;
    filter: url(#sketchy);
    pointer-events: none;
  }

  .player-track-info {
    margin-bottom: 12px;
  }

  .player-song {
    font-family: "Lacquer", system-ui;
    font-size: 13px;
    color:#040404;
    margin: 0;
    background-color:#ffc300;
  }


  .player-row {
    display: flex;
    align-items: center;
    gap: 14px;
  }

  .play-btn {
    background: none;
    border: none;
    padding: 0;
    cursor: pointer;
    flex-shrink: 0;
    display: flex;
  }

  .scrubber-wrap {
    flex: 1;
    cursor: pointer;
    padding: 6px 0;
  }

  .scrubber-wrap svg {
    display: block;
    overflow: visible;
  }

  /* Bottom Text */
  .bottom-text {
    position: absolute;
    bottom: 2vh;
    right: 2vw;
    font-size: 22px;
    color: #2a2a2a;
  }
  .bottom-text p {
    margin:1px;
  }

  /* ── 1200px ── */
  @media (max-width: 1200px) {
    .social-icons     { left: 10%; top: 13.4%; }
    .logo-section     { left: 33%; top: 16%;   }
    .polaroid-wrapper { right: 10%; top: 26.8%; }
    .music-player     { left: 38%; }
  }

  /* ── 1000px ── */
  @media (max-width: 1000px) {
    .social-icons          { left: 10%; top: 16.4%; gap: 2vh; }
    .social-icons img      { width: 80px; height: 80px; }
    .logo-section          { left: 33%; top: 19%; }
    .logo-section img      { width: 240px; }
    .name-text             { font-size: 25px; letter-spacing: 0}
    .polaroid-wrapper      { right: 5%; top: 29.8%; }
    .polaroid-photo-container { width: 212px; height: 246px; }
    .polaroid-photo        { width: 252px; }
    .music-player          { left: 34%; bottom: 10vh; width: 275px; }
    .bottom-text           { font-size: 20px; }
  }

  /* ── 865px ── */
  @media (max-width: 865px) {
    .social-icons          { left: 10%; top: 16%; }
    .social-icons img      { width: 75px; height: 75px; }
    .logo-section          { left: 33%; top: 19%; }
    .logo-section img      { width: 210px; }
    .name-text             { font-size: 23px; }
    .polaroid-wrapper      { right: 4%; top: 29%; }
    .polaroid-photo-container { width: 190px; height: 218px; }
    .polaroid-photo        { width: 226px; }
    .music-player          { width: 260px; }
  }


  /* ── 600px — column layout ── */
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
      left: auto; top:auto;
      order: 1;
    }

    .social-icons { order: 2; }
    .polaroid-wrapper { order: 3; }
    .music-player { order: 4; }
    .bottom-text { order: 5; }
    .logo-section img { width: 180px; }
    .name-text { font-size: 22px; }

    .social-icons {
      position: relative;
      left: auto; top: auto;
      margin-top: -27px;
      flex-direction: row;
      justify-content: center;
      align-items: flex-start;
      width: 100%;
      gap: 5vw;
    }
    .social-icons img { width: 65px; height: 65px; }
    .social-icons a:first-child { margin-left: 0; margin-top: 0px; }
    .social-icons a:nth-child(2) { margin-top: 18px; }
    .social-icons a:nth-child(3) { margin-left: 0; margin-top: 8px; }
    .social-icons a:nth-child(4) { margin-left: 0; margin-top: 15px; }

    .polaroid-wrapper {
      position: relative;
     top: auto;
      transform: rotate(4deg);
      right: 15px;
      padding-top: 10px;
    }
    .polaroid-photo-container { width: 200px; height: 230px; }
    .polaroid-photo { width: 238px; }

    .music-player {
      position: relative;
      bottom: auto; left:10%;
      width: 280px;
      min-width:210px;
      margin-top:36px;
      align-self:start;
    }

    .bottom-text {
      position: relative;
      bottom: 0px; right: 5vw;
      text-align: left;
      font-size: 16px;
      padding-top:20px;
      padding-bottom:10px;
        align-self:end;
    }
  }
  /* ── 400px ── */
  @media (max-width: 400px) {
        .page {gap:3vh;}
        .social-icons {margin-top:-14px;}
    .music-player { left: auto; width: 70vw; }
    .polaroid-photo-container { width: min(200px, 62vw); height: min(230px, 71.5vw); }
    .polaroid-photo { width: min(238px, 74vw); }
  }

  /* ── 320px ── */
  @media (max-width: 325px) {


    .logo-section img { width: 150px; }
    .social-icons {gap:15px;}
    .social-icons img { width: 55px; height: 55px; }
    .bottom-text {font-size:12px;}
  }


</style>

<!-- SVG filter for hand-drawn effect -->
<svg style="position:absolute;width:0;height:0;overflow:hidden">
  <defs>
    <filter id="sketchy" x="-5%" y="-30%" width="110%" height="160%">
      <feTurbulence type="fractalNoise" baseFrequency="0.04" numOctaves="3" seed="8" result="noise"/>
      <feDisplacementMap in="SourceGraphic" in2="noise" scale="2.5" xChannelSelector="R" yChannelSelector="G"/>
    </filter>
  </defs>
</svg>

<!-- Hidden YouTube player -->
<div class="yt-hidden">
  <div id="yt-player"></div>
</div>

<div class="page">
  <!-- Social Icons -->
  <div class="social-icons">
    <a href="https://linkedin.com/in/lizziechai11" target="_blank" rel="noopener noreferrer">
      <img src="/linkedin_icon.png" alt="LinkedIn">
    </a>
    <a href="https://instagram.com/lizziech41" target="_blank" rel="noopener noreferrer">
      <img src="/instagram_icon.png" alt="Instagram">
    </a>
    <a href="https://github.com/echai-11" target="_blank" rel="noopener noreferrer">
      <img src="/github_icon.png" alt="GitHub">
    </a>
    <a href="mailto:contact@lizziechai.com">
      <img src="/gmail_icon.png" alt="Email">
    </a>
  </div>

  <!-- Logo + Name -->
  <div class="logo-section">
    <img src="/LizzieChaiLogo.png" alt="Lizzie Chai Logo">
    <span class="name-text covered-by-your-grace-regular">LIZZIE CHAI</span>
  </div>

  <!-- Polaroid -->
  <div class="polaroid-wrapper">
    <div class="polaroid">
      <div class="polaroid-photo-container">
        <div class="flip-card" class:flipped onclick={() => flipped = !flipped} role="button" tabindex="0" onkeydown={(e) => e.key === 'Enter' && (flipped = !flipped)}>
          <div class="flip-front">
            <img src="/LizzieAndDodo1.png" alt="Lizzie and Dodo" class="polaroid-photo">
          </div>
          <div class="flip-back">
            <img src="/LizzieAndDodo2.png" alt="Lizzie and Dodo" class="polaroid-photo">
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- Music Player -->
  <div class="music-player">
    <div class="player-track-info">
      <span class="player-song">look to windward // sleep token</span>
    </div>
    <div class="player-row">
      <button class="play-btn" onclick={togglePlay} aria-label={isPlaying ? 'Pause' : 'Play'}>
        <svg width="46" height="46" viewBox="0 0 46 46" fill="none" style="filter:url(#sketchy)">
          <circle cx="23" cy="23" r="19" stroke="#040404" stroke-width="2.2"/>
          {#if isPlaying}
            <rect x="16" y="14" width="5" height="18" rx="1" fill="#040404"/>
            <rect x="25" y="14" width="5" height="18" rx="1" fill="#040404"/>
          {:else}
            <polygon points="18,13 37,23 18,33" fill="#040404"/>
          {/if}
        </svg>
      </button>
      <div class="scrubber-wrap" onclick={seek} onkeydown={(e: KeyboardEvent) => { if (e.key === 'ArrowRight') { currentTime = Math.min(duration, currentTime + 5); player?.seekTo(currentTime, true); } else if (e.key === 'ArrowLeft') { currentTime = Math.max(0, currentTime - 5); player?.seekTo(currentTime, true); } }} role="slider" tabindex="0" aria-valuenow={progress} aria-valuemin={0} aria-valuemax={100}>
        <svg width="100%" height="18" style="filter:url(#sketchy)">
          <line x1="0" y1="9" x2="100%" y2="9" stroke="#040404" stroke-width="3" stroke-linecap="round"/>
          <line x1="0" y1="9" x2="{progress}%" y2="9" stroke="#040404" stroke-width="3" stroke-linecap="round"/>
          <circle cx="{progress}%" cy="9" r="6" fill="#0A66C2"/>
        </svg>
      </div>
    </div>
  </div>

  <!-- Bottom Text -->
  <div class="bottom-text lacquer-regular"><p>i created this page on</p><p>june 8, 2026</p></div>
</div>
