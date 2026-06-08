<script lang="ts">
  let flipped = $state(false);
</script>

<div class="polaroid-wrapper">
  <div class="polaroid-photo-container">
    <div
      class="flip-card"
      class:flipped
      onclick={() => (flipped = !flipped)}
      role="button"
      tabindex="0"
      onkeydown={(e) => e.key === 'Enter' && (flipped = !flipped)}
    >
      <div class="flip-front">
        <img src="/LizzieAndDodo1.png" alt="Lizzie and Dodo" class="polaroid-photo">
      </div>
      <div class="flip-back">
        <img src="/LizzieAndDodo2.png" alt="Lizzie and Dodo" class="polaroid-photo">
      </div>
    </div>
  </div>
</div>

<style>
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

  @media (max-width: 1200px) {
    .polaroid-wrapper { right: 10%; top: 26.8%; }
  }

  @media (max-width: 1000px) {
    .polaroid-wrapper         { right: 5%; top: 29.8%; }
    .polaroid-photo-container { width: 212px; height: 246px; }
    .polaroid-photo           { width: 252px; }
  }

  @media (max-width: 865px) {
    .polaroid-wrapper         { right: 4%; top: 29%; }
    .polaroid-photo-container { width: 190px; height: 218px; }
    .polaroid-photo           { width: 226px; }
  }

  @media (max-width: 679px) {
    .polaroid-wrapper {
      position: relative;
      top: auto;
      right: 15px;
      transform: rotate(4deg);
      padding-top: 10px;
      order: 3;
    }
    .polaroid-photo-container { width: 200px; height: 230px; }
    .polaroid-photo           { width: 238px; }
  }
</style>
