<script>
  import { onMount, onDestroy } from 'svelte';
  import ContainerClass from './container.js';

  // access all props including children using Svelte runes style
  let { children, ...options } = $props();

  let host;
  let instance;

  onMount(() => {
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
