<script>
  let { flipped, text = ["no label", "no label"]} = $props()
</script>

<div class:flipped class="page">
  <div class="front">
    <img src={text[0]} alt="O:" >
  </div>
  <div class="back">
    <img src={text[1]} alt="O:" >
  </div>
</div>

<style>
  .page {
    position: relative;
    transform-style: preserve-3d;
    transform-origin: left;
    transition: all 0.8s ease-in-out;
    width: var(--page-width);
    aspect-ratio: 2 / 3;
    box-shadow: 2px 2px 5px rgba(170, 170, 170, 0.643);


    div {
      width: 100%;
      height: 100%;
      position: absolute;
      backface-visibility: hidden;
    }
  }

  .flipped {
    rotate: y 180deg;
  }

  .back {
    rotate: y 180deg;
  }

  img {
    width: 100%;
    height: 100%;

    object-fit: fill;
  }
</style>