<script>
  import Page from "$lib/Page.svelte";
  import Zine from "$lib/Zine.svelte";

  const percyPages = [
    "1.png",
    "2.png",
    "3.png",
    "4.png",
    "5.png",
    "6.png",
    "7.png",
    "8.png",
  ]
</script>

<div class="container">
  <Zine pages={percyPages} />
</div>

<style>
  :global(body) {
    margin: 0;
    padding: 0;
  }

  .container {
    height: 90dvh;

    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }
</style>