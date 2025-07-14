<script>
  import { onMount, onDestroy } from 'svelte';
  import ButtonClass from './button.js';

  let { children, text = 'Button', size = '48', type = 'rounded', onClick, ...rest } = $props();

  let host;
  let instance;

  onMount(() => {
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
