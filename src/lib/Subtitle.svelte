<script>
  export let delay = 0
  export let time = 1
  export let rc = null
  let header
  import { onMount } from "svelte"
  // set css variable --delay of header
  onMount(() => {
    header.style.setProperty("--delay", `${delay}s`)
    header.style.setProperty("--time", `${time}s`)
  })
  window.setTimeout(() => {
    if (rc && header) {
      // get bounding client rect of header
      const rect = header.getBoundingClientRect()
      // get left 20% of header
      const left = rect.left + rect.width * 0.05
      // get bottom of header
      const bottom = rect.bottom - 2
      // get color of h2
      const color = getComputedStyle(header).color
      // change color into array
      const colorArr = color.match(/\d+/g).map(n => parseInt(n))
      rc.newRipple(left, bottom, colorArr)
    }
  }, delay * 1000)
</script>

<h2 bind:this={header}><slot /></h2>

<style>
  h2 {
    color: var(--accent-color);
    margin: 0;
    margin-bottom: 0.5rem;
    --time: 1s;
    --delay: 0s;
    display: block;
    animation: fadein var(--time) var(--delay) linear both;
    position: sticky;
    top: 0;
    background-color: rgb(var(--lighter-secondary));
    padding: 0.5rem;
    box-sizing: border-box;
    border-radius: 0.5rem;
    padding-bottom: 0;
    z-index: 1;
  }
  h2::after {
    position: relative;
    content: "";
    display: block;
    width: 100%;
    box-sizing: border-box;
    height: 3px;
    top: 0;
    background-color: var(--accent-color);
    transform: rotate(calc(var(--rotation) * -1));
    transition: transform 0.5s ease-out;
    transform-origin: 1rem 0;
    animation: stretch var(--time) var(--delay) ease-out both;
  }

  @keyframes stretch {
    from {
      transform: scaleX(0);
    }
    to {
      transform: scaleX(1);
    }
  }
  @keyframes fadein {
    from {
      opacity: 0;
    }
    to {
      opacity: 1;
    }
  }
</style>
