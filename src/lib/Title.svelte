<script>
  export let rippleCanvas
  let title
  import { onMount } from "svelte"
  function dropText(span) {
    span.style.transition = "transform 0.5s"
    setTimeout(() => {
      span.style.transform = "translateY(0)"
    }, 0)
  }

  /**
   * @param {number} amount
   */
  async function rotate(amount) {
    title.style.setProperty("--rotation", `${amount}deg`)
    await new Promise(resolve => setTimeout(resolve, 500))
  }

  onMount(async () => {
    if (title) {
      drop()
    }
  })

  async function reset() {
    await rotate(0.5)
    await rotate(-1.5)
    title.children[1].style.transform = ""
    await rotate(3)
    title.children[0].style.transform = ""
  }

  /**
   * @param {HTMLElement} element
   */
  function getCenterOfElement(element) {
    const rect = element.getBoundingClientRect()
    return {
      x: rect.left + rect.width / 2,
      y: rect.top + rect.height / 2,
      bottom: rect.bottom,
    }
  }
  async function drop() {
    await rotate(0)
    dropText(title.children[0])
    window.setTimeout(async () => {
      // first drop the first two children of title
      rotate(-3)
      // get the center of the first child
      await new Promise(resolve => setTimeout(resolve, 75))
      await secondDrop()
      await rotate(0)
      const { bottom, left, right, top } = title.getBoundingClientRect()
      const accent = getComputedStyle(document.documentElement)
        .getPropertyValue("--accent")
        .split(",")
        .map(n => parseInt(n))
      const secondary = getComputedStyle(document.documentElement)
        .getPropertyValue("--secondary")
        .split(",")
        .map(n => parseInt(n))
      rippleCanvas.newRipple(right, top, accent)
      rippleCanvas.newRipple(left, top, secondary)
    }, 475)

    async function secondDrop() {
      dropText(title.children[1])
      window.setTimeout(() => {
        // then drop the last two children of title
        rotate(1.5)
      }, 500)
      return new Promise(resolve => setTimeout(resolve, 1000))
    }
  }
  async function animate() {
    await reset()
    drop()
  }
</script>

<h1 bind:this={title} on:click={animate}>
  <span id="first">
    <span>Happiness</span>
    <span>Comes</span>
  </span>
  <span id="second">
    <span>From</span>
    <span>Balance</span>
  </span>
</h1>

<style>
  h1 {
    font-size: min(5vw, 40px);
    font-weight: bold;
    text-align: center;
    margin: 0;
    line-height: 1.5;
    --rotation: 0deg;
    transform: rotate(var(--rotation));
    transform-origin: 50% 80%;
    transition: transform 0.5s ease-out;
    display: flex;
    justify-content: space-between;
    margin-bottom: 12vh;
    width: 90%;
    max-width: 800px;
    margin-left: auto;
    margin-right: auto;
    cursor: pointer;
  }
  :global(body) {
    padding-top: 20vh;
  }
  h1::before {
    content: "";
    display: block;
    width: 100%;
    height: max(0.4vmax, 3px);
    position: absolute;
    bottom: 20%;
    left: 0;
    z-index: -1;
    background-color: var(--avg-color);
  }
  h1::after {
    content: "";
    display: block;
    width: 0;
    height: 0;
    position: absolute;
    bottom: -0.25em;
    left: 50%;
    margin-left: -0.5em;
    border-left: 0.5em solid transparent;
    border-right: 0.5em solid transparent;
    border-bottom: 0.5em solid var(--avg-color);
    transform: rotate(calc(var(--rotation) * -1));
    transition: transform 0.5s ease-out;
    z-index: -1;
  }
  h1 > span {
    display: inline-block;
    margin: 0 0.1rem;
    transform: translateY(-1000%);
    transition: transform 0.5s ease-in;
  }
  #second > * {
    position: relative;
    color: var(--accent-color);
  }
  #first {
    position: relative;
    font-size: 0.76em;
    top: 0.34em;
  }
  @media only screen and (max-width: 600px) {
    h1 {
      font-size: 8vw;
      margin-bottom: 20vh;
      margin-left: 1.5rem;
    }
    :global(body) {
      padding-top: 20vh;
    }
    h1 > span {
      display: flex;
      flex-wrap: wrap;
    }
    #first {
      font-size: 1em !important;
      top: 0em !important;
      justify-content: left;
    }
    #second {
      font-size: 1em !important;
      top: 0em !important;
      justify-content: right;
    }
    h1:before {
      height: 0.75vw;
      bottom: 10%;
    }
  }
</style>
