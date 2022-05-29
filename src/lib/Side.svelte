<script>
  import { onMount } from "svelte"

  let collapsed = false
  let div, slot, width
  let mobile = window.innerWidth < 1120
  // on resize set mobile
  window.addEventListener("resize", () => {
    mobile = window.innerWidth < 1120
  })
  function collapse(e) {
    collapsed = !collapsed
    const width = div.getBoundingClientRect().width
    div.style.setProperty("--width", `${width}px`)
  }
  if (mobile) {
    onMount(() => {
      width = div.getBoundingClientRect().width
      const def = div.style.getPropertyValue("--transition-speed")
      div.style.setProperty("--transition-speed", "0s")
      window.setTimeout(async () => {
        collapse()
        await new Promise(resolve => setTimeout(resolve, 10))
        div.style.setProperty("--transition-speed", def)
      }, 0)
    })
  }
  onMount(() => {
    slot.addEventListener("click", () => {
      console.log("HERE was a click")
      if (mobile && !collapsed) collapse()
      else if (collapsed) collapse()
    })
    // listen for resize
    let timeout = null
    window.addEventListener("resize", () => {
      if (timeout) clearTimeout(timeout)
      timeout = setTimeout(() => {
        if (mobile && !collapsed) collapse()
        else if (!mobile && collapsed) collapse()
        width = div.getBoundingClientRect().width
        div.style.setProperty("--width", `${width}px`)
      }, 0)
    })
  })
  // listen for touch down event
  let down = false
  let start = { x: 0, y: 0 }
  document.addEventListener("touchstart", e => {
    down = true
    start = { x: e.touches[0].clientX, y: e.touches[0].clientY }
    console.log("touchstart", start)
  })
  // listen for touch move event
  document.addEventListener("touchmove", e => {
    if (down) {
      const x = e.touches[0].clientX
      const y = e.touches[0].clientY
      const dx = x - start.x
      const dy = y - start.y
      console.log("touchmove", { x, y, dx, dy })
      if (Math.abs(dx) * 0.1 > Math.abs(dy) && Math.abs(dx) > 100) {
        if (dx < 0) {
          if (!collapsed) collapse()
        } else {
          if (collapsed) collapse()
        }
      }
    }
  })
  // listen for touch up event
  document.addEventListener("touchend", e => {
    down = false
    console.log("touchend")
  })
</script>

<aside>
  <div
    class="disable"
    class:collapsed={mobile ? collapsed : true}
    on:click={collapse}
  />
  <div class="side" bind:this={div} class:collapsed class:mobile>
    <button on:click={collapse}>
      <i
        ><svg
          class="app-switcher-button-icon"
          version="1.1"
          id="Layer_1"
          x="0"
          y="0"
          width="100%"
          height="100%"
          viewBox="0 0 24 24"
          enable-background="new 0 0 24 24"
          xml:space="preserve"
          ><path
            d="M8.59,16.59L13.17,12L8.59,7.41L10,6l6,6l-6,6L8.59,16.59z"
          /><path fill="none" d="M0,0h24v24H0V0z" /></svg
        ></i
      ></button
    >
    <div bind:this={slot}>
      <slot name="main" />
    </div>
  </div>
  <slot name="under" />
</aside>

<style>
  .disable {
    position: fixed;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
    background: rgba(var(--avg), 0.3);
    transition: background-color 0.2s, backdrop-filter 1s,
      -webkit-backdrop-filter 1s;
    transform-origin: right;
    backdrop-filter: blur(2px);
    -webkit-backdrop-filter: blur(2px);
    z-index: 5;
  }
  .disable.collapsed {
    background: rgba(var(--primary), 0);
    backdrop-filter: blur(0);
    -webkit-backdrop-filter: blur(0);
    pointer-events: none;
  }
  .side {
    position: fixed;
    top: 0;
    left: 0;
    z-index: 10;
    background-color: rgba(var(--lighter-secondary));
    height: 100%;
    z-index: 10;
    --width: 0px;
    --transition-speed: 0.5s;
    text-align: right;
    transition: left var(--transition-speed) ease-out;
    padding-top: 3rem;
  }
  .side.collapsed {
    left: calc(var(--width) * -1);
    transition: left var(--transition-speed) var(--transition-speed) ease-out;
  }
  button {
    position: absolute;
    top: 0.5rem;
    right: 0%;
    width: calc(100% - 0.5rem);
    height: 2rem;
    border-radius: 1rem;
    outline: none;
    z-index: 50;
    font: inherit;
    cursor: pointer;
    text-align: left;
    border: none;
    color: inherit;
    background: rgb(var(--lgt-secondary));
    border-top-right-radius: 0;
    border-bottom-right-radius: 0;
    transition: right var(--transition-speed) var(--transition-speed),
      width var(--transition-speed) var(--transition-speed),
      border-radius var(--transition-speed) var(--transition-speed),
      background-color var(--transition-speed),
      padding-left var(--transition-speed) var(--transition-speed);
    box-sizing: border-box;
  }
  @media screen and (max-width: 600px) {
    button {
      top: unset;
      bottom: 5rem;
    }
    .side {
      padding-left: 0.75rem;
      box-sizing: border-box;
      padding-top: 0.5rem;
    }
  }
  button:hover {
    width: calc(100% + 1.5rem);
    right: -2rem;
    border-radius: 1rem;
    padding-left: calc(var(--width) * 0.125);
    background-color: rgb(var(--lgt-secondary));
    transition: width calc(var(--transition-speed) * 2),
      right calc(var(--transition-speed) * 2),
      padding-left calc(var(--transition-speed) * 2),
      border-radius calc(var(--transition-speed) * 2),
      background-color calc(var(--transition-speed) * 2);
  }
  button:hover i {
    transform: rotate(135deg);
    transition: transform calc(var(--transition-speed) * 2);
  }
  .side.collapsed button:hover i {
    transform: rotate(45deg);
    transition: transform calc(var(--transition-speed) * 2);
  }
  .side.collapsed button {
    right: -3rem;
    left: auto;
    width: calc(var(--width) + 2rem);
    border-radius: 2rem;
    border-bottom-left-radius: 0;
    border-top-left-radius: 0;
    background: rgb(var(--lgt-accent));
    padding-left: calc(var(--width) + 0.5rem);
    transition: right var(--transition-speed), width var(--transition-speed),
      border-radius var(--transition-speed),
      background-color var(--transition-speed),
      padding-left var(--transition-speed);
  }
  .side.collapsed.mobile button {
    right: -1.5rem;
  }
  .side.collapsed.mobile button:hover {
    right: -2.5rem;
  }
  .collapsed button:hover {
    padding-left: calc(var(--width) * 0.875);
    background-color: rgb(var(--lgt-secondary));
    transition-duration: calc(var(--transition-speed) * 2);
  }
  .side.collapsed i {
    transform: rotate(0deg);
  }

  .side.collapsed:hover {
    transition-duration: calc(var(--transition-speed) * 2);
    left: calc(var(--width) * -0.85);
    transition-delay: 0s;
  }
  .side.collapsed.mobile {
    left: calc(var(--width) * -1);
  }
  .collapsed.mobile button:hover {
    padding-left: calc(var(--width) + 0.5rem);
    background-color: rgb(var(--lgt-accent));
    transition-duration: calc(var(--transition-speed) * 2);
  }
  .side i {
    font-style: normal;
    transform: rotate(180deg);
    transition: transform var(--transition-speed) var(--transition-speed);
    display: inline-block;
    width: fit-content;
    width: 1.25em;
    height: 100%;
  }
  svg {
    fill: var(--secondary-color);
  }
</style>
