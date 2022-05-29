<script>
  import { onMount } from "svelte"
  /** @type {HTMLElement} */
  export let div = null
  export let rc = null
  let li
  let progress
  // get the parent of the header
  const parent = div
  function setProgress() {
    // get height and top of parent
    const { height, top } = parent.getBoundingClientRect()
    progress = (-1 * top + window.innerHeight) / height
    li.style.setProperty(
      "--progress",
      Math.max(0, Math.min(1, progress)) * 100 + "%"
    )
  }
  // on mount set the progress
  onMount(() => {
    setProgress()
  })
  // on resize set the progress
  window.addEventListener("resize", setProgress)
  // on scroll set the progress
  window.addEventListener("scroll", setProgress)
  // on click set the progress
  document.addEventListener("click", setProgress)
  async function scrollIntoView(event) {
    event.preventDefault()
    div.scrollIntoView({ behavior: "smooth", block: "start" })
    await new Promise(resolve => setTimeout(resolve, 0))
    // get the center of the element
    const { top, left, width, height } = div.getBoundingClientRect()
    const center = {
      x: left + width / 2,
      y: top + height / 2,
    }
    // get the --accent css variable
    const accent = getComputedStyle(document.documentElement)
      .getPropertyValue("--accent")
      .split(",")
      .map(n => parseInt(n))
    if (rc) rc.newRipple(center.x, center.y, accent)
    event.target && event.target.blur()
  }
</script>

<li bind:this={li}>
  <button
    on:click={scrollIntoView}
    class:active={progress > 0 && progress < 1}
    class:passed={progress > 0}
  >
    <slot />
  </button>
</li>

<style>
  li {
    list-style: none;
    position: relative;
    margin: 0;
    padding: 0;
  }
  button {
    --progress: 50;
    display: block;
    width: fit-content;
    text-align: left;
    padding: 0.25rem;
    padding-left: 1rem;
    background-color: transparent;
    border: none;
    outline: none;
    cursor: pointer;
    width: 100%;
    color: inherit;
    font: inherit;
  }
  button.passed {
    background: rgba(var(--secondary), 0.1);
  }
  button.active {
    background-color: rgba(var(--accent), 0.2);
  }
  button:hover {
    background-color: rgba(var(--accent), 0.1);
  }
  li::before {
    /* make a vertical line */
    content: "";
    display: block;
    width: 0.25rem;
    height: 100%;
    position: absolute;
    left: 0;
    top: 0;
    background-color: rgb(var(--secondary));
  }
  li::after {
    /* make a vertical line with height of progress */
    content: "";
    display: block;
    width: 0.25rem;
    height: var(--progress);
    position: absolute;
    left: 0;
    top: 0;
    background-color: var(--accent-color);
  }
</style>
