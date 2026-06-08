<div class="loader-overlay">
  <div class="loader">
    <div class="bar bar1"></div>
    <div class="bar bar2"></div>
    <div class="bar bar3"></div>
    <div class="bar bar4"></div>
    <div class="bar bar5"></div>
    <div class="bar bar6"></div>
  </div>
 <div class="loader-text">
    <p >{">> loading"}</p>
  </div>
</div>
 

<style>
  .loader-overlay {
    position: fixed;
    inset: 0;
    z-index: 9999;
    background:#1E1D19;
    display: flex;
    flex-direction:column;
    align-items: center;
    justify-content: center;
  }

  .loader {
    width: 80px;
    height: 70px;
    display: flex;
    align-items: center;
    gap: 2px;
  }

  .bar {
    flex: 1;
    height: 100%;
    animation: bounce 0.8s infinite ease-in-out;
  }

  .bar1 { background-color: #754fa0; }
  .bar2 { background-color: #09b7bf; animation-delay: -0.7s; }
  .bar3 { background-color: #90d36b; animation-delay: -0.6s; }
  .bar4 { background-color: #f2d40d; animation-delay: -0.5s; }
  .bar5 { background-color: #fcb12b; animation-delay: -0.4s; }
  .bar6 { background-color: #ed1b72; animation-delay: -0.3s; }

  .loader-text {
    color: white;
    font-family: 'Courier New', Courier, monospace;
    font-size: 1rem;
    display:block;
  }

  @keyframes bounce {
    0%, 40%, 100% { transform: scaleY(0.05); }
    20%            { transform: scaleY(1.0); }
  }
</style>
