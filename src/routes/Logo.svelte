<script lang="ts">
  const logoFrames = ['/cat_eyes_open.png', '/cat_half_closed.png', '/cat_sleeping.png'] as const;

  let logoFrame = $state(0);
  let logoInterval: ReturnType<typeof setInterval> | undefined;
  let isBlinking = $state(false);

  export function startBlink() {
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

<div
  class="logo-section"
  role="img"
  aria-label="Lizzie Chai logo"
  onmouseenter={startBlink}
  onmouseleave={stopBlink}
  onclick={startBlink}
  data-gtm-click="logo"
>
  <img src={logoFrames[logoFrame]} alt="Lizzie Chai Logo" class:wobbling={isBlinking}>
  <span class="name-text covered-by-your-grace-regular">LIZZIE CHAI</span>
</div>

<style>
  .covered-by-your-grace-regular {
    font-family: "Covered By Your Grace", cursive;
    font-weight: 400;
    font-style: normal;
  }

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

  @media (max-width: 1200px) {
    .logo-section { left: 33%; top: 16%; }
  }

  @media (max-width: 1000px) {
    .logo-section     { left: 33%; top: 19%; }
    .logo-section img { width: 240px; height: 240px; }
    .name-text        { font-size: 25px; letter-spacing: 0; }
  }

  @media (max-width: 865px) {
    .logo-section     { left: 29%; top: 19%; }
    .logo-section img { width: 210px; height: 210px; }
    .name-text        { font-size: 23px; }
  }

  @media (max-width: 679px) {
    .logo-section {
      position: relative;
      left: auto;
      top: auto;
      order: 1;
    }

    .logo-section img { width: 180px; height: 180px; }
    .name-text        { font-size: 22px; }
  }

  @media (max-width: 325px) {
    .logo-section img { width: 150px; height: 150px; }
  }
</style>
