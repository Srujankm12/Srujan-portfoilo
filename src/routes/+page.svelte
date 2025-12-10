<script lang="ts">
  import { onMount } from "svelte";

  let currentQuoteIndex = $state(0);
  let currentProjectIndex = $state(0);
  let isVideoLoaded = $state(false);
  let mouseX = $state(0);
  let mouseY = $state(0);
  let cursorX = $state(0);
  let cursorY = $state(0);
  let isHovering = $state(false);

  const quotes = [
    "Building scalable systems",
    "Code that matters",
    "Innovation through simplicity",
  ];

  const heroWords = ["Developer", "Engineer", "Creator", "Builder"];
  let currentWordIndex = $state(0);
  let displayText = $state("");
  let isDeleting = $state(false);

  const projects = [
    {
      title: "GFS Real Estate Platform",
      year: "2024",
      description:
        "Developed comprehensive real estate platform for GFS Developments, a global property developer operating across UAE, UK, Saudi Arabia, Germany, Canada, USA, Italy, Pakistan, Turkey, Spain, France, and Egypt. Built property listing system, CRM integration, and automated inquiry management for luxury residential and commercial projects.",
      tags: ["Next.js", "Node.js", "PostgreSQL", "AWS", "Real Estate CRM"],
      link: "https://gfs.nezdev.com/",
    },
    {
      title: "PayBazaar Fintech Portal",
      year: "2024",
      description:
        "Enterprise-grade fintech solution with comprehensive role-based access control. Implemented DMT (Domestic Money Transfer), AEPS, automated KYC verification, wallet management, commission tracking, and secure banking API integrations for seamless financial transactions.",
      tags: ["Next.js", "Golang", "PostgreSQL", "Banking APIs", "Security"],
      link: "#",
    },
    {
      title: "Cashew Trading E-Commerce",
      year: "2024",
      description:
        "Global B2B e-commerce platform for cashew and dry fruits trading worldwide. Built with multi-currency support, real-time inventory management, bulk order processing, international shipping integration, and supplier-buyer marketplace with quality certifications.",
      tags: ["React", "Node.js", "MongoDB", "Payment Gateway", "Logistics"],
      link: "#",
    },
    {
      title: "Chartered Accountant Dashboard",
      year: "2025",
      description:
        "Comprehensive management system for CA firms with biometric attendance, leave management, real-time messaging, auditor communication portal, client tracking, document management, and automated reporting. Features include task assignment and deadline tracking.",
      tags: ["SvelteKit", "Golang", "PostgreSQL", "WebSocket", "Biometric"],
      link: "#",
    },
    {
      title: "E-Commerce & Billing Suite",
      year: "2024",
      description:
        "Developed multiple e-commerce platforms and billing software solutions for retail businesses. Features include inventory management, point-of-sale integration, invoice generation, GST compliance, customer analytics, and multi-store management with cloud sync.",
      tags: ["React", "Express", "MongoDB", "Invoice System", "GST"],
      link: "#",
    },
    {
      title: "Telephone Recharge Platform",
      year: "2025",
      description:
        "Serverless recharge platform with Golang and AWS Lambda. Implemented multi-role APIs, operator integrations, secure JWT authentication, commission management, async email notifications using RabbitMQ, and real-time transaction monitoring.",
      tags: ["Golang", "AWS Lambda", "MongoDB", "RabbitMQ", "JWT"],
      link: "#",
    },
    {
      title: "VoiceVerse AI Assistant",
      year: "2025",
      description:
        "AI-powered voice assistant leveraging Whisper AI for speech recognition and LLaMA 3.2 for natural language understanding. Implemented RAG (Retrieval-Augmented Generation) with Qdrant vector database for context-aware responses and knowledge retrieval.",
      tags: ["Whisper AI", "LLaMA 3.2", "RAG", "Qdrant", "Python"],
      link: "#",
    },
    {
      title: "Biometric Attendance System",
      year: "2025",
      description:
        "Enterprise biometric authentication system with Flutter dashboard. Real-time attendance tracking, facial recognition, automated reporting, leave management, shift scheduling, and cloud synchronization with analytics and compliance reports.",
      tags: ["Flutter", "Golang", "WebSocket", "AWS", "Biometric"],
      link: "#",
    },
  ];

  const techStack = [
    {
      name: "Golang",
      logo: "https://go.dev/blog/go-brand/Go-Logo/PNG/Go-Logo_Blue.png",
    },
    { name: "Node.js", logo: "https://nodejs.org/static/images/logo.svg" },
    {
      name: "Python",
      logo: "https://www.python.org/static/community_logos/python-logo-generic.svg",
    },
    {
      name: "TypeScript",
      logo: "https://upload.wikimedia.org/wikipedia/commons/4/4c/Typescript_logo_2020.svg",
    },
    {
      name: "JavaScript",
      logo: "https://upload.wikimedia.org/wikipedia/commons/6/6a/JavaScript-logo.png",
    },
    {
      name: "React",
      logo: "https://upload.wikimedia.org/wikipedia/commons/a/a7/React-icon.svg",
    },
    {
      name: "React.js",
      logo: "https://upload.wikimedia.org/wikipedia/commons/a/a7/React-icon.svg",
    },
    {
      name: "Next.js",
      logo: "https://assets.vercel.com/image/upload/v1662130559/nextjs/Icon_light_background.png",
    },
    {
      name: "SvelteKit",
      logo: "https://upload.wikimedia.org/wikipedia/commons/1/1b/Svelte_Logo.svg",
    },
    {
      name: "Flutter",
      logo: "https://storage.googleapis.com/cms-storage-bucket/4fd0db61df0567c0f352.png",
    },
    {
      name: "Tailwind CSS",
      logo: "https://upload.wikimedia.org/wikipedia/commons/d/d5/Tailwind_CSS_Logo.svg",
    },
    {
      name: "AWS",
      logo: "https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg",
    },
    {
      name: "MongoDB",
      logo: "https://www.mongodb.com/assets/images/global/favicon.ico",
    },
    {
      name: "PostgreSQL",
      logo: "https://wiki.postgresql.org/images/a/a4/PostgreSQL_logo.3colors.svg",
    },
    { name: "Redis", logo: "https://redis.io/images/redis-white.png" },
    {
      name: "Docker",
      logo: "https://www.docker.com/wp-content/uploads/2022/03/Moby-logo.png",
    },
    {
      name: "REST API",
      logo: "https://img.icons8.com/color/96/api-settings.png",
    },
    {
      name: "GraphQL",
      logo: "https://upload.wikimedia.org/wikipedia/commons/1/17/GraphQL_Logo.svg",
    },
    {
      name: "WebSocket",
      logo: "https://img.icons8.com/color/96/websocket.png",
    },
    {
      name: "Git",
      logo: "https://git-scm.com/images/logos/downloads/Git-Icon-1788C.png",
    },
    {
      name: "Linux",
      logo: "https://upload.wikimedia.org/wikipedia/commons/3/35/Tux.svg",
    },
  ];

  const experiences = [
    {
      company: "Levion Studio",
      role: "Co-Founder & Full-Stack Developer",
      period: "Oct 2025 - Present",
      location: "Bengaluru, Karnataka",
      description:
        "Founded a digital solutions studio specializing in modern web applications and scalable systems. Leading full-stack development, client projects, and technical strategy. Building innovative solutions with cutting-edge technologies and delivering exceptional digital experiences.",
    },
    {
      company: "Vithsutra Technologies",
      role: "Co-Founder & Backend Developer",
      period: "Apr 2025 - Oct 2025",
      location: "Moodbidri, Karnataka",
      description:
        "Co-founded and engineered serverless backend systems with Golang and AWS Lambda. Developed biometric solutions with real-time analytics, integrated async messaging systems, and built scalable infrastructure for enterprise applications.",
    },
    {
      company: "SR Automation",
      role: "Full Stack Developer Intern",
      period: "Dec 2024 - Mar 2025",
      location: "Bangalore, Karnataka",
      description:
        "Built full-stack applications with SvelteKit and Go. Optimized REST APIs, implemented real-time features, and deployed scalable infrastructure on cloud platforms. Worked on automation systems and dashboard development.",
    },
  ];

  let scrollContainer: HTMLElement;

  onMount(() => {
    // Typing animation
    let typingSpeed = 150;
    let deletingSpeed = 100;
    let pauseTime = 2000;

    const typeWriter = () => {
      const currentWord = heroWords[currentWordIndex];

      if (!isDeleting && displayText.length < currentWord.length) {
        displayText = currentWord.slice(0, displayText.length + 1);
        setTimeout(typeWriter, typingSpeed);
      } else if (isDeleting && displayText.length > 0) {
        displayText = currentWord.slice(0, displayText.length - 1);
        setTimeout(typeWriter, deletingSpeed);
      } else if (!isDeleting && displayText.length === currentWord.length) {
        setTimeout(() => {
          isDeleting = true;
          typeWriter();
        }, pauseTime);
      } else if (isDeleting && displayText.length === 0) {
        isDeleting = false;
        currentWordIndex = (currentWordIndex + 1) % heroWords.length;
        setTimeout(typeWriter, 500);
      }
    };

    typeWriter();

    // Custom cursor
    const handleMouseMove = (e: MouseEvent) => {
      mouseX = e.clientX;
      mouseY = e.clientY;
    };

    window.addEventListener("mousemove", handleMouseMove);

    // Smooth cursor follow
    const animateCursor = () => {
      cursorX += (mouseX - cursorX) * 0.1;
      cursorY += (mouseY - cursorY) * 0.1;
      requestAnimationFrame(animateCursor);
    };
    animateCursor();

    // Rotate quotes
    const quoteInterval = setInterval(() => {
      currentQuoteIndex = (currentQuoteIndex + 1) % quotes.length;
    }, 3500);

    // Smooth scroll for navigation
    document.querySelectorAll('a[href^="#"]').forEach((anchor) => {
      anchor.addEventListener("click", (e) => {
        e.preventDefault();
        const target = anchor.getAttribute("href");
        if (target) {
          document
            .querySelector(target)
            ?.scrollIntoView({ behavior: "smooth" });
        }
      });
    });

    setTimeout(() => {
      isVideoLoaded = true;
    }, 500);

    return () => {
      window.removeEventListener("mousemove", handleMouseMove);
      clearInterval(quoteInterval);
    };
  });
