<script lang="ts">
  import '../app.css';
  import { onMount } from 'svelte';

  let { children } = $props();

  let mouseX = $state(0);
  let mouseY = $state(0);
  let cursorX = $state(0);
  let cursorY = $state(0);
  let isHovering = $state(false);

  onMount(() => {
    const handleMouseMove = (e: MouseEvent) => {
      mouseX = e.clientX;
      mouseY = e.clientY;

      // Smart Global Hover Detection (Event Delegation)
      const target = e.target as HTMLElement;
      isHovering = !!target.closest('a, button, [role="button"], input, textarea');
    };
    window.addEventListener("mousemove", handleMouseMove);

    const animateCursor = () => {
      cursorX += (mouseX - cursorX) * 0.15;
      cursorY += (mouseY - cursorY) * 0.15;
      requestAnimationFrame(animateCursor);
    };
    animateCursor();

    return () => {
      window.removeEventListener("mousemove", handleMouseMove);
    };
  });
</script>

<svelte:head>
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
  <link
    href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;800&family=Plus+Jakarta+Sans:wght@300;400;600;800&display=swap"
    rel="stylesheet"
  />
</svelte:head>

<!-- Branded Global Cursor: SRUJAN -->
<div 
  class="fixed pointer-events-none z-[9999] mix-blend-exclusion text-white hidden md:flex items-center justify-center gap-2"
  style="left: {cursorX}px; top: {cursorY}px; transform: translate(-50%, -50%)"
>
  <div class="w-1 h-1 bg-white rounded-full transition-all duration-500 {isHovering ? 'scale-[10] opacity-20' : 'scale-1 opacity-100'}"></div>
  <span class="text-[9px] font-extrabold tracking-[0.5em] uppercase transition-all duration-300 {isHovering ? 'scale-110 opacity-100' : 'scale-100 opacity-30'}">SRUJAN</span>
</div>

{@render children()}

<style>
  :global(body) {
    cursor: none !important;
  }
  
  :global(a, button, [role="button"], input, textarea) {
    cursor: none !important;
  }
</style>
