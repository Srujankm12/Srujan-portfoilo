<script lang="ts">
  import { onMount } from "svelte";

  // State
  let currentQuoteIndex = $state(0);
  let isVideoLoaded = $state(false);
  let mouseX = $state(0);
  let mouseY = $state(0);
  let cursorX = $state(0);
  let cursorY = $state(0);
  let isHovering = $state(false);
  let scrollProgress = $state(0);

  // Hero Typing
  const heroWords = [
    "Freelancer Web Developer",
    "SEO Strategist",
    "Application Developer",
  ];
  let currentWordIndex = $state(0);
  let displayText = $state("");
  let isDeleting = $state(false);

  // Projects - Enhanced with images
  const projects = [
    {
      title: "GFS Real Estate Website",
      category: "Real Estate & CRM",
      year: "2025",
      image: "/projects/gfs.png",
      description:
        "A global property solution spanning UAE, UK, and beyond. Built with a robust property listing engine and automated inquiry management.",
      stack: ["Reactjs", "PostgreSQL", "AWS", "CRM"],
      link: "https://gfs-web-hrby4.ondigitalocean.app/",
      accent: "from-amber-500/20 to-transparent",
    },
    {
      title: "PayBazaar Fintech Portal",
      category: "Fintech & Banking",
      year: "2025",
      image: "/projects/paybazaar.png",
      description:
        "Enterprise fintech portal with DMT, AEPS, and automated KYC. Secure banking API integrations for large-scale transactions.",
      stack: ["React js", "Golang", "AWS", "Security"],
      link: "https://paybazaar.in",
      accent: "from-blue-500/20 to-transparent",
    },
    {
      title: "South Canara Agro Mart",
      category: "E-Commerce & Logistics",
      year: "2026",
      image: "/projects/southcanaragromart.png",
      description:
        "Professional B2B e-commerce platform for cashew trading. Features real-time inventory tracking and optimized trade logistics.",
      stack: ["React", "Node.js", "MongoDB", "Logistics"],
      link: "https://southcanaraagromart.com",
      appLink:
        "http://files.southcanaraagromart.com/application/application-36369344-3bdd-4f44-a97d-f7bd8f806ff4.apk",
      accent: "from-emerald-500/20 to-transparent",
    },
    {
      title: "Chartered Accountant Dashboard",
      category: "Enterprise ERP",
      year: "2025",
      image: "/projects/ca_dashboard_mockup_indian_v2.png",
      description:
        "Premium management suite for Indian CA firms. Features GST compliance tracking and automated audit workflows.",
      stack: ["SvelteKit", "Golang", "WebSocket", "ERP"],
      link: "#",
      accent: "from-purple-500/20 to-transparent",
    },
  ];

  const techStack = [
    { name: "Golang", logo: "https://cdn.simpleicons.org/go/white" },
    { name: "Node.js", logo: "https://cdn.simpleicons.org/nodedotjs/white" },
    {
      name: "TypeScript",
      logo: "https://cdn.simpleicons.org/typescript/white",
    },
    { name: "Next.js", logo: "https://cdn.simpleicons.org/nextdotjs/white" },
    { name: "Svelte", logo: "https://cdn.simpleicons.org/svelte/white" },
    { name: "React Native", logo: "https://cdn.simpleicons.org/react/white" },
    { name: "Docker", logo: "https://cdn.simpleicons.org/docker/white" },
    { name: "Git", logo: "https://cdn.simpleicons.org/git/white" },
    { name: "GitHub", logo: "https://cdn.simpleicons.org/github/white" },
    { name: "Vercel", logo: "https://cdn.simpleicons.org/vercel/white" },
    { name: "Redis", logo: "https://cdn.simpleicons.org/redis/white" },
    {
      name: "PostgreSQL",
      logo: "https://cdn.simpleicons.org/postgresql/white",
    },
  ];

  const reviews = [
    {
      name: "Deekshith",
      role: "Founder, South Canara Agro Mart",
      content:
        "The app and website are absolute game-changers. Srujan is a complete workaholic who crushed every deadline we set. His dedication to the project was exceptional—highly recommended for high-pressure timelines!",
      stars: 5,
    },
    {
      name: "Nithin Kumar",
      role: "Team Lead, GFS Developments",
      content:
        "Srujan's collaborative spirit is world-class. He worked day and night to ensure the GFS platform was perfect. Thank you buddy, your effort and technical expertise are truly commendable. Great work!",
      stars: 4.5,
    },
    {
      name: "CA Arjun Rao",
      role: "Senior Partner, Audit & Compliance",
      content:
        "Impressed by how Srujan integrated complex requirements: auditor tracking, check-in/out systems, and real-time messaging for my firm. He understood the auditing workflow perfectly and built a seamless suite.",
      stars: 4.5,
    },
    {
      name: "GV Infotech Team",
      role: "Strategic Lead, PayBazaar Fintech",
      content:
        "Srujan and Shubhang have a clear vision for fintech reliability. From DMT and AEPS to mobile recharges, they made every transaction smooth and secure. The dashboard is incredibly intuitive and robust.",
      stars: 4.5,
    },
  ];

  onMount(() => {
    // Typing Animation
    const typeWriter = () => {
      const currentWord = heroWords[currentWordIndex];
      if (!isDeleting && displayText.length < currentWord.length) {
        displayText = currentWord.slice(0, displayText.length + 1);
        setTimeout(typeWriter, 100);
      } else if (isDeleting && displayText.length > 0) {
        displayText = currentWord.slice(0, displayText.length - 1);
        setTimeout(typeWriter, 50);
      } else if (!isDeleting && displayText.length === currentWord.length) {
        setTimeout(() => {
          isDeleting = true;
          typeWriter();
        }, 2000);
      } else if (isDeleting && displayText.length === 0) {
        isDeleting = false;
        currentWordIndex = (currentWordIndex + 1) % heroWords.length;
        setTimeout(typeWriter, 500);
      }
    };
    typeWriter();

    // Custom Cursor
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

    // Scroll Observer
    const handleScroll = () => {
      const winHeight = window.innerHeight;
      const docHeight = document.documentElement.scrollHeight;
      const scrollTop = window.scrollY;
      scrollProgress = (scrollTop / (docHeight - winHeight)) * 100;
    };
    window.addEventListener("scroll", handleScroll);

    // Intersection Observer for Reveal
    const observer = new IntersectionObserver(
      (entries) => {
        entries.forEach((entry) => {
          if (entry.isIntersecting) {
            entry.target.classList.add("reveal-visible");
          }
        });
      },
      { threshold: 0.15 },
    );

    document.querySelectorAll(".reveal").forEach((el) => observer.observe(el));

    return () => {
      window.removeEventListener("mousemove", handleMouseMove);
      window.removeEventListener("scroll", handleScroll);
    };
  });
