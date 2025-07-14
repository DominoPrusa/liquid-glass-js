<script>
  import { onMount, onDestroy } from 'svelte';
  import html2canvas from 'html2canvas';
  import ContainerClass from './container.js';

  // access all props including children using Svelte runes style
  let { children, ...options } = $props();

  let host;
  let instance;

  onMount(() => {
    if (typeof window !== 'undefined' && !window.html2canvas) {
      window.html2canvas = html2canvas;
    }
    instance = new ContainerClass(options);
    if (host) {
      host.appendChild(instance.element);

      // move any rendered child nodes inside the container element
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
</style>
