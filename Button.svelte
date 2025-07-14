<script>
  import { onMount, onDestroy } from 'svelte';
  import html2canvas from 'html2canvas';
  import ButtonClass from './button.js';

  let { children, text = 'Button', size = '48', type = 'rounded', onClick, ...rest } = $props();

  let host;
  let instance;

  onMount(() => {
    if (typeof window !== 'undefined' && !window.html2canvas) {
      window.html2canvas = html2canvas;
    }
    instance = new ButtonClass({ text, size, type, onClick, ...rest });
    if (host) {
      host.appendChild(instance.element);
      // move any rendered child nodes inside the button element
      const nodes = Array.from(host.childNodes).filter(n => n !== instance.element);
      nodes.forEach(n => instance.element.appendChild(n));
    }
  });

  onDestroy(() => {
    if (instance && instance.element && instance.element.parentNode) {
      instance.element.parentNode.removeChild(instance.element);
    }
  });
</script>

<div bind:this={host}>
  {@render children?.()}
</div>

<style>
  .glass-container {
    position: relative;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 20px;
    padding: 10px;
    box-sizing: border-box;
  }

  .glass-container-circle {
    aspect-ratio: 1 / 1;
    flex-shrink: 0;
    flex-grow: 0;
  }

  .glass-container-pill {
    flex-shrink: 0;
    flex-grow: 0;
  }

  .glass-button {
    position: relative;
    box-shadow: 0 25px 50px rgba(0, 0, 0, 0.25);
    cursor: pointer;
    pointer-events: auto;
    flex-shrink: 0;
    display: flex;
    align-items: center;
    justify-content: center;
    box-sizing: border-box;
  }

  .glass-button-circle {
    aspect-ratio: 1 / 1;
    flex-shrink: 0;
    flex-grow: 0;
  }

  .glass-button-text {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    z-index: 1;
    pointer-events: none;
    font-weight: normal;
    color: white;
    font-family: system-ui, -apple-system, sans-serif;
    white-space: nowrap;
  }
</style>
