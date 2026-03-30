<script lang="ts">
  import { page } from "$app/state";
  import { blogPosts } from "$lib/blog";
  import { onMount, tick } from "svelte";

  let post = $derived(blogPosts.find(p => p.slug === page.params.slug));

  let isHovering = $state(false);
  let mouseX = $state(0);
  let mouseY = $state(0);
  let cursorX = $state(0);
  let cursorY = $state(0);
  let scrollProgress = $state(0);

  // Svelte Action for robust reveals
  function reveal(node: HTMLElement) {
    const observer = new IntersectionObserver((entries) => {
      if (entries[0].isIntersecting) {
        node.classList.add("reveal-visible");
        observer.disconnect();
      }
    }, { threshold: 0.05 });
    
    observer.observe(node);
    return {
      destroy() { observer.disconnect(); }
    };
  }

  $effect(() => {
    if (!post) return;

    const handleMouseMove = (e: MouseEvent) => {
      mouseX = e.clientX;
      mouseY = e.clientY;
    };
    window.addEventListener("mousemove", handleMouseMove);

    const animateCursor = () => {
      cursorX += (mouseX - cursorX) * 0.15;
      cursorY += (mouseY - cursorY) * 0.15;
      requestAnimationFrame(animateCursor);
    };
    animateCursor();

    const handleScroll = () => {
      const winHeight = window.innerHeight;
      const docHeight = document.documentElement.scrollHeight;
      const scrollTop = window.scrollY;
      scrollProgress = (scrollTop / (docHeight - winHeight)) * 100;
    };
    window.addEventListener("scroll", handleScroll);

    return () => {
      window.removeEventListener("mousemove", handleMouseMove);
      window.removeEventListener("scroll", handleScroll);
    };
  });
</script>

<svelte:head>
  {#if post}
    <title>{post.seoTitle}</title>
    <meta name="description" content={post.seoDescription} />
  {/if}
</svelte:head>

{#if post}
  <!-- Cursor -->
  <div 
    class="fixed pointer-events-none z-[9999] mix-blend-exclusion text-white hidden md:flex items-center justify-center gap-2"
    style="left: {cursorX}px; top: {cursorY}px; transform: translate(-50%, -50%)"
  >
    <div class="w-1 h-1 bg-white rounded-full transition-transform duration-500 {isHovering ? 'scale-[8]' : 'scale-1'}"></div>
    <span class="text-[8px] font-bold tracking-[0.4em] uppercase transition-opacity duration-300 {isHovering ? 'opacity-100' : 'opacity-40'}">SKM</span>
  </div>

  <!-- Progress Bar -->
  <div class="fixed top-0 left-0 h-[2px] bg-white z-[100]" style="width: {scrollProgress}%"></div>

  <div class="bg-[#050505] text-white min-h-screen selection:bg-white/20 pt-40 pb-24 font-[400] leading-relaxed overflow-x-hidden">
    <!-- Navbar (Subset for Blog) -->
    <nav class="fixed top-6 left-1/2 -translate-x-1/2 z-50 w-[min(90vw,300px)] backdrop-blur-2xl border border-white/10 rounded-full py-4 px-8 flex justify-center items-center">
      <div class="flex gap-8 text-xs font-medium uppercase tracking-[0.2em] opacity-60">
        <a href="/blog" class="hover:text-white hover:opacity-100 transition-all cursor-none" on:mouseenter={() => (isHovering = true)} on:mouseleave={() => (isHovering = false)}>← Back</a>
      </div>
    </nav>

    <div class="container max-w-4xl mx-auto px-6">
      <header class="mb-24 reveal-visible" use:reveal>
        <div class="flex gap-4 mb-8">
          {#each post.tags as tag}
            <span class="px-4 py-1.5 bg-white/5 backdrop-blur-md rounded-full text-[10px] border border-white/10 uppercase tracking-widest font-bold">{tag}</span>
          {/each}
        </div>
        <h1 class="text-4xl md:text-7xl font-[800] tracking-tighter mb-10 leading-none">{post.title}</h1>
        <div class="flex items-center gap-6 opacity-40 text-xs font-bold uppercase tracking-[0.3em]">
          <span>{post.date}</span>
          <span>•</span>
          <span>{post.readingTime}</span>
        </div>
      </header>

      <article class="prose prose-invert prose-lg max-w-none reveal" use:reveal>
        <div class="article-content">
          {@html post.content
            .replace(/\r\n/g, '\n')
            .replace(/^\s*###\s+(.*$)/gm, '<h3 class="text-2xl font-bold mt-12 mb-6 text-white leading-tight">$1</h3>')
            .replace(/^\s*##\s+(.*$)/gm, '<h2 class="text-3xl font-bold mt-16 mb-8 text-white leading-tight">$1</h2>')
            .replace(/\*\*(.*?)\*\*/g, '<strong class="text-white font-bold">$1</strong>')
            .replace(/\[(.*?)\]\((.*?)\)/g, '<a href="$2" class="text-white underline decoration-white/20 underline-offset-4 hover:decoration-white transition-all">$1</a>')
            .replace(/^\s*\* (.*$)/gm, '<li class="mb-4 text-white/70 ml-6 list-disc">$1</li>')
            .replace(/\n\n/g, '<br/><br/>')
            .replace(/^\s*---\s*$/gm, '<hr class="border-white/5 my-16" />')
            .replace(/🚀/g, '')}
        </div>
      </article>

      <footer class="mt-32 pt-16 border-t border-white/10 reveal" use:reveal>
        <div class="p-12 rounded-[40px] bg-white/[0.03] border border-white/10 text-center">
            <h3 class="text-2xl font-bold mb-6">Build the Future of Your Product.</h3>
            <p class="text-white/40 mb-10 max-w-lg mx-auto">Ready to scale your digital presence with high-performance engineering?</p>
            <a 
              href="mailto:developersrujan12@gmail.com" 
              class="inline-block px-12 py-6 bg-white text-black font-bold uppercase text-xs tracking-widest rounded-2xl hover:scale-105 transition-all text-center cursor-none"
              on:mouseenter={() => (isHovering = true)} 
              on:mouseleave={() => (isHovering = false)}
            >
                Start Your Project Build
            </a>
        </div>
      </footer>
    </div>
  </div>
{:else}
  <div class="bg-[#050505] min-h-screen flex items-center justify-center">
    <p class="text-white opacity-40 uppercase tracking-widest text-xs">Post not found</p>
  </div>
{/if}

<style>
  :global(html) {
    font-family: 'Plus Jakarta Sans', sans-serif;
    scroll-behavior: smooth;
    cursor: none;
  }

  h1, :global(h2), :global(h3) {
    font-family: 'Outfit', sans-serif;
  }

  .reveal {
    opacity: 0;
    transform: translateY(30px);
    transition: opacity 1s cubic-bezier(0.16, 1, 0.3, 1), transform 1s cubic-bezier(0.16, 1, 0.3, 1);
  }

  :global(.reveal-visible) {
    opacity: 1 !important;
    transform: translateY(0) !important;
  }

  .article-content :global(h2) {
    font-size: 2.25rem;
    line-height: 1.2;
    letter-spacing: -0.04em;
    color: white;
  }

  .article-content :global(p) {
    color: rgba(255, 255, 255, 0.7);
    line-height: 1.8;
    margin-bottom: 2rem;
  }

  .article-content :global(li) {
    color: rgba(255, 255, 255, 0.7);
    list-style: disc;
    margin-left: 1.5rem;
  }

  .article-content :global(strong) {
    color: white;
    font-weight: 700;
  }
</style>yle>
