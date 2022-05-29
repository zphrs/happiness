<script>
  import { onMount } from "svelte"
  import Quote from "./Quote.svelte"

  import Subtitle from "./Subtitle.svelte"

  export let title = "LOREM IPSUM"
  export let source = null
  export let link = null
  export let rc
  export let delay
  export let quote = null
  export let visible = true
  let div, button
  $: {
    if (div) {
      div.style.setProperty("--delay", delay + "s")
      div.style.setProperty("--p-delay", delay + 1 + "s")
    }
  }
  function toggleVisible() {
    delay = 0
    const color = visible
      ? getComputedStyle(div)
          .getPropertyValue("--accent")
          .split(",")
          .map(n => parseInt(n))
      : getComputedStyle(div)
          .getPropertyValue("--secondary")
          .split(",")
          .map(n => parseInt(n))
    if (!visible) {
      const { left, width, top, height } = button.getBoundingClientRect()
      rc.newRipple(left + width / 2, top + height / 2, color)
    } else {
      window.setTimeout(() => {
        const { left, width, top, height } = button.getBoundingClientRect()
        rc.newRipple(left + width / 2, top + height / 2, color)
      }, 0)
    }
    visible = !visible
  }
  /**
   *
   * @param {string} selector
   */
  export function querySelector(selector) {
    return div.querySelector(selector)
  }
  export function getBoundingClientRect() {
    return div.getBoundingClientRect()
  }

  export function scrollIntoView(dict) {
    div.scrollIntoView(dict)
  }
</script>

<div bind:this={div} class="glass">
  <Subtitle {rc} {delay}>{title}</Subtitle>
  <div class="paragraph">
    <button
      bind:this={button}
      on:click={() => toggleVisible()}
      class:pink={visible}
    >
      {#if !visible}
        <span>Tap me to learn more...</span>
      {:else}
        <span>Tap me to show less</span>
      {/if}
    </button>
    <br />
    {#if visible}
      <slot />
      <br />
      {#if quote}
        <Quote {link} {source}>
          {quote}
        </Quote>
      {/if}
    {/if}
  </div>
</div>

<style>
  div {
    --delay: 0s;
  }
  button {
    cursor: pointer;
    font: inherit;
    background-color: inherit;
    border: 2px solid var(--accent-color);
    border-radius: 0.5rem;
    color: inherit;
    transition: background-color 1s;
    width: 100%;
  }
  button:hover {
    background-color: var(--glass-color);
  }

  button.pink:hover {
    background-color: rgba(var(--accent), 0.2);
  }

  .glass {
    max-width: 800px;
    width: calc(100% - 2rem);
  }

  @media screen and (max-width: 600px) {
    .glass {
      max-width: 600px;
      width: 100%;
      border-radius: 0;
      background: rgb(var(--avg), 0.1);
      box-sizing: border-box;
      margin-bottom: 3rem;
      padding-left: 1.75rem;
    }
    :global(body) {
      margin: 0;
    }
  }

  .paragraph {
    animation: fadein 1s var(--p-delay) ease both;
  }

  @keyframes stretch {
    from {
      transform: scaleX(0);
    }
    to {
      transform: scaleX(1);
    }
  }

  @keyframes fadeout {
    from {
      backdrop-filter: blur(8px);
      background-color: rgb(var(--primary), 1);
    }
    to {
      backdrop-filter: blur(0px);
      background-color: rgb(var(--primary), 0);
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