</script>

<svelte:head>
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link
    rel="preconnect"
    href="https://fonts.gstatic.com"
    crossorigin="anonymous"
  />
  <link
    href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;800&family=Plus+Jakarta+Sans:wght@300;400;600;800&display=swap"
    rel="stylesheet"
  />
  <title>Srujan KM | Freelance Full-Stack Developer India</title>
  <meta
    name="description"
    content="Professional freelance web developer and full-stack software engineer in India. I build high-performance Golang backends, React, Next.js & SvelteKit web apps for global clients."
  />
  <meta
    name="keywords"
    content="freelance web developer, full stack developer India, Golang developer freelance, Next.js developer freelance, React expert India, web app developer, hire software developer"
  />
</svelte:head>

<!-- Cursor -->
<div
  class="fixed pointer-events-none z-[9999] mix-blend-exclusion text-white hidden md:block"
  style="left: {cursorX}px; top: {cursorY}px; transform: translate(-50%, -50%)"
>
  <div
    class="cursor-dot {isHovering
      ? 'scale-[4]'
      : 'scale-1'} transition-transform duration-300"
  ></div>
</div>

<div
  class="bg-[#050505] text-white selection:bg-white/20 overflow-x-hidden pt-6"
>
  <!-- Progress Bar -->
  <div
    class="fixed top-0 left-0 h-[2px] bg-white z-[100]"
    style="width: {scrollProgress}%"
  ></div>

  <!-- Navbar -->
  <nav
    class="fixed top-6 left-1/2 -translate-x-1/2 z-50 w-[min(90vw,600px)] backdrop-blur-2xl border border-white/10 rounded-full py-4 px-8 flex justify-center items-center transition-all duration-300 hover:border-white/20"
  >
    <div
      class="flex gap-4 md:gap-8 text-[10px] md:text-xs font-medium uppercase tracking-[0.2em] opacity-60"
    >
      <a
        href="#services"
        class="hover:text-white hover:opacity-100 transition-all cursor-none"
        on:mouseenter={() => (isHovering = true)}
        on:mouseleave={() => (isHovering = false)}>Expertise</a
      >
      <a
        href="#work"
        class="hover:text-white hover:opacity-100 transition-all cursor-none"
        on:mouseenter={() => (isHovering = true)}
        on:mouseleave={() => (isHovering = false)}>Portfolio</a
      >
      <a
        href="/blog"
        class="hover:text-white hover:opacity-100 transition-all cursor-none hidden sm:block"
        on:mouseenter={() => (isHovering = true)}
        on:mouseleave={() => (isHovering = false)}>Articles</a
      >
      <a
        href="#reviews"
        class="hover:text-white hover:opacity-100 transition-all cursor-none hidden sm:block"
        on:mouseenter={() => (isHovering = true)}
        on:mouseleave={() => (isHovering = false)}>Reviews</a
      >
      <a
        href="#contact"
        class="hover:text-white hover:opacity-100 transition-all cursor-none"
        on:mouseenter={() => (isHovering = true)}
        on:mouseleave={() => (isHovering = false)}>Contact</a
      >
    </div>
  </nav>

  <!-- Hero Section -->
  <section
    class="relative min-h-[110vh] flex items-center justify-center px-6 overflow-hidden"
  >
    <!-- Sophisticated Background -->
    <div class="absolute inset-0 z-0">
      <div
        class="absolute top-0 right-0 w-[800px] h-[800px] bg-blue-500/5 rounded-full blur-[150px] -translate-y-1/2 translate-x-1/4"
      ></div>
      <div
        class="absolute bottom-0 left-0 w-[600px] h-[600px] bg-purple-500/5 rounded-full blur-[120px] translate-y-1/3 -translate-x-1/4"
      ></div>
      <div class="grid-layer opacity-[0.03]"></div>
    </div>

    <!-- Video Background Overlay -->
    <div
      class="absolute inset-0 z-0 pointer-events-none opacity-20 filter grayscale contrast-[150%]"
    >
      <video autoplay loop muted playsinline class="w-full h-full object-cover">
        <source src="/videos/hero.mp4" type="video/mp4" />
      </video>
    </div>

    <div
      class="container relative z-10 max-w-7xl mx-auto flex flex-col items-center text-center"
    >
      <div class="flex flex-col gap-6 mb-8 reveal stagger-1">
        <h1
          class="text-[clamp(3.5rem,12vw,10rem)] font-[900] leading-[0.85] tracking-[-0.05em] uppercase italic"
        >
          <span
            class="text-white/40 block text-[0.3em] font-bold tracking-[0.5em] not-italic mb-8"
            >HELLO, I AM</span
          >
          Srujan KM
        </h1>
      </div>

      <div class="h-12 mb-8 reveal stagger-2">
        <p
          class="text-2xl md:text-4xl font-[500] text-white tracking-tight uppercase"
        >
          {displayText}<span class="animate-pulse text-white/50">_</span>
        </p>
      </div>

      <div class="mb-16 reveal stagger-2">
        <p
          class="text-[10px] md:text-xs font-bold uppercase tracking-[0.6em] text-white/30"
        >
          Build your vision into a <span class="text-white"
            >scalable digital reality.</span
          >
        </p>
      </div>

      <div
        class="flex flex-col md:flex-row gap-6 reveal stagger-3 justify-center w-full max-w-xl mx-auto"
      >
        <a
          href="mailto:developersrujan12@gmail.com"
          class="flex-1 px-12 py-6 bg-white text-black font-bold uppercase text-xs tracking-[0.3em] rounded-2xl hover:scale-105 transition-all text-center cursor-none"
          on:mouseenter={() => (isHovering = true)}
          on:mouseleave={() => (isHovering = false)}
        >
          Start Your Project Build
        </a>
        <a
          href="#work"
          class="flex-1 px-12 py-6 bg-white/5 border border-white/10 text-white font-bold uppercase text-xs tracking-[0.3em] rounded-2xl hover:bg-white/10 transition-all text-center cursor-none"
          on:mouseenter={() => (isHovering = true)}
          on:mouseleave={() => (isHovering = false)}
        >
          View Case Studies
        </a>
      </div>
    </div>

    <!-- Mouse Scroll Hint -->
    <div class="absolute bottom-12 left-1/2 -translate-x-1/2 reveal opacity-30">
      <div
        class="w-6 h-10 border-2 border-white/30 rounded-full flex justify-center p-2"
      >
        <div class="w-1 h-2 bg-white/50 rounded-full animate-bounce"></div>
      </div>
    </div>
  </section>

  <!-- SINGLE LINE Infinite Tech Marquee -->
  <section
    class="py-16 bg-white/[0.02] overflow-hidden border-y border-white/5"
  >
    <div
      class="marquee-group flex opacity-30 hover:opacity-100 transition-opacity duration-700 group cursor-default"
    >
      <div class="flex whitespace-nowrap animate-marquee">
        {#each [...techStack, ...techStack, ...techStack, ...techStack] as tech}
          <div
            class="flex items-center gap-8 md:gap-16 px-8 md:px-16 py-8 transition-all duration-500 hover:scale-125"
          >
            <div
              class="w-16 h-16 md:w-24 md:h-24 flex items-center justify-center filter drop-shadow-[0_0_15px_rgba(255,255,255,0.1)]"
            >
              <img
                src={tech.logo}
                alt={tech.name}
                class="max-w-full max-h-full object-contain brightness-0 invert opacity-60 group-hover:opacity-90 transition-opacity"
              />
            </div>
          </div>
        {/each}
      </div>
    </div>
  </section>

  <!-- Services / Expertise Section -->
  <section id="services" class="py-32 relative overflow-hidden">
    <div class="container max-w-7xl mx-auto px-6 relative">
      <!-- Section Background Glow -->
      <div
        class="absolute top-0 right-0 w-[500px] h-[500px] bg-blue-500/10 rounded-full blur-[150px] pointer-events-none -z-10"
      ></div>
      <div
        class="absolute bottom-0 left-0 w-[500px] h-[500px] bg-purple-500/10 rounded-full blur-[150px] pointer-events-none -z-10"
      ></div>

      <div
        class="flex flex-col md:flex-row justify-between items-end mb-24 reveal"
      >
        <div class="max-w-4xl">
          <h2
            class="text-5xl md:text-7xl font-[800] tracking-tighter mb-8 leading-none text-transparent bg-clip-text bg-gradient-to-r from-white to-white/30"
          >
            Services I Provide
          </h2>
          <p class="text-white/40 text-lg md:text-xl font-medium max-w-md">
            I provide end-to-end digital solutions, from stunning user
            interfaces to robust backend architectures.
          </p>
        </div>
        <div class="hidden md:block py-10 opacity-10">
          <span class="text-[10rem] font-[800] leading-none text-white"
            >01-06</span
          >
        </div>
      </div>

      <div
        class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6 md:gap-10"
      >
        <!-- Web Dev -->
        <div
          class="group relative p-12 rounded-[40px] bg-white/[0.03] border border-white/10 hover:bg-white/[0.05] transition-all duration-700 reveal stagger-1 overflow-hidden"
        >
          <div
            class="absolute -right-20 -top-20 w-64 h-64 bg-blue-500/5 blur-[100px] rounded-full group-hover:bg-blue-500/10 transition-all duration-700"
          ></div>
          <div class="relative z-10 text-center md:text-left">
            <span
              class="text-xs font-bold uppercase tracking-[0.4em] text-white/30 mb-8 block font-elite italic"
              >Step 01 / Web</span
            >
            <h3 class="text-3xl font-bold mb-6">
              Custom Website<br />Development
            </h3>
            <p class="text-white/50 leading-relaxed mb-10 text-lg">
              I build fast, professional websites that look great on any device.
              My focus is on making your business easy to find and easy for your
              customers to use.
            </p>
            <div
              class="flex flex-wrap gap-2 pt-4 border-t border-white/5 justify-center md:justify-start"
            >
              <span
                class="px-4 py-2 rounded-full border border-white/5 text-[10px] uppercase font-bold text-white/40"
                >Business Sites</span
              >
              <span
                class="px-4 py-2 rounded-full border border-white/5 text-[10px] uppercase font-bold text-white/40"
                >E-Commerce</span
              >
            </div>
          </div>
        </div>

        <!-- App Dev -->
        <div
          class="group relative p-12 rounded-[40px] bg-white/[0.03] border border-white/10 hover:bg-white/[0.05] transition-all duration-700 reveal stagger-2 overflow-hidden"
        >
          <div
            class="absolute -right-20 -top-20 w-64 h-64 bg-purple-500/5 blur-[100px] rounded-full group-hover:bg-purple-500/10 transition-all duration-700"
          ></div>
          <div class="relative z-10 text-center md:text-left">
            <span
              class="text-xs font-bold uppercase tracking-[0.4em] text-white/30 mb-8 block font-elite italic"
              >Step 02 / Mobile</span
            >
            <h3 class="text-3xl font-bold mb-6">Mobile App<br />Development</h3>
            <p class="text-white/50 leading-relaxed mb-10 text-lg">
              I create high-quality apps for <span class="text-white font-bold"
                >iPhone and Android</span
              >. From new ideas to modernizing old apps, I handle everything
              from start to finish.
            </p>
            <div
              class="flex flex-wrap gap-2 pt-4 border-t border-white/5 justify-center md:justify-start"
            >
              <span
                class="px-4 py-2 rounded-full border border-white/5 text-[10px] uppercase font-bold text-white/40"
                >iOS Apps</span
              >
              <span
                class="px-4 py-2 rounded-full border border-white/5 text-[10px] uppercase font-bold text-white/40"
                >Android Apps</span
              >
            </div>
          </div>
        </div>

        <!-- SEO -->
        <div
          class="group relative p-12 rounded-[40px] bg-white/[0.03] border border-white/10 hover:bg-white/[0.05] transition-all duration-700 reveal stagger-3 overflow-hidden"
        >
          <div
            class="absolute -right-20 -top-20 w-64 h-64 bg-emerald-500/5 blur-[100px] rounded-full group-hover:bg-emerald-500/10 transition-all duration-700"
          ></div>
          <div class="relative z-10 text-center md:text-left">
            <span
              class="text-xs font-bold uppercase tracking-[0.4em] text-white/30 mb-8 block font-elite italic"
              >Step 03 / Growth</span
            >
            <h3 class="text-3xl font-bold mb-6">SEO & Digital<br />Strategy</h3>
            <p class="text-white/50 leading-relaxed mb-10 text-lg">
              I help your business rank at the top of google to attract more
              customers. I use advanced tools to make sure your brand is the
              first one people see when they search.
            </p>
            <div
              class="flex flex-wrap gap-2 pt-4 border-t border-white/5 justify-center md:justify-start"
            >
              <span
                class="px-4 py-2 rounded-full border border-white/5 text-[10px] uppercase font-bold text-white/40"
                >Google Ranking</span
              >
              <span
                class="px-4 py-2 rounded-full border border-white/5 text-[10px] uppercase font-bold text-white/40"
                >Visibility</span
              >
            </div>
          </div>
        </div>

        <!-- Full Stack -->
        <div
          class="group relative p-12 rounded-[40px] bg-white/[0.03] border border-white/10 hover:bg-white/[0.05] transition-all duration-700 reveal stagger-1 overflow-hidden transition-all"
        >
          <div
            class="absolute -right-20 -top-20 w-64 h-64 bg-amber-500/5 blur-[100px] rounded-full group-hover:bg-amber-500/10 transition-all duration-700"
          ></div>
          <div class="relative z-10 text-center md:text-left">
            <span
              class="text-xs font-bold uppercase tracking-[0.4em] text-white/30 mb-8 block font-elite italic"
              >Step 04 / Product</span
            >
            <h3 class="text-3xl font-bold mb-6">
              Full-Stack Software<br />Solutions
            </h3>
            <p class="text-white/50 leading-relaxed mb-10 text-lg">
              I build entire digital products from scratch. From the buttons you
              click to the data behind it, I handle the whole "Full-Stack"
              system for your business.
            </p>
            <div
              class="flex flex-wrap gap-2 pt-4 border-t border-white/5 justify-center md:justify-start"
            >
              <span
                class="px-4 py-2 rounded-full border border-white/5 text-[10px] uppercase font-bold text-white/40"
                >Complete Systems</span
              >
              <span
                class="px-4 py-2 rounded-full border border-white/5 text-[10px] uppercase font-bold text-white/40"
                >Architecture</span
              >
            </div>
          </div>
        </div>

        <!-- Backend -->
        <div
          class="group relative p-12 rounded-[40px] bg-white/[0.03] border border-white/10 hover:bg-white/[0.05] transition-all duration-700 reveal stagger-2 overflow-hidden"
        >
          <div
            class="absolute -right-20 -top-20 w-64 h-64 bg-rose-500/5 blur-[100px] rounded-full group-hover:bg-rose-500/10 transition-all duration-700"
          ></div>
          <div class="relative z-10 text-center md:text-left">
            <span
              class="text-xs font-bold uppercase tracking-[0.4em] text-white/30 mb-8 block font-elite italic"
              >Step 05 / Engineering</span
            >
            <h3 class="text-3xl font-bold mb-6">Backend Development(Golang)</h3>
            <p class="text-white/50 leading-relaxed mb-10 text-lg">
              Using specialized tools like <span class="text-white font-bold"
                >Golang</span
              >, I build the "brain" of your app. This ensures your data is
              fast, secure, and always reliable.
            </p>
            <div
              class="flex flex-wrap gap-2 pt-4 border-t border-white/5 justify-center md:justify-start"
            >
              <span
                class="px-4 py-2 rounded-full border border-white/5 text-[10px] uppercase font-bold text-white/40"
                >Golang</span
              >
              <span
                class="px-4 py-2 rounded-full border border-white/5 text-[10px] uppercase font-bold text-white/40"
                >Security</span
              >
            </div>
          </div>
        </div>

        <!-- UI/UX -->
        <div
          class="group relative p-12 rounded-[40px] bg-white/[0.03] border border-white/10 hover:bg-white/[0.05] transition-all duration-700 reveal stagger-3 overflow-hidden"
        >
          <div
            class="absolute -right-20 -top-20 w-64 h-64 bg-cyan-500/5 blur-[100px] rounded-full group-hover:bg-cyan-500/10 transition-all duration-700"
          ></div>
          <div class="relative z-10 text-center md:text-left">
            <span
              class="text-xs font-bold uppercase tracking-[0.4em] text-white/30 mb-8 block font-elite italic"
              >Step 06 / Vision</span
            >
            <h3 class="text-3xl font-bold mb-6">UI/UX Product<br />Design</h3>
            <p class="text-white/50 leading-relaxed mb-10 text-lg">
              I design how your product looks and feels. I create modern,
              intuitive designs that make your software beautiful and exciting
              for your users to use.
            </p>
            <div
              class="flex flex-wrap gap-2 pt-4 border-t border-white/5 justify-center md:justify-start"
            >
              <span
                class="px-4 py-2 rounded-full border border-white/5 text-[10px] uppercase font-bold text-white/40"
                >Modern Design</span
              >
              <span
                class="px-4 py-2 rounded-full border border-white/5 text-[10px] uppercase font-bold text-white/40"
                >User Flows</span
              >
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>

  <!-- Professional Roadmap -->
  <section
    class="py-32 px-6 border-t border-white/5 relative overflow-hidden"
    id="process"
  >
    <div
      class="absolute top-0 left-1/2 -translate-x-1/2 w-full h-[1px] bg-gradient-to-r from-transparent via-white/10 to-transparent"
    ></div>

    <div class="container max-w-7xl mx-auto">
      <div class="flex flex-col items-center text-center mb-24 reveal">
        <span
          class="text-xs font-bold uppercase tracking-[0.5em] text-white/30 mb-6 block"
          >Our Path to Success</span
        >
        <h2
          class="text-5xl md:text-7xl font-[800] tracking-tighter mb-8 italic uppercase text-white"
        >
          The Professional<br />Roadmap
        </h2>
        <p class="text-white/40 max-w-xl text-lg">
          A simple, 4-step process designed to take your idea from a concept to
          a high-performing digital reality.
        </p>
      </div>

      <div
        class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-6 md:gap-8"
      >
        <!-- Step 1 -->
        <div
          class="group relative p-10 rounded-[35px] bg-white/[0.02] border border-white/5 hover:bg-white/[0.05] transition-all duration-500 reveal stagger-1"
        >
          <span
            class="text-5xl font-black text-white/5 mb-8 block group-hover:text-white/10 transition-colors uppercase italic"
            >01</span
          >
          <h3
            class="text-xl font-bold mb-4 uppercase tracking-wider text-white"
          >
            Goal Setting & Planning
          </h3>
          <p class="text-white/40 text-sm leading-relaxed">
            We start by understanding your business goals, target audience, and
            exactly what you need to achieve success.
          </p>
        </div>

        <!-- Step 2 -->
        <div
          class="group relative p-10 rounded-[35px] bg-white/[0.02] border border-white/5 hover:bg-white/[0.05] transition-all duration-500 reveal stagger-2"
        >
          <span
            class="text-5xl font-black text-white/5 mb-8 block group-hover:text-white/10 transition-colors uppercase italic"
            >02</span
          >
          <h3
            class="text-xl font-bold mb-4 uppercase tracking-wider text-white"
          >
            Blueprint & Design
          </h3>
          <p class="text-white/40 text-sm leading-relaxed">
            I create a visual and technical blueprint of your product, ensuring
            everything is planned perfectly before we write any code.
          </p>
        </div>

        <!-- Step 3 -->
        <div
          class="group relative p-10 rounded-[35px] bg-white/[0.02] border border-white/5 hover:bg-white/[0.05] transition-all duration-500 reveal stagger-3"
        >
          <span
            class="text-5xl font-black text-white/5 mb-8 block group-hover:text-white/10 transition-colors uppercase italic"
            >03</span
          >
          <h3
            class="text-xl font-bold mb-4 uppercase tracking-wider text-white"
          >
            Building Your Product
          </h3>
          <p class="text-white/40 text-sm leading-relaxed">
            This is where the magic happens. I build your software using the
            latest tech to ensure it's fast, secure, and easy to use.
          </p>
        </div>

        <!-- Step 4 -->
        <div
          class="group relative p-10 rounded-[35px] bg-white/[0.02] border border-white/5 hover:bg-white/[0.05] transition-all duration-500 reveal stagger-4"
        >
          <span
            class="text-5xl font-black text-white/5 mb-8 block group-hover:text-white/10 transition-colors uppercase italic"
            >04</span
          >
          <h3
            class="text-xl font-bold mb-4 uppercase tracking-wider text-white"
          >
            Launching & Growing
          </h3>
          <p class="text-white/40 text-sm leading-relaxed">
            We launch your product to the world and monitor its performance,
            making sure it scales as your business grows.
          </p>
        </div>
      </div>
    </div>
  </section>

  <!-- Projects Section (Case-Study Style) -->
  <section id="work" class="py-32 bg-[#050505]">
    <div class="container max-w-7xl mx-auto px-6">
      <div
        class="flex flex-col md:flex-row justify-between items-end mb-24 reveal"
      >
        <div class="max-w-2xl">
          <h2
            class="text-6xl md:text-9xl font-[800] tracking-tighter mb-8 leading-none"
          >
            High-Impact<br />Deliverables
          </h2>
          <p class="text-white/40 text-lg md:text-xl font-medium max-w-md">
            Transforming technical complexity into seamless business value
            through professional engineering.
          </p>
        </div>
      </div>

      <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
        {#each projects as project, i}
          <div
            role="link"
            tabindex="0"
            on:click={() => project.link && window.open(project.link, "_blank")}
            on:keydown={(e) =>
              e.key === "Enter" &&
              project.link &&
              window.open(project.link, "_blank")}
            class="group relative aspect-[14/16] rounded-[48px] overflow-hidden bg-[#111] reveal stagger-{i +
              1} border border-white/5 cursor-none block"
            on:mouseenter={() => (isHovering = true)}
            on:mouseleave={() => (isHovering = false)}
          >
            <!-- Project Image -->
            <img
              src={project.image}
              alt={project.title}
              class="absolute inset-0 w-full h-full object-cover transition-transform duration-1000 group-hover:scale-110 opacity-60 group-hover:opacity-100"
            />
            <div
              class="absolute inset-0 bg-gradient-to-t from-black via-black/40 to-transparent z-10"
            ></div>

            <!-- Dynamic Content Overlay -->
            <div
              class="absolute inset-0 p-12 flex flex-col justify-end z-20 group-hover:bg-white/5 transition-all duration-700"
            >
              <div
                class="translate-y-8 group-hover:translate-y-0 transition-transform duration-700"
              >
                <span
                  class="text-xs font-bold uppercase tracking-[0.4em] text-white/40 mb-4 block"
                  >{project.category}</span
                >
                <h3 class="text-4xl font-bold mb-6 text-white leading-tight">
                  {project.title}
                </h3>

                <div
                  class="grid grid-cols-1 gap-2 opacity-0 group-hover:opacity-100 transition-all duration-700 delay-100 mb-8"
                >
                  <p
                    class="text-[12px] font-bold text-white/60 mb-3 leading-relaxed"
                  >
                    <span class="text-white block mb-1">STRATEGIC GOAL:</span>
                    {project.description ||
                      "Deploying high-performance digital architecture for global scalability."}
                  </p>

                  {#if project.appLink}
                    <a
                      href={project.appLink}
                      class="flex items-center gap-2 text-[10px] font-bold uppercase tracking-widest text-emerald-400 hover:text-emerald-300 transition-colors cursor-none w-fit pb-4"
                      on:mouseenter={() => (isHovering = true)}
                      on:mouseleave={() => (isHovering = false)}
                    >
                      <svg
                        class="w-4 h-4"
                        fill="none"
                        stroke="currentColor"
                        viewBox="0 0 24 24"
                        ><path
                          stroke-linecap="round"
                          stroke-linejoin="round"
                          stroke-width="2"
                          d="M4 16v1a3 3 0 003 3h10a3 3 0 003-3v-1m-4-4l-4 4m0 0l-4-4m4 4V4"
                        /></svg
                      >
                      Download Android Solution
                    </a>
                  {/if}
                </div>

                <div
                  class="flex flex-wrap gap-2 text-white/30 text-xs font-bold uppercase tracking-widest pt-6 border-t border-white/10 group-hover:text-white transition-colors duration-700"
                >
                  {#each project.stack as tech}
                    <span>{tech}</span>
                    {#if tech !== project.stack[project.stack.length - 1]}
                      <span class="mx-1">•</span>
                    {/if}
                  {/each}
                </div>
              </div>
            </div>
          </div>
        {/each}
      </div>
    </div>
  </section>

  <!-- Client Reviews Section -->
  <section id="reviews" class="py-32 bg-white/[0.01]">
    <div class="container max-w-7xl mx-auto px-6">
      <div class="text-center mb-24 reveal">
        <h2
          class="text-4xl md:text-7xl font-[800] mb-8 tracking-tighter uppercase italic"
        >
          Trusted by<br />Global Partners
        </h2>
        <p class="text-white/40 uppercase tracking-[0.3em] text-xs font-bold">
          Proven Results via High-Performance Collaboration
        </p>
      </div>

      <div
        class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-2 xl:grid-cols-4 gap-8"
      >
        {#each reviews as review}
          <div
            class="p-10 rounded-3xl bg-white/5 border border-white/10 backdrop-blur-xl reveal stagger-1 hover:border-white/40 transition-colors duration-500 group flex flex-col justify-between"
          >
            <div>
              <div class="mb-8">
                <h4 class="font-bold text-sm">{review.name}</h4>
                <p class="text-[10px] text-white/40 uppercase tracking-widest">
                  {review.role}
                </p>
              </div>
              <p class="text-white/70 leading-relaxed italic text-sm">
                "{review.content}"
              </p>
            </div>
            <div
              class="mt-8 flex gap-1 opacity-20 group-hover:opacity-100 transition-opacity"
            >
              {#each Array(Math.floor(review.stars)) as _}
                <span class="text-amber-400">★</span>
              {/each}
              {#if review.stars % 1 !== 0}
                <span class="text-amber-400">☆</span>
              {/if}
            </div>
          </div>
        {/each}
      </div>
    </div>
  </section>

  <!-- FAQ Section -->
  <section id="faq" class="py-32 bg-[#050505] border-t border-white/5">
    <div class="container max-w-7xl mx-auto px-6">
      <div class="grid grid-cols-1 md:grid-cols-2 gap-24">
        <div class="reveal">
          <h2
            class="text-6xl md:text-8xl font-[800] tracking-tighter mb-8 text-white leading-tight uppercase italic"
          >
            Common <br />Questions
          </h2>
          <p class="text-white/40 text-lg md:text-xl font-medium max-w-md">
            Find answers to common questions about timelines, costs, and my
            professional process.
          </p>
        </div>

        <div class="space-y-12">
          {#each [{ q: "How long does a project take?", a: "Most custom projects take 6-12 weeks, depending on the complexity of your requirements and goals." }, { q: "What technology do you use?", a: "I specialize in high-performance stacks like Golang, Next.js 15 (React), and SvelteKit because they are the most secure and scalable for modern business success." }, { q: "Can we work together remotely?", a: "Yes! I work with business owners globally using clear, daily communication to ensure alignment." }, { q: "Do you help with existing apps?", a: "Absolutely. I can help migrate your old software to the cloud or update it with new features to improve performance." }, { q: "Is my business data safe?", a: "Security is my top priority. I build every system with bank-level security to protect your business and your users." }] as faq, i}
            <div class="reveal stagger-{i + 1} border-b border-white/5 pb-12">
              <h3
                class="text-xl font-bold mb-4 text-white leading-tight uppercase tracking-wide opacity-100 italic"
              >
                {faq.q}
              </h3>
              <p class="text-white/40 leading-relaxed font-medium">
                {faq.a}
              </p>
            </div>
          {/each}
        </div>
      </div>
    </div>
  </section>

  <!-- Final Contact Section -->
  <section id="contact" class="py-40 bg-[#050505] relative overflow-hidden">
    <!-- Sophisticated Abstract Glow -->
    <div
      class="absolute -top-[50%] -left-[20%] w-[100%] h-[100%] bg-white/[0.02] blur-[150px] rounded-full"
    ></div>
    <div
      class="absolute -bottom-[50%] -right-[20%] w-[100%] h-[100%] bg-blue-500/[0.02] blur-[150px] rounded-full"
    ></div>

    <div class="container max-w-5xl mx-auto px-6 text-center relative z-10">
      <div class="reveal">
        <span
          class="text-xs font-bold uppercase tracking-[0.6em] text-white/30 mb-8 block reveal stagger-1"
          >Ready to Scale?</span
        >
        <h2
          class="text-6xl md:text-9xl font-[800] tracking-tighter mb-20 leading-[0.85] uppercase italic"
        >
          Let's Build Your<br />Digital Legacy
        </h2>

        <div class="flex flex-col items-center gap-16 reveal stagger-2">
          <a
            href="mailto:developersrujan12@gmail.com"
            class="w-full md:w-auto px-20 py-10 bg-white text-black font-bold uppercase text-sm tracking-[0.2em] rounded-full hover:scale-105 transition-all text-center cursor-none shadow-[0_0_50px_rgba(255,255,255,0.1)]"
            on:mouseenter={() => (isHovering = true)}
            on:mouseleave={() => (isHovering = false)}
          >
            Start Your Project Build
          </a>

          <div
            class="flex flex-wrap gap-12 items-center pt-16 border-t border-white/5 w-full justify-center"
          >
            <a
              href="https://www.linkedin.com/in/srujan-km-12s/"
              target="_blank"
              class="text-[10px] font-bold uppercase tracking-[0.3em] text-white/30 hover:text-white transition-colors cursor-none py-2"
              on:mouseenter={() => (isHovering = true)}
              on:mouseleave={() => (isHovering = false)}>LinkedIn</a
            >
            <a
              href="https://github.com/Srujankm12"
              target="_blank"
              class="text-[10px] font-bold uppercase tracking-[0.3em] text-white/30 hover:text-white transition-colors cursor-none py-2"
              on:mouseenter={() => (isHovering = true)}
              on:mouseleave={() => (isHovering = false)}>GitHub</a
            >
            <a
              href="https://x.com/DeveloperS29854"
              target="_blank"
              class="text-[10px] font-bold uppercase tracking-[0.3em] text-white/30 hover:text-white transition-colors cursor-none py-2"
              on:mouseenter={() => (isHovering = true)}
              on:mouseleave={() => (isHovering = false)}>Twitter</a
            >
          </div>
        </div>
      </div>
    </div>
  </section>

  <!-- Navigation Info -->
  <footer class="py-24 bg-[#050505] border-t border-white/5">
    <div class="container max-w-7xl mx-auto px-6">
      <div
        class="grid grid-cols-1 md:grid-cols-3 gap-16 text-center md:text-left mb-24"
      >
        <div class="reveal stagger-1">
          <span
            class="text-[10px] font-bold uppercase tracking-[0.3em] text-white/20 block mb-6"
            >Global Availability</span
          >
          <p class="text-xl font-bold text-white/80 tracking-tight">
            Bengaluru • India<br /><span
              class="text-white/30 font-medium italic text-sm"
              >Working with founders worldwide</span
            >
          </p>
        </div>
        <div class="reveal stagger-2">
          <span
            class="text-[10px] font-bold uppercase tracking-[0.3em] text-white/20 block mb-6"
            >Core Services</span
          >
          <p class="text-xl font-bold text-white/80 tracking-tight">
            Full-Stack Solutions<br /><span
              class="text-white/30 font-medium italic text-sm"
              >Consulting & Architecture</span
            >
          </p>
        </div>
        <div class="reveal stagger-3">
          <span
            class="text-[10px] font-bold uppercase tracking-[0.3em] text-white/20 block mb-6"
            >Direct Contact</span
          >
          <p
            class="text-xl font-bold text-white/80 tracking-tight underline underline-offset-8 decoration-white/10"
          >
            developersrujan12@gmail.com<br /><span
              class="text-white/30 font-medium text-sm">+91 83100 29635</span
            >
          </p>
        </div>
      </div>
      <div
        class="flex flex-col md:flex-row justify-between items-center gap-8 pt-12 border-t border-white/5 text-[9px] font-bold uppercase tracking-[0.5em] text-white/10"
      >
        <p>© 2026 SKM — FULL STACK SOFTWARE SOLUTIONS</p>
        <p class="hover:text-white/30 transition-colors">EST. IN BENGALURU</p>
      </div>
    </div>
  </footer>
</div>

<style>
  :global(html) {
    font-family: "Plus Jakarta Sans", sans-serif;
    scroll-behavior: smooth;
    cursor: none;
    background-color: #050505;
  }

  h1,
  h2,
  h3 {
    font-family: "Outfit", sans-serif;
  }

  .reveal {
    opacity: 0;
    transform: translateY(30px);
    transition:
      opacity 1s cubic-bezier(0.16, 1, 0.3, 1),
      transform 1s cubic-bezier(0.16, 1, 0.3, 1);
  }

  :global(.reveal-visible) {
    opacity: 1 !important;
    transform: translateY(0) !important;
  }

  .stagger-1 {
    transition-delay: 0.1s;
  }
  .stagger-2 {
    transition-delay: 0.2s;
  }
  .stagger-3 {
    transition-delay: 0.3s;
  }
  .stagger-4 {
    transition-delay: 0.4s;
  }

  /* Custom Cursor */
  .cursor-dot {
    width: 8px;
    height: 8px;
    background: white;
    border-radius: 50%;
  }

  /* Marquee */
  @keyframes marquee {
    0% {
      transform: translateX(0);
    }
    100% {
      transform: translateX(-25%);
    }
  }

  @keyframes marquee-reverse {
    0% {
      transform: translateX(-25%);
    }
    100% {
      transform: translateX(0);
    }
  }

  .animate-marquee {
    animation: marquee 15s linear infinite;
  }

  .animate-marquee-reverse {
    animation: marquee-reverse 15s linear infinite;
  }

  .group:hover .animate-marquee,
  .group:hover .animate-marquee-reverse {
    animation-play-state: paused;
  }

  /* Selection Override */
  ::selection {
    background: rgba(255, 255, 255, 0.99);
    color: black;
  }

  /* Video Smoothness */
  video {
    transition: opacity 2s ease-in-out;
  }
</style>
