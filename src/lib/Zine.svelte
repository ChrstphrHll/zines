<script lang="ts">
  import Page from "./Page.svelte";

  let { pages } = $props()
  let collatedPages = $derived.by(()=> {
    let collated = []
    let tempPage = []
    for (const page of pages) {
      tempPage.push(page)
      if (tempPage.length === 2) {
        collated.push(tempPage)
        tempPage = []
      }
    }
    if (tempPage.length !== 0) {
      collated.push(tempPage)
    }
    return collated
  })

  let currentPage = $state(0)
  let flippingPage = $state(1)

  function setCurrentPage(pageIndex: number) {
    if (pageIndex > 4 || pageIndex < 0) {
      return
    }


    if (pageIndex - currentPage < 0) {
      flippingPage = pageIndex + 1
    } else {
      flippingPage = pageIndex
    }

    currentPage = pageIndex
  }
  
  function isFlipped(pageIndex: number) {
    if (currentPage > pageIndex) {
      return true
    }
    return false
  }

  function getZIndex(pageIndex: number) {
    console.log("getting zindex for", pageIndex)
    if (flippingPage === pageIndex) {
      return 2
    }

    const diffFromFlipping = Math.abs(pageIndex - flippingPage)
    if (diffFromFlipping === 1) {
      return 1
    }

    return 0
  }

  function shrinkOnFirstAndLast() {
    if (currentPage === 0) {
      return "translate: calc(-0.5 * var(--page-width));"
    }

    if (currentPage === collatedPages.length) {
      return "translate: calc(0.5 * var(--page-width));"
    }
    return ""
  }
</script>

<div class="container">
  <div class="book" style="{shrinkOnFirstAndLast()}">
    {#each collatedPages as page, index }
      <div class="page-container" style="z-index:{getZIndex(index + 1)};">
        <Page text={page} flipped={isFlipped(index)} />
      </div>
    {/each}
  </div>

  <div>
    <button onclick={() => setCurrentPage(currentPage - 1)}>&lt;</button>
    <button onclick={() => setCurrentPage(currentPage + 1)}>&gt;</button>
  </div>
</div>

<style>
  :global(:root) {
    --page-width: min(300px, 45vw);
  }

  .page-container {
    position: absolute;
    right: 0;
  }

  .book{
    position: relative;
    width: calc(var(--page-width) * 2);
    height: calc(var(--page-width) * 1.5);

    transition: all 0.8s ease-in-out;
  }

  .container {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 16px;
  }
</style>