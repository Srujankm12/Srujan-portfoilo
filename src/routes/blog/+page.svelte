<script lang="ts">
  import { blogPosts } from "$lib/blog";
  import { onMount } from "svelte";

  let isHovering = $state(false);
  let mouseX = $state(0);
  let mouseY = $state(0);
  let cursorX = $state(0);
  let cursorY = $state(0);

  onMount(() => {
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

    const observer = new IntersectionObserver((entries) => {
      entries.forEach(entry => {
        if (entry.isIntersecting) {
          entry.target.classList.add("reveal-visible");
        }
      });
    }, { threshold: 0.15 });

    document.querySelectorAll(".reveal").forEach(el => observer.observe(el));

    return () => {
      window.removeEventListener("mousemove", handleMouseMove);
    };
  });
</script>

<svelte:head>
  <title>Insights & Engineering | Software Architecture Blog</title>
  <meta name="description" content="Deep dives into backend engineering, Next.js optimization, and scaling digital products in 2026." />
</svelte:head>

<!-- Cursor -->
<div 
  class="fixed pointer-events-none z-[9999] mix-blend-exclusion text-white hidden md:block"
  style="left: {cursorX}px; top: {cursorY}px; transform: translate(-50%, -50%)"
>
  <div class="w-2 h-2 bg-white rounded-full {isHovering ? 'scale-[4]' : 'scale-1'} transition-transform duration-300"></div>
</div>

<div class="bg-[#050505] text-white min-h-screen selection:bg-white/20 pt-32 pb-24">
  <!-- Navbar (Subset for Blog) -->
  <nav class="fixed top-6 left-1/2 -translate-x-1/2 z-50 w-[min(90vw,600px)] backdrop-blur-2xl border border-white/10 rounded-full py-4 px-8 flex justify-center items-center">
    <div class="flex gap-8 text-xs font-medium uppercase tracking-[0.2em] opacity-60">
      <a href="/" class="hover:text-white hover:opacity-100 transition-all cursor-none" on:mouseenter={() => (isHovering = true)} on:mouseleave={() => (isHovering = false)}>Portfolio</a>
      <a href="/blog" class="text-white opacity-100 transition-all cursor-none">Articles</a>
    </div>
  </nav>

  <div class="container max-w-7xl mx-auto px-6">
    <div class="mb-24 reveal">
      <h1 class="text-6xl md:text-9xl font-[800] tracking-tighter mb-8 leading-none">Engineering<br />Insights</h1>
      <p class="text-white/40 text-lg md:text-xl font-medium max-w-2xl">Sharing specialized knowledge on Golang backends, React & Next.js systems, SvelteKit performance, and the future of scalable digital products.</p>
    </div>

    <div class="grid grid-cols-1 md:grid-cols-2 gap-12">
      {#each blogPosts as post, i}
        <article class="group reveal stagger-{i + 1}">
          <a href="/blog/{post.slug}" class="block relative rounded-3xl overflow-hidden aspect-video bg-[#111] border border-white/5 cursor-none transition-all" on:mouseenter={() => (isHovering = true)} on:mouseleave={() => (isHovering = false)}>
            <div class="absolute inset-0 bg-gradient-to-br from-blue-500/10 via-transparent to-purple-500/10 opacity-0 group-hover:opacity-100 transition-opacity duration-700"></div>
            <div class="absolute inset-0 flex items-center justify-center p-12 text-center">
               <span class="text-xs font-bold uppercase tracking-[0.4em] opacity-20 group-hover:opacity-100 transition-opacity">{post.tags[0]}</span>
            </div>
            
            <div class="absolute inset-0 p-8 flex flex-col justify-end z-20">
              <div class="translate-y-4 group-hover:translate-y-0 transition-transform duration-500">
                <span class="text-[10px] font-bold uppercase tracking-[0.3em] text-white/50 mb-3 block">{post.date} • {post.readingTime}</span>
                <h2 class="text-3xl font-bold mb-4">{post.title}</h2>
                <div class="flex flex-wrap gap-2 opacity-0 group-hover:opacity-100 transition-all duration-700 delay-100">
                  {#each post.tags as tag}
                    <span class="px-3 py-1 bg-white/10 backdrop-blur-md rounded-full text-[10px] border border-white/5">{tag}</span>
                  {/each}
                </div>
              </div>
            </div>
          </a>
        </article>
      {/each}
    </div>
  </div>
</div>

<style>
  :global(html) {
    font-family: 'Plus Jakarta Sans', sans-serif;
    scroll-behavior: smooth;
    cursor: none;
  }

  h1, h2 {
    font-family: 'Outfit', sans-serif;
  }

  .reveal {
    opacity: 0;
    transform: translateY(30px);
    transition: opacity 1s cubic-bezier(0.16, 1, 0.3, 1), transform 1s cubic-bezier(0.16, 1, 0.3, 1);
  }

  .reveal-visible {
    opacity: 1 !important;
    transform: translateY(0) !important;
  }

  .stagger-1 { transition-delay: 0.1s; }
  .stagger-2 { transition-delay: 0.2s; }
  .stagger-3 { transition-delay: 0.3s; }
  .stagger-4 { transition-delay: 0.4s; }
</style>
