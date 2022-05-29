<script>
  let canvas, ctx
  import { onMount } from "svelte"
  let ripples = []
  let lastScroll = window.scrollY
  class Ripple {
    /**
     * @param {number} x
     * @param {number} y
     */
    constructor(x, y, rgb) {
      this.x = x
      this.y = y
      this.radius = 0
      this.opacity = 0.5
      this.speed = 4000
      this.rgb = rgb
    }
    /**
     * @param {CanvasRenderingContext2D} ctx
     */
    draw(ctx) {
      ctx.beginPath()
      ctx.arc(this.x, this.y, this.radius, 0, Math.PI * 2)
      const gradient = ctx.createRadialGradient(
        this.x,
        this.y,
        0,
        this.x,
        this.y,
        this.radius
      )
      // get css variable --avg-color
      const primaryColor = getComputedStyle(document.documentElement)
        .getPropertyValue("--primary")
        .split(",")
        .map(n => parseInt(n))
      gradient.addColorStop(
        0,
        `rgba(${this.rgb.join(",")}, ${this.opacity * 0.5})`
      )
      gradient.addColorStop(1, `rgba(${primaryColor.join(",")}, 0)`)
      ctx.fillStyle = gradient

      ctx.fill()
    }
    /**
     * @param {CanvasRenderingContext2D} ctx
     */
    update(ctx, delta) {
      this.radius += this.speed * delta
      this.opacity -= 2 * delta
      this.draw(ctx)
    }
  }
  /**
   * @param {number} x
   * @param {number} y
   * @param {[number, number, number]} rgb
   */
  export function newRipple(x, y, rgb) {
    const ripple = new Ripple(x, y, rgb)
    const rippleCt = ripples.length
    ripples.push(ripple)
    if (rippleCt === 0) {
      window.requestAnimationFrame(updateRipples)
    }
    return ripple
  }

  // update and draw all ripples
  let lastTime = 0
  function updateRipples(time) {
    const delta = lastTime === 0 ? 0.0016 : (time - lastTime) / 10000
    lastTime = time
    ctx.clearRect(0, 0, canvas.width, canvas.height)
    const diff = window.scrollY - lastScroll
    lastScroll = window.scrollY
    ripples.forEach(ripple => {
      ripple.y -= diff
      ripple.draw(ctx)
      ripple.update(ctx, delta)
    })
    ripples = ripples.filter(ripple => ripple.opacity >= 0)
    if (ripples.length > 0) {
      window.requestAnimationFrame(updateRipples)
    } else {
      ctx.clearRect(0, 0, canvas.width, canvas.height)
      lastTime = 0
    }
  }

  // on mount set the width and height of the canvas to the width and height of the window
  onMount(() => {
    canvas.width = window.innerWidth
    canvas.height = Math.max(window.innerHeight, screen.height)
    ctx = canvas.getContext("2d")
    // set blend mode to multiply
    ctx.globalCompositeOperation = "multiply"
  })
  // listen for resize events and set the width and height of the canvas to the width and height of the window
  window.addEventListener("resize", () => {
    canvas.width = window.innerWidth
    canvas.height = Math.max(screen.height, window.innerHeight)
  })
</script>

<canvas bind:this={canvas} />

<style>
  canvas {
    position: fixed;
    top: 0;
    left: 0;
    z-index: 100;
    /* make touch events pass through the canvas */
    touch-action: none;
    pointer-events: none;
  }
</style>
