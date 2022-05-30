<script>
  import { onMount } from "svelte"

  let progress =
      window.scrollY / (document.body.scrollHeight - window.innerHeight),
    outer,
    headers = []
  $: outer && outer.style.setProperty("--progress", progress * 100 + "%")
  function setProgress() {
    progress =
      window.scrollY / (document.body.scrollHeight - window.innerHeight)
    outer && outer.style.setProperty("--progress", progress * 100 + "%")
    headers = headers
  }
  window.addEventListener("scroll", setProgress)
  document.addEventListener("click", setProgress)
  onMount(() => {
    window.setTimeout(() => {
      headers = [...document.querySelectorAll("h2, h3, h4, h5, h6")]
      console.log("HEADERS", headers)
    }, 0)
  })
</script>

<div bind:this={outer} class="outer">
  <div class="inner" />
  {#each headers as header}
    <div
      class="header"
      class:passed={header.parentElement.getBoundingClientRect().top +
        header.parentElement.getBoundingClientRect().height <
        0}
      class:active={progress >
        (window.scrollY + header.parentElement.getBoundingClientRect().top) /
          document.body.scrollHeight}
      style="--position: {((window.scrollY +
        header.parentElement.getBoundingClientRect().top) /
        document.body.scrollHeight) *
        100}%; --height: {(header.parentElement.getBoundingClientRect().height /
        document.body.scrollHeight) *
        100}%;"
      on:click={function () {
        header.parentElement.scrollIntoView({
          behavior: "smooth",
          block: "start",
        })
      }}
      on:touchmove={function (e) {
        console.log("TOUCH", e)

        header.parentElement.scrollIntoView({
          behavior: "smooth",
          block: "start",
        })
      }}
    />
  {/each}
</div>

<style>
  div {
    position: fixed;
    top: 0rem;
    left: 0rem;
    width: 0.25rem;
    height: 100%;
    box-sizing: border-box;
  }
  .header {
    position: fixed;
    top: calc(var(--position) - 0.75rem);
    left: 0.25rem;

    height: 1.5rem;
    width: 1.5rem;
    border-radius: 0rem;
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 50%;
    cursor: pointer;
    transition: background-color 0.4s;
  }
  .header:hover {
    background-color: rgb(var(--accent), 0.2);
  }
  .header::after {
    content: "";
    position: absolute;
    width: 1rem;
    height: 1rem;
    border-radius: 2rem;
    border: 2px solid var(--accent-color);
    transition: background-color 0.4s;
  }
  .header.active::after {
    background-color: rgb(var(--lgt-accent));
  }
  .header.passed::after {
    background-color: rgb(var(--lgt-secondary));
  }
  .outer {
    --progress: 0%;
    background-color: rgb(var(--lgt-secondary));
  }
  .inner {
    height: var(--progress);
    background-color: rgba(var(--lgt-accent), 1);
  }
</style>
