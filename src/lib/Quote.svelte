<script>
  export let link
  export let source
</script>

<div class="glass quote">
  <q>
    <slot />
  </q>
  <a href={link} target="_blank" rel="noreferrer noopener">{source}</a>
</div>

<style>
  .quote.glass {
    padding-top: 0;
    padding: 0.5em;
    background: var(--glass-color);
  }

  q {
    position: relative;
    margin-left: 0.5rem;
    margin-right: 0.5rem;
  }

  q::before {
    content: "“";
    font-size: 1.5rem;
    font-weight: bold;
    color: var(--accent-color);
    margin-right: 0.1rem;
    position: absolute;
    left: -0.75rem;
    width: fit-content;
    top: -0.25rem;
  }
  q::after {
    content: "”";
    font-size: 1.5rem;
    font-weight: bold;
    color: var(--accent-color);
    position: absolute;
    right: -0.75rem;
    bottom: -0.25rem;
    margin-left: 0.1rem;
  }

  a {
    color: inherit;
    text-align: right;
    display: block;
    cursor: pointer;
    font-style: italic;
  }
</style>
