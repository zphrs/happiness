<script>
  import TocElem from "./TocElem.svelte"
  /** @type {HTMLElement[]}*/
  export let divs = []
  export let rc = null
  $: console.log(divs)
</script>

<ul>
  {#each divs as div}
    <TocElem {div} {rc}>
      {div ? div.querySelector("h2")?.textContent : ""}
    </TocElem>
  {/each}
</ul>

<style>
  ul {
    padding: 0;
    margin: 0;
    width: max-content;
    display: flex;
    flex-direction: column;
    align-items: stretch;
    list-style: none;
  }
</style>