</script>

<svelte:head>
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
  <link
    href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@300;400;500;600;700&family=Inter:wght@300;400;500;600&display=swap"
    rel="stylesheet"
  />
  <style>
    * {
      scroll-behavior: smooth !important;
    }
    body {
      overflow-x: hidden;
    }
  </style>
</svelte:head>

<!-- Custom Cursor -->
<div
  class="custom-cursor"
  style="transform: translate({cursorX}px, {cursorY}px)"
  class:cursor-hovering={isHovering}
></div>

<div class="min-h-screen bg-black text-white cursor-none">
  <!-- Minimal Navbar -->
  <nav class="fixed top-0 w-full z-50 mix-blend-difference">
    <div class="max-w-7xl mx-auto px-6 py-6 flex justify-between items-center">
      <div class="font-space text-sm font-medium tracking-wider">SK</div>
      <div class="flex gap-8 text-sm font-inter font-light">
        <a href="#hero" class="hover:opacity-60 transition-opacity">Work</a>
        <a href="#about" class="hover:opacity-60 transition-opacity">About</a>
        <a href="#contact" class="hover:opacity-60 transition-opacity"
          >Contact</a
        >
      </div>
    </div>
  </nav>

  <!-- Hero Section -->
  <section
    id="hero"
    class="relative min-h-screen flex items-center justify-center overflow-hidden"
  >
    <!-- Animated Grid Background -->
    <div class="absolute inset-0 grid-background"></div>

    <!-- Video Background - No overlay -->
    <div class="absolute inset-0 z-0">
      <video
        class="w-full h-full object-cover opacity-30"
        autoplay
        loop
        muted
        playsinline
      >
        <source src="/vedio/hero.mp4" type="video/mp4" />
      </video>
    </div>

    <!-- Gradient Orbs -->
    <div class="absolute inset-0 overflow-hidden z-0">
      <div class="orb orb-1"></div>
      <div class="orb orb-2"></div>
      <div class="orb orb-3"></div>
    </div>

    <!-- Content -->
    <div class="relative z-20 max-w-6xl mx-auto px-6 w-full">
      <div class="flex flex-col items-start">
        <!-- Small intro text -->
        <div class="overflow-hidden mb-6">
          <p
            class="font-inter text-sm tracking-widest text-white/40 uppercase slide-up"
          >
            Full-Stack Developer
          </p>
        </div>

        <!-- Main heading with typing effect -->
        <div class="overflow-hidden mb-4">
          <h1
            class="text-7xl md:text-9xl font-space font-bold tracking-tighter slide-up-delay-1"
          >
            Srujan KM
          </h1>
        </div>

        <!-- Animated subtitle -->
        <div class="overflow-hidden mb-12 h-24 md:h-32">
          <h2
            class="text-4xl md:text-6xl font-space font-light tracking-tight slide-up-delay-2"
          >
            <span class="text-white/50">I'm a </span>
            <span class="typing-text">{displayText}</span>
            <span class="typing-cursor">|</span>
          </h2>
        </div>

        <!-- Description -->
        <div class="overflow-hidden mb-12 max-w-2xl">
          <p
            class="font-inter text-lg md:text-xl text-white/60 leading-relaxed slide-up-delay-3"
          >
            Co-Founder of Levion Studio, crafting scalable systems and elegant
            solutions. Specializing in serverless architectures, modern web
            technologies, and innovative digital experiences.
          </p>
        </div>

        <!-- CTA Buttons -->
        <div class="flex gap-4 slide-up-delay-4">
          <a
            href="#work"
            class="group px-8 py-4 bg-white text-black font-inter font-medium relative overflow-hidden"
            on:mouseenter={() => (isHovering = true)}
            on:mouseleave={() => (isHovering = false)}
          >
            <span class="relative z-10">View Projects</span>
            <div
              class="absolute inset-0 bg-black translate-y-full group-hover:translate-y-0 transition-transform duration-300"
            ></div>
            <span
              class="absolute inset-0 flex items-center justify-center text-white opacity-0 group-hover:opacity-100 transition-opacity duration-300 z-10"
            >
              View Projects
            </span>
          </a>
          <a
            href="#contact"
            class="px-8 py-4 border border-white/30 font-inter font-medium hover:bg-white hover:text-black transition-all duration-300"
            on:mouseenter={() => (isHovering = true)}
            on:mouseleave={() => (isHovering = false)}
          >
            Get In Touch
          </a>
        </div>

        <!-- Stats -->
        <div class="grid grid-cols-3 gap-8 mt-24 slide-up-delay-5">
          <div>
            <div class="text-4xl font-space font-bold mb-2">10+</div>
            <div class="text-sm font-inter text-white/40">Projects</div>
          </div>
          <div>
            <div class="text-4xl font-space font-bold mb-2">3+</div>
            <div class="text-sm font-inter text-white/40">Years</div>
          </div>
          <div>
            <div class="text-4xl font-space font-bold mb-2">20+</div>
            <div class="text-sm font-inter text-white/40">Technologies</div>
          </div>
        </div>
      </div>
    </div>

    <!-- Scroll Indicator -->
    <div class="absolute bottom-12 left-1/2 -translate-x-1/2 z-20">
      <div class="scroll-indicator">
        <div class="scroll-line"></div>
      </div>
      <p class="text-xs font-inter text-white/30 mt-4 tracking-widest">
        SCROLL
      </p>
    </div>
  </section>

  <!-- Projects Section -->
  <section id="work" class="relative py-40 overflow-hidden">
    <!-- Background Effects -->
    <div class="absolute inset-0 grid-background opacity-30"></div>
    <div
      class="absolute top-0 right-0 w-96 h-96 bg-white/5 rounded-full blur-3xl"
    ></div>

    <div class="relative z-10">
      <div class="max-w-7xl mx-auto px-6 mb-24">
        <div class="flex justify-between items-end">
          <div class="max-w-2xl">
            <div class="overflow-hidden mb-4">
              <p
                class="text-sm font-inter text-white/40 tracking-widest uppercase slide-up"
              >
                Portfolio
              </p>
            </div>
            <div class="overflow-hidden mb-6">
              <h2
                class="text-6xl md:text-8xl font-space font-bold tracking-tighter slide-up-delay-1"
              >
                Selected Work
              </h2>
            </div>
            <div class="overflow-hidden">
              <p class="text-lg text-white/50 font-inter slide-up-delay-2">
                Scroll horizontally to explore projects →
              </p>
            </div>
          </div>
          <div class="hidden md:block">
            <div class="text-7xl font-space font-bold text-white/5">
              0{projects.length}
            </div>
          </div>
        </div>
      </div>

      <!-- Horizontal Scroll Container -->
      <div
        bind:this={scrollContainer}
        class="flex gap-8 px-6 md:px-12 overflow-x-auto scrollbar-thin pb-8"
      >
        {#each projects as project, i (project.title)}
          <div class="project-card flex-shrink-0 w-[400px] md:w-[500px]">
            <div
              class="project-card-inner border border-white/10 p-10 h-full hover:border-white/30 transition-all duration-700 group relative overflow-hidden bg-black/40 backdrop-blur-sm"
            >
              <!-- Animated corner accent -->
              <div
                class="absolute top-0 right-0 w-32 h-32 border-t-2 border-r-2 border-white/0 group-hover:border-white/20 transition-all duration-700"
              ></div>

              <!-- Hover gradient effect -->
              <div
                class="absolute inset-0 bg-gradient-to-br from-white/5 via-transparent to-transparent opacity-0 group-hover:opacity-100 transition-opacity duration-700"
              ></div>

              <div class="relative z-10">
                <div class="flex justify-between items-start mb-8">
                  <span
                    class="text-7xl font-space font-bold text-white/5 group-hover:text-white/10 transition-colors duration-700"
                  >
                    {String(i + 1).padStart(2, "0")}
                  </span>
                  <span class="text-sm text-white/40 font-inter tracking-wider"
                    >{project.year}</span
                  >
                </div>

                <h3
                  class="text-3xl font-space font-bold mb-5 tracking-tight group-hover:tracking-tighter transition-all duration-300"
                >
                  {project.title}
                </h3>

                <p
                  class="text-white/60 font-inter leading-relaxed mb-10 text-sm"
                >
                  {project.description}
                </p>

                <div class="flex flex-wrap gap-2 mb-10">
                  {#each project.tags as tag}
                    <span
                      class="text-xs font-inter px-4 py-2 border border-white/20 text-white/70 hover:border-white/40 hover:bg-white/5 transition-all duration-300"
                    >
                      {tag}
                    </span>
                  {/each}
                </div>

                <div
                  class="flex items-center gap-3 text-sm font-inter font-medium opacity-50 group-hover:opacity-100 transition-all duration-500 group-hover:gap-4"
                >
                  <span class="tracking-wide">Explore Project</span>
                  <span class="text-xl">→</span>
                </div>
              </div>
            </div>
          </div>
        {/each}
      </div>
    </div>
  </section>

  <!-- Experience Section -->
  <section id="about" class="relative py-40 overflow-hidden">
    <!-- Background Effects -->
    <div class="absolute inset-0 grid-background opacity-20"></div>
    <div
      class="absolute bottom-0 left-0 w-96 h-96 bg-white/5 rounded-full blur-3xl"
    ></div>

    <div class="relative z-10 max-w-6xl mx-auto px-6">
      <div class="mb-24">
        <div class="overflow-hidden mb-4">
          <p
            class="text-sm font-inter text-white/40 tracking-widest uppercase slide-up"
          >
            Background
          </p>
        </div>
        <div class="overflow-hidden">
          <h2
            class="text-6xl md:text-8xl font-space font-bold tracking-tighter slide-up-delay-1"
          >
            Experience
          </h2>
        </div>
      </div>

      <div class="space-y-24">
        {#each experiences as exp, i}
          <div class="experience-item group">
            <div
              class="relative border-l-2 border-white/10 pl-10 hover:border-white/30 transition-all duration-700"
            >
              <!-- Animated dot -->
              <div
                class="absolute -left-[9px] top-0 w-4 h-4 bg-black border-2 border-white/20 rounded-full group-hover:border-white/60 group-hover:scale-150 transition-all duration-500"
              ></div>

              <div
                class="bg-black/30 backdrop-blur-sm border border-white/5 p-8 hover:border-white/10 transition-all duration-500"
              >
                <div
                  class="flex flex-col lg:flex-row lg:justify-between lg:items-start mb-6"
                >
                  <div>
                    <div class="overflow-hidden mb-3">
                      <h3
                        class="text-4xl md:text-5xl font-space font-bold tracking-tighter"
                      >
                        {exp.company}
                      </h3>
                    </div>
                    <p
                      class="text-white/70 font-inter font-medium text-xl tracking-wide"
                    >
                      {exp.role}
                    </p>
                  </div>
                  <div
                    class="text-white/40 font-inter text-sm mt-4 lg:mt-0 lg:text-right space-y-1"
                  >
                    <p class="text-lg">{exp.period}</p>
                    <p class="tracking-wider">{exp.location}</p>
                  </div>
                </div>

                <div class="w-16 h-px bg-white/20 mb-6"></div>

                <p
                  class="text-white/60 font-inter leading-relaxed text-lg max-w-3xl"
                >
                  {exp.description}
                </p>
              </div>
            </div>
          </div>
        {/each}
      </div>
    </div>
  </section>

  <!-- Tech Stack Carousel -->
  <section class="relative py-40 overflow-hidden">
    <!-- Background Effects -->
    <div class="absolute inset-0 grid-background opacity-20"></div>
    <div
      class="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 w-[600px] h-[600px] bg-white/5 rounded-full blur-3xl"
    ></div>

    <div class="relative z-10">
      <div class="max-w-7xl mx-auto px-6 mb-20">
        <div class="overflow-hidden mb-4">
          <p
            class="text-sm font-inter text-white/40 tracking-widest uppercase slide-up"
          >
            Technologies
          </p>
        </div>
        <div class="overflow-hidden">
          <h2
            class="text-6xl md:text-8xl font-space font-bold tracking-tighter slide-up-delay-1 mb-6"
          >
            Tech Stack
          </h2>
        </div>
        <div class="overflow-hidden">
          <p class="text-lg text-white/40 font-inter slide-up-delay-2">
            Tools and technologies I work with
          </p>
        </div>
      </div>

      <!-- First Row -->
      <div class="marquee-wrapper mb-8">
        <div class="marquee">
          {#each [...techStack, ...techStack] as tech}
            <div class="tech-tag-logo">
              <img src={tech.logo} alt={tech.name} class="tech-logo" />
              <span>{tech.name}</span>
            </div>
          {/each}
        </div>
      </div>

      <!-- Second Row -->
      <div class="marquee-wrapper-reverse">
        <div class="marquee-reverse">
          {#each [...techStack, ...techStack] as tech}
            <div class="tech-tag-logo">
              <img src={tech.logo} alt={tech.name} class="tech-logo" />
              <span>{tech.name}</span>
            </div>
          {/each}
        </div>
      </div>
    </div>
  </section>

  <!-- Contact Section -->
  <section id="contact" class="relative py-40 overflow-hidden">
    <!-- Background Effects -->
    <div class="absolute inset-0 grid-background opacity-30"></div>
    <div
      class="absolute top-0 right-0 w-96 h-96 bg-white/5 rounded-full blur-3xl"
    ></div>
    <div
      class="absolute bottom-0 left-0 w-96 h-96 bg-white/5 rounded-full blur-3xl"
    ></div>

    <div class="relative z-10 max-w-6xl mx-auto px-6">
      <div class="mb-20">
        <div class="overflow-hidden mb-4">
          <p
            class="text-sm font-inter text-white/40 tracking-widest uppercase slide-up"
          >
            Get In Touch
          </p>
        </div>
        <div class="overflow-hidden mb-8">
          <h2
            class="text-6xl md:text-9xl font-space font-bold tracking-tighter slide-up-delay-1"
          >
            Let's Work<br />Together
          </h2>
        </div>
        <div class="overflow-hidden max-w-3xl">
          <p
            class="text-xl md:text-2xl text-white/50 font-inter leading-relaxed slide-up-delay-2"
          >
            Available for freelance projects, full-time opportunities, and
            collaborations. Let's build something amazing.
          </p>
        </div>
      </div>

      <div class="flex flex-col md:flex-row gap-4 mb-24 slide-up-delay-3">
        <a
          href="mailto:developersrujan12@gmail.com"
          class="group px-12 py-6 bg-white text-black font-inter font-medium text-lg relative overflow-hidden"
        >
          <span class="relative z-10">Send Email</span>
          <div
            class="absolute inset-0 bg-black translate-x-full group-hover:translate-x-0 transition-transform duration-500"
          ></div>
          <span
            class="absolute inset-0 flex items-center justify-center text-white opacity-0 group-hover:opacity-100 transition-opacity duration-500 z-10"
          >
            Send Email
          </span>
        </a>
        <a
          href="https://linkedin.com/in/srujan-km-12"
          target="_blank"
          rel="noopener noreferrer"
          class="px-12 py-6 border border-white/30 font-inter font-medium text-lg hover:bg-white hover:text-black transition-all duration-500"
        >
          LinkedIn
        </a>
        <a
          href="https://github.com/Srujankm12"
          target="_blank"
          rel="noopener noreferrer"
          class="px-12 py-6 border border-white/30 font-inter font-medium text-lg hover:bg-white hover:text-black transition-all duration-500"
        >
          GitHub
        </a>
      </div>

      <div
        class="grid md:grid-cols-2 gap-16 pt-16 border-t border-white/10 slide-up-delay-4"
      >
        <div class="space-y-6">
          <p
            class="text-sm font-inter text-white/40 tracking-widest uppercase mb-6"
          >
            Contact
          </p>
          <div class="space-y-4 text-white/60 font-inter">
            <a
              href="tel:+918310029635"
              class="block text-2xl hover:text-white transition-colors duration-300 group"
            >
              <span
                class="inline-block group-hover:translate-x-2 transition-transform duration-300"
              >
                +91 8310029635
              </span>
            </a>
            <a
              href="mailto:developersrujan12@gmail.com"
              class="block text-2xl hover:text-white transition-colors duration-300 group"
            >
              <span
                class="inline-block group-hover:translate-x-2 transition-transform duration-300"
              >
                developersrujan12@gmail.com
              </span>
            </a>
          </div>
        </div>
        <div class="space-y-6">
          <p
            class="text-sm font-inter text-white/40 tracking-widest uppercase mb-6"
          >
            Location
          </p>
          <p class="text-white/60 font-inter text-2xl leading-relaxed">
            Bengaluru, Karnataka<br />India
          </p>
        </div>
      </div>

      <!-- Social Links -->
      <div class="mt-24 pt-16 border-t border-white/10">
        <div
          class="flex flex-col md:flex-row justify-between items-start gap-8"
        >
          <div>
            <p
              class="text-sm font-inter text-white/40 tracking-widest uppercase mb-4"
            >
              Connect
            </p>
            <div class="flex flex-wrap gap-6 text-white/40 font-inter">
              <a
                href="https://linkedin.com/in/srujan-km-12"
                target="_blank"
                class="hover:text-white transition-colors duration-300"
              >
                Personal LinkedIn ↗
              </a>
              <a
                href="https://github.com/Srujankm12"
                target="_blank"
                class="hover:text-white transition-colors duration-300"
              >
                GitHub ↗
              </a>
            </div>
          </div>
          <div>
            <p
              class="text-sm font-inter text-white/40 tracking-widest uppercase mb-4"
            >
              Levion Studio
            </p>
            <div class="flex flex-wrap gap-6 text-white/40 font-inter">
              <a
                href="https://www.linkedin.com/company/levion-studio/"
                target="_blank"
                class="hover:text-white transition-colors duration-300"
              >
                Company LinkedIn ↗
              </a>
              <a
                href="https://www.instagram.com/levionstudi0/"
                target="_blank"
                class="hover:text-white transition-colors duration-300"
              >
                Instagram ↗
              </a>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>

  <!-- Footer -->
  <footer class="relative border-t border-white/5 overflow-hidden">
    <!-- Background Effects -->
    <div class="absolute inset-0 grid-background opacity-10"></div>
    <div
      class="absolute top-0 left-1/4 w-96 h-96 bg-white/5 rounded-full blur-3xl"
    ></div>

    <div class="relative z-10">
      <!-- Main Footer Content -->
      <div class="max-w-7xl mx-auto px-6 py-20">
        <div class="grid md:grid-cols-2 lg:grid-cols-4 gap-12 mb-16">
          <!-- Brand Column -->
          <div class="lg:col-span-1">
            <div class="mb-6">
              <h3 class="text-4xl font-space font-bold mb-3 tracking-tight">
                Levion Studio
              </h3>
              <p class="text-white/50 font-inter text-sm leading-relaxed">
                Crafting digital experiences with modern technologies and
                innovative solutions.
              </p>
            </div>
            <div class="flex gap-4">
              <a
                href="https://www.linkedin.com/company/levion-studio/"
                target="_blank"
                rel="noopener noreferrer"
                class="w-12 h-12 border border-white/20 flex items-center justify-center hover:border-white/40 hover:bg-white/5 transition-all duration-300 group"
              >
                <span class="text-lg group-hover:scale-110 transition-transform"
                  >in</span
                >
              </a>
              <a
                href="https://www.instagram.com/levionstudi0/"
                target="_blank"
                rel="noopener noreferrer"
                class="w-12 h-12 border border-white/20 flex items-center justify-center hover:border-white/40 hover:bg-white/5 transition-all duration-300 group"
              >
                <svg
                  class="w-5 h-5 group-hover:scale-110 transition-transform"
                  fill="currentColor"
                  viewBox="0 0 24 24"
                >
                  <path
                    d="M12 2.163c3.204 0 3.584.012 4.85.07 3.252.148 4.771 1.691 4.919 4.919.058 1.265.069 1.645.069 4.849 0 3.205-.012 3.584-.069 4.849-.149 3.225-1.664 4.771-4.919 4.919-1.266.058-1.644.07-4.85.07-3.204 0-3.584-.012-4.849-.07-3.26-.149-4.771-1.699-4.919-4.92-.058-1.265-.07-1.644-.07-4.849 0-3.204.013-3.583.07-4.849.149-3.227 1.664-4.771 4.919-4.919 1.266-.057 1.645-.069 4.849-.069zM12 0C8.741 0 8.333.014 7.053.072 2.695.272.273 2.69.073 7.052.014 8.333 0 8.741 0 12c0 3.259.014 3.668.072 4.948.2 4.358 2.618 6.78 6.98 6.98C8.333 23.986 8.741 24 12 24c3.259 0 3.668-.014 4.948-.072 4.354-.2 6.782-2.618 6.979-6.98.059-1.28.073-1.689.073-4.948 0-3.259-.014-3.667-.072-4.947-.196-4.354-2.617-6.78-6.979-6.98C15.668.014 15.259 0 12 0zm0 5.838a6.162 6.162 0 100 12.324 6.162 6.162 0 000-12.324zM12 16a4 4 0 110-8 4 4 0 010 8zm6.406-11.845a1.44 1.44 0 100 2.881 1.44 1.44 0 000-2.881z"
                  />
                </svg>
              </a>
              <a
                href="https://github.com/Srujankm12"
                target="_blank"
                rel="noopener noreferrer"
                class="w-12 h-12 border border-white/20 flex items-center justify-center hover:border-white/40 hover:bg-white/5 transition-all duration-300 group"
              >
                <svg
                  class="w-5 h-5 group-hover:scale-110 transition-transform"
                  fill="currentColor"
                  viewBox="0 0 24 24"
                >
                  <path
                    d="M12 0c-6.626 0-12 5.373-12 12 0 5.302 3.438 9.8 8.207 11.387.599.111.793-.261.793-.577v-2.234c-3.338.726-4.033-1.416-4.033-1.416-.546-1.387-1.333-1.756-1.333-1.756-1.089-.745.083-.729.083-.729 1.205.084 1.839 1.237 1.839 1.237 1.07 1.834 2.807 1.304 3.492.997.107-.775.418-1.305.762-1.604-2.665-.305-5.467-1.334-5.467-5.931 0-1.311.469-2.381 1.236-3.221-.124-.303-.535-1.524.117-3.176 0 0 1.008-.322 3.301 1.23.957-.266 1.983-.399 3.003-.404 1.02.005 2.047.138 3.006.404 2.291-1.552 3.297-1.23 3.297-1.23.653 1.653.242 2.874.118 3.176.77.84 1.235 1.911 1.235 3.221 0 4.609-2.807 5.624-5.479 5.921.43.372.823 1.102.823 2.222v3.293c0 .319.192.694.801.576 4.765-1.589 8.199-6.086 8.199-11.386 0-6.627-5.373-12-12-12z"
                  />
                </svg>
              </a>
            </div>
          </div>

          <!-- Quick Links -->
          <div>
            <h4
              class="text-sm font-inter text-white/40 tracking-widest uppercase mb-6"
            >
              Navigation
            </h4>
            <ul class="space-y-3">
              <li>
                <a
                  href="#hero"
                  class="text-white/60 hover:text-white font-inter transition-colors duration-300 flex items-center gap-2 group"
                >
                  <span
                    class="opacity-0 group-hover:opacity-100 transition-opacity"
                    >→</span
                  >
                  <span>Home</span>
                </a>
              </li>
              <li>
                <a
                  href="#work"
                  class="text-white/60 hover:text-white font-inter transition-colors duration-300 flex items-center gap-2 group"
                >
                  <span
                    class="opacity-0 group-hover:opacity-100 transition-opacity"
                    >→</span
                  >
                  <span>Projects</span>
                </a>
              </li>
              <li>
                <a
                  href="#about"
                  class="text-white/60 hover:text-white font-inter transition-colors duration-300 flex items-center gap-2 group"
                >
                  <span
                    class="opacity-0 group-hover:opacity-100 transition-opacity"
                    >→</span
                  >
                  <span>Experience</span>
                </a>
              </li>
              <li>
                <a
                  href="#contact"
                  class="text-white/60 hover:text-white font-inter transition-colors duration-300 flex items-center gap-2 group"
                >
                  <span
                    class="opacity-0 group-hover:opacity-100 transition-opacity"
                    >→</span
                  >
                  <span>Contact</span>
                </a>
              </li>
            </ul>
          </div>

          <!-- Services -->
          <div>
            <h4
              class="text-sm font-inter text-white/40 tracking-widest uppercase mb-6"
            >
              Services
            </h4>
            <ul class="space-y-3 text-white/60 font-inter text-sm">
              <li>Web Development</li>
              <li>Backend Systems</li>
              <li>API Development</li>
              <li>Cloud Solutions</li>
              <li>Mobile Apps</li>
              <li>UI/UX Design</li>
            </ul>
          </div>

          <!-- Contact Info -->
          <div>
            <h4
              class="text-sm font-inter text-white/40 tracking-widest uppercase mb-6"
            >
              Get In Touch
            </h4>
            <ul class="space-y-4">
              <li>
                <a
                  href="mailto:developersrujan12@gmail.com"
                  class="text-white/60 hover:text-white font-inter text-sm transition-colors duration-300 block"
                >
                  developersrujan12@gmail.com
                </a>
              </li>
              <li>
                <a
                  href="tel:+918310029635"
                  class="text-white/60 hover:text-white font-inter text-sm transition-colors duration-300 block"
                >
                  +91 8310029635
                </a>
              </li>
              <li class="text-white/40 font-inter text-sm">
                Bengaluru, Karnataka<br />India
              </li>
            </ul>
          </div>
        </div>

        <!-- Divider -->
        <div class="w-full h-px bg-white/5 mb-12"></div>

        <!-- Bottom Bar -->
        <div
          class="flex flex-col md:flex-row justify-between items-center gap-6"
        >
          <div
            class="flex flex-col md:flex-row items-center gap-6 text-sm text-white/30 font-inter"
          >
            <p>© 2025 Srujan KM</p>
            <span class="hidden md:block">•</span>
            <p>Levion Studio</p>
            <span class="hidden md:block">•</span>
            <p>All rights reserved</p>
          </div>

          <div class="flex items-center gap-6 text-sm text-white/30 font-inter">
            <a
              href="https://linkedin.com/in/srujan-km-12"
              target="_blank"
              class="hover:text-white transition-colors duration-300"
            >
              Personal
            </a>
            <span>•</span>
            <span>Made with ❤️ in India</span>
          </div>
        </div>
      </div>

      <!-- Scroll to Top Button -->
      <div class="absolute bottom-8 right-8">
        <button
          on:click={() => window.scrollTo({ top: 0, behavior: "smooth" })}
          class="w-12 h-12 border border-white/20 flex items-center justify-center hover:border-white/40 hover:bg-white/5 transition-all duration-300 group"
        >
          <span class="text-lg group-hover:-translate-y-1 transition-transform"
            >↑</span
          >
        </button>
      </div>
    </div>
  </footer>
</div>

<style>
  .font-space {
    font-family: "Space Grotesk", sans-serif;
  }

  .font-inter {
    font-family: "Inter", sans-serif;
  }

  :global(html) {
    scroll-behavior: smooth;
  }

  /* Custom Cursor */
  .custom-cursor {
    position: fixed;
    width: 20px;
    height: 20px;
    border: 2px solid white;
    border-radius: 50%;
    pointer-events: none;
    z-index: 9999;
    mix-blend-mode: difference;
    transition:
      width 0.3s,
      height 0.3s;
  }

  .cursor-hovering {
    width: 40px;
    height: 40px;
  }

  /* Grid Background */
  .grid-background {
    background-image:
      linear-gradient(rgba(255, 255, 255, 0.03) 1px, transparent 1px),
      linear-gradient(90deg, rgba(255, 255, 255, 0.03) 1px, transparent 1px);
    background-size: 50px 50px;
    animation: gridMove 20s linear infinite;
  }

  @keyframes gridMove {
    0% {
      background-position: 0 0;
    }
    100% {
      background-position: 50px 50px;
    }
  }

  /* Gradient Orbs */
  .orb {
    position: absolute;
    border-radius: 50%;
    filter: blur(80px);
    opacity: 0.15;
    animation: float 20s ease-in-out infinite;
  }

  .orb-1 {
    width: 500px;
    height: 500px;
    background: radial-gradient(circle, white, transparent);
    top: 10%;
    left: 10%;
    animation-delay: 0s;
  }

  .orb-2 {
    width: 400px;
    height: 400px;
    background: radial-gradient(circle, white, transparent);
    bottom: 10%;
    right: 10%;
    animation-delay: -10s;
  }

  .orb-3 {
    width: 450px;
    height: 450px;
    background: radial-gradient(circle, white, transparent);
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    animation-delay: -5s;
  }

  @keyframes float {
    0%,
    100% {
      transform: translate(0, 0) scale(1);
    }
    33% {
      transform: translate(50px, -50px) scale(1.1);
    }
    66% {
      transform: translate(-50px, 50px) scale(0.9);
    }
  }

  /* Typing Animation */
  .typing-text {
    color: white;
  }

  .typing-cursor {
    animation: blink 1s step-end infinite;
  }

  @keyframes blink {
    0%,
    50% {
      opacity: 1;
    }
    51%,
    100% {
      opacity: 0;
    }
  }

  /* Slide up animations */
  .slide-up {
    animation: slideUp 0.8s ease-out;
  }

  .slide-up-delay-1 {
    animation: slideUp 0.8s ease-out 0.1s both;
  }

  .slide-up-delay-2 {
    animation: slideUp 0.8s ease-out 0.2s both;
  }

  .slide-up-delay-3 {
    animation: slideUp 0.8s ease-out 0.3s both;
  }

  .slide-up-delay-4 {
    animation: slideUp 0.8s ease-out 0.4s both;
  }

  .slide-up-delay-5 {
    animation: slideUp 0.8s ease-out 0.5s both;
  }

  @keyframes slideUp {
    from {
      opacity: 0;
      transform: translateY(30px);
    }
    to {
      opacity: 1;
      transform: translateY(0);
    }
  }

  /* Scroll Indicator */
  .scroll-indicator {
    width: 2px;
    height: 60px;
    background: rgba(255, 255, 255, 0.1);
    position: relative;
    margin: 0 auto;
  }

  .scroll-line {
    width: 100%;
    height: 20px;
    background: white;
    position: absolute;
    top: 0;
    animation: scrollDown 2s ease-in-out infinite;
  }

  @keyframes scrollDown {
    0% {
      top: 0;
      opacity: 1;
    }
    100% {
      top: 40px;
      opacity: 0;
    }
  }

  /* Scrollbar */
  .scrollbar-thin::-webkit-scrollbar {
    height: 6px;
  }

  .scrollbar-thin::-webkit-scrollbar-track {
    background: rgba(255, 255, 255, 0.05);
  }

  .scrollbar-thin::-webkit-scrollbar-thumb {
    background: rgba(255, 255, 255, 0.2);
    border-radius: 3px;
  }

  /* Project Cards */
  .project-card {
    animation: fadeInUp 0.6s ease-out both;
  }

  @keyframes fadeInUp {
    from {
      opacity: 0;
      transform: translateY(20px);
    }
    to {
      opacity: 1;
      transform: translateY(0);
    }
  }

  /* Experience Items */
  .experience-item {
    animation: fadeInLeft 0.8s ease-out both;
  }

  @keyframes fadeInLeft {
    from {
      opacity: 0;
      transform: translateX(-20px);
    }
    to {
      opacity: 1;
      transform: translateX(0);
    }
  }

  /* Marquee */
  .marquee-wrapper,
  .marquee-wrapper-reverse {
    overflow: hidden;
    white-space: nowrap;
    position: relative;
  }

  .marquee,
  .marquee-reverse {
    display: inline-flex;
    gap: 1.5rem;
    animation: scroll-left 45s linear infinite;
  }

  .marquee-reverse {
    animation: scroll-right 45s linear infinite;
  }

  @keyframes scroll-left {
    0% {
      transform: translateX(0);
    }
    100% {
      transform: translateX(-50%);
    }
  }

  @keyframes scroll-right {
    0% {
      transform: translateX(-50%);
    }
    100% {
      transform: translateX(0);
    }
  }

  .tech-tag {
    display: inline-block;
    padding: 1.25rem 2.5rem;
    border: 1px solid rgba(255, 255, 255, 0.1);
    font-family: "Inter", sans-serif;
    font-size: 0.875rem;
    font-weight: 500;
    white-space: nowrap;
    transition: all 0.3s ease;
  }

  .tech-tag:hover {
    border-color: rgba(255, 255, 255, 0.3);
    background: rgba(255, 255, 255, 0.05);
  }

  .tech-tag-logo {
    display: inline-flex;
    align-items: center;
    gap: 1rem;
    padding: 1.25rem 2.5rem;
    border: 1px solid rgba(255, 255, 255, 0.1);
    font-family: "Inter", sans-serif;
    font-size: 0.875rem;
    font-weight: 500;
    white-space: nowrap;
    transition: all 0.3s ease;
    background: rgba(0, 0, 0, 0.3);
    backdrop-filter: blur(10px);
  }

  .tech-tag-logo:hover {
    border-color: rgba(255, 255, 255, 0.3);
    background: rgba(255, 255, 255, 0.05);
    transform: translateY(-2px);
  }

  .tech-logo {
    width: 24px;
    height: 24px;
    object-fit: contain;
    filter: brightness(0) invert(1);
  }

  .tech-tag-logo:hover .tech-logo {
    filter: none;
  }

  .marquee-wrapper:hover .marquee,
  .marquee-wrapper-reverse:hover .marquee-reverse {
    animation-play-state: paused;
  }

  /* Video */
  video {
    transition: opacity 1s ease-in-out;
  }
</style>
