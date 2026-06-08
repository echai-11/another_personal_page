<script lang="ts">
  import { onMount, onDestroy } from 'svelte';

  let player: any = null;
  let isPlaying = $state(false);
  let currentTime = $state(0);
  let duration = $state(0);
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
    position: fixed;
    left: 80px;
    margin-top:80px;
    display: flex;
    flex-direction: column;
    gap: 3vh;
    align-items: center;
    width:200px;
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


  .social-icons a:hover img {
    transform: scale(1.08);
  }

  /* Logo + Name */
  .logo-section {
    transform:rotate(-13.7deg);
    display: flex;
    flex-direction: column;
    align-items: center;
    margin-top:100px;
    margin-left:400px;
    position:fixed;
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
    margin-top: 4px;
    text-align:right;
    align-self:end;
  }

  /* Polaroid */
  .polaroid-wrapper {
   position: fixed;
  right: 180px;
  top: 75px;
    transform: rotate(11.4deg);
  }

  .polaroid-photo-container {
    width: 230px;
    height: 264px;
    overflow: hidden;
    display:flex;
    flex-direction:row;
    justify-content:center;
  }

  .polaroid-photo {
    width:272px;
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
    left: 33%;
    background: #f5e8c8;
    border-radius: 18px;
    padding: 14px 20px 20px 20px;
    width: 320px;
    box-shadow: 2px 3px 10px rgba(0, 0, 0, 0.1);
  }

  .player-dots {
    display: flex;
    justify-content: flex-end;
    gap: 5px;
    margin-bottom: 10px;
  }

  .player-track-info {
    margin-bottom: 12px;
  }

  .player-song {
    font-family: "Lacquer", system-ui;
    font-size: 13px;
    color: #3a2e20;
    margin: 0;
    text-transform: lowercase;
  }

  .player-artist {
    font-size: 11px;
    color: #7a6a58;
    margin: 2px 0 0 0;
    text-transform: lowercase;
  }

  .player-dot {
    width: 7px;
    height: 7px;
    background: #7a6a58;
    border-radius: 50%;
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
    bottom: 9vh;
    right: 5vw;
    font-size: 22px;
    color: #2a2a2a;
  }
  .bottom-text p {
    margin:0;
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
    <a href="https://linkedin.com/in/lizziechai" target="_blank" rel="noopener noreferrer">
      <img src="/linkedin_icon.png" alt="LinkedIn">
    </a>
    <a href="https://instagram.com" target="_blank" rel="noopener noreferrer">
      <img src="/instagram_icon.png" alt="Instagram">
    </a>
    <a href="https://github.com/lizziechai" target="_blank" rel="noopener noreferrer">
      <img src="/github_icon.png" alt="GitHub">
    </a>
    <a href="mailto:lizzie.chai@gmail.com">
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
        <img src="/LizzieAndDodo1.png" alt="Lizzie and Dodo" class="polaroid-photo">
      </div>
    </div>
  </div>

  <!-- Music Player -->
  <div class="music-player">
    <div class="player-track-info">
      <p class="player-song">look to windward // sleep token</p>
    </div>
    <div class="player-row">
      <button class="play-btn" onclick={togglePlay} aria-label={isPlaying ? 'Pause' : 'Play'}>
        <svg width="46" height="46" viewBox="0 0 46 46" fill="none" style="filter:url(#sketchy)">
          <circle cx="23" cy="23" r="19" stroke="#4a4030" stroke-width="2.2"/>
          {#if isPlaying}
            <rect x="16" y="14" width="5" height="18" rx="1" fill="#4a4030"/>
            <rect x="25" y="14" width="5" height="18" rx="1" fill="#4a4030"/>
          {:else}
            <polygon points="18,13 37,23 18,33" fill="#4a4030"/>
          {/if}
        </svg>
      </button>
      <div class="scrubber-wrap" onclick={seek} onkeydown={(e: KeyboardEvent) => { if (e.key === 'ArrowRight') { currentTime = Math.min(duration, currentTime + 5); player?.seekTo(currentTime, true); } else if (e.key === 'ArrowLeft') { currentTime = Math.max(0, currentTime - 5); player?.seekTo(currentTime, true); } }} role="slider" tabindex="0" aria-valuenow={progress} aria-valuemin={0} aria-valuemax={100}>
        <svg width="100%" height="18" style="filter:url(#sketchy)">
          <line x1="0" y1="9" x2="100%" y2="9" stroke="#c8b89a" stroke-width="3" stroke-linecap="round"/>
          <line x1="0" y1="9" x2="{progress}%" y2="9" stroke="#4a4030" stroke-width="3" stroke-linecap="round"/>
          <circle cx="{progress}%" cy="9" r="6" fill="#4a4030"/>
        </svg>
      </div>
    </div>
  </div>

  <!-- Bottom Text -->
  <div class="bottom-text lacquer-regular"><p>i created this page on</p><p>june 8, 2026</p></div>
</div>
