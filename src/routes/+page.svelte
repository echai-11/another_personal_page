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
    margin-bottom: 14px;
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

  .play-circle {
    width: 40px;
    height: 40px;
    border-radius: 50%;
    border: 2.5px solid #4a4030;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-shrink: 0;
    cursor: pointer;
  }

  .play-triangle {
    width: 0;
    height: 0;
    border-top: 9px solid transparent;
    border-bottom: 9px solid transparent;
    border-left: 15px solid #4a4030;
    margin-left: 4px;
  }

  .scrubber {
    flex: 1;
    height: 2px;
    background: #4a4030;
    border-radius: 2px;
    position: relative;
  }

  .scrubber-thumb {
    position: absolute;
    left: 28%;
    top: 50%;
    transform: translateY(-50%);
    width: 11px;
    height: 11px;
    background: #4a4030;
    border-radius: 50%;
    margin-left: -5.5px;
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
    <div class="player-dots">
      <div class="player-dot"></div>
      <div class="player-dot"></div>
      <div class="player-dot"></div>
    </div>
    <div class="player-row">
      <div class="play-circle">
        <div class="play-triangle"></div>
      </div>
      <div class="scrubber">
        <div class="scrubber-thumb"></div>
      </div>
    </div>
  </div>

  <!-- Bottom Text -->
  <div class="bottom-text lacquer-regular"><p>i created this page on</p><p>june 8, 2026</p></div>
</div>
