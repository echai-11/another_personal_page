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
        },
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

  function scrubKeydown(e: KeyboardEvent) {
    if (e.key === 'ArrowRight') {
      currentTime = Math.min(duration, currentTime + 5);
      player?.seekTo(currentTime, true);
    } else if (e.key === 'ArrowLeft') {
      currentTime = Math.max(0, currentTime - 5);
      player?.seekTo(currentTime, true);
    }
  }

  const progress = $derived(duration ? (currentTime / duration) * 100 : 0);
</script>

<!-- SVG filter for hand-drawn border effect -->
<svg style="position:absolute;width:0;height:0;overflow:hidden">
  <defs>
    <filter id="sketchy" x="-5%" y="-30%" width="110%" height="160%">
      <feTurbulence type="fractalNoise" baseFrequency="0.04" numOctaves="3" seed="8" result="noise"/>
      <feDisplacementMap in="SourceGraphic" in2="noise" scale="2.5" xChannelSelector="R" yChannelSelector="G"/>
    </filter>
  </defs>
</svg>

<div class="yt-hidden">
  <div id="yt-player"></div>
</div>

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
    <div
      class="scrubber-wrap"
      onclick={seek}
      onkeydown={scrubKeydown}
      role="slider"
      tabindex="0"
      aria-valuenow={progress}
      aria-valuemin={0}
      aria-valuemax={100}
    >
      <svg width="100%" height="18" style="filter:url(#sketchy)">
        <line x1="0" y1="9" x2="100%" y2="9" stroke="#040404" stroke-width="3" stroke-linecap="round"/>
        <line x1="0" y1="9" x2="{progress}%" y2="9" stroke="#040404" stroke-width="3" stroke-linecap="round"/>
        <circle cx="{progress}%" cy="9" r="6" fill="#0A66C2"/>
      </svg>
    </div>
  </div>
</div>

<style>
  .yt-hidden {
    position: fixed;
    left: -9999px;
    width: 200px;
    height: 150px;
    pointer-events: none;
  }

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
    color: #040404;
    margin: 0;
    background-color: #ffc300;
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

  @media (max-width: 1200px) {
    .music-player { left: 38%; }
  }

  @media (max-width: 1000px) {
    .music-player { left: 34%; bottom: 10vh; width: 275px; }
  }

  @media (max-width: 865px) {
    .music-player { width: 260px; }
  }

  @media (max-width: 679px) {
    .music-player {
      position: relative;
      bottom: auto;
      left: 10%;
      width: 280px;
      min-width: 210px;
      margin-top: 36px;
      align-self: start;
      order: 4;
    }
  }

  @media (max-width: 400px) {
    .music-player { left: auto; width: 70vw; }
  }
</style>
