<script lang="ts">
  const socialLinks = [
    { href: 'https://linkedin.com/in/lizziechai11', src: '/linkedin_icon.png', alt: 'LinkedIn',  external: true,  track: 'social-linkedin'  },
    { href: 'https://instagram.com/lizziech41',     src: '/instagram_icon.png', alt: 'Instagram', external: true,  track: 'social-instagram' },
    { href: 'https://github.com/echai-11',          src: '/github_icon.png',    alt: 'GitHub',    external: true,  track: 'social-github'    },
    { href: 'mailto:contact@lizziechai.com',        src: '/gmail_icon.png',     alt: 'Email',     external: false, track: 'social-email'     },
  ] as const;
</script>

<div class="social-icons">
  {#each socialLinks as { href, src, alt, external, track }}
    <a {href} target={external ? '_blank' : undefined} rel={external ? 'noopener noreferrer' : undefined} data-gtm-click={track}>
      <img {src} {alt}>
    </a>
  {/each}
</div>

<style>
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

  @media (max-width: 1200px) {
    .social-icons { left: 10%; top: 13.4%; }
  }

  @media (max-width: 1000px) {
    .social-icons     { left: 10%; top: 16.4%; gap: 2vh; }
    .social-icons img { width: 80px; height: 80px; }
  }

  @media (max-width: 865px) {
    .social-icons     { left: 10%; top: 16%; }
    .social-icons img { width: 75px; height: 75px; }
  }

  @media (max-width: 679px) {
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
  }

  @media (max-width: 400px) {
    .social-icons { margin-top: -14px; }
  }

  @media (max-width: 325px) {
    .social-icons     { gap: 15px; }
    .social-icons img { width: 55px; height: 55px; }
  }
</style>
