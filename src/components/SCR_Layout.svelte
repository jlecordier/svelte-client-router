<main class="scr-main-layout">
  <slot name="scr_header">
    <h1 class="scr-main-layout__header">Svelte Client Router - The Svelte SPA Router!</h1>
  </slot>
  <slot />
</main>

<style>
  .scr-main-layout {
    display: grid;
    grid-template-columns: 1fr;
    justify-content: center;
    align-content: center;
    margin: 1rem 0;
    padding: 1rem 0;
    width: 100%;
    min-height: 1vh;
  }

  .scr-main-layout__header {
    text-align: center;
    color: #ff3e00;
    border-bottom: 1px solid #000;
    padding: 1rem 0;
  }
</style>
