<script lang="ts">
	import { onMount } from 'svelte';

	let activeSection = $state('about');
	let scrollY = $state(0);
	let currentQuoteIndex = $state(0);

	const quotes = [
		"Building scalable systems that matter",
		"Code is poetry written for humans, not just machines",
		"Every problem is an opportunity to learn"
	];

	const projects = [
		{
			title: 'Telephone Recharge Machine',
			description: 'Serverless backend with Golang and AWS Lambda. Multi-role APIs, secure authentication, and async email queues.',
			tags: ['Golang', 'AWS Lambda', 'MongoDB', 'RabbitMQ']
		},
		{
			title: 'VoiceVerse AI Assistant',
			description: 'Voice assistant with Whisper AI and LLaMA 3.2. RAG-powered responses with Qdrant vector search.',
			tags: ['Whisper AI', 'LLaMA 3.2', 'RAG', 'Qdrant']
		},
		{
			title: 'PayBazaar Fintech',
			description: 'Multi-role fintech platform with role-based dashboards. DMT transfers, KYC verification, banking APIs.',
			tags: ['Next.js', 'Golang', 'PostgreSQL', 'Fintech']
		}
	];

	const skills = [
		{ category: 'Backend', items: ['Golang', 'Node.js', 'Express', 'AWS Lambda', 'PostgreSQL', 'MongoDB'] },
		{ category: 'Frontend', items: ['React', 'Next.js', 'SvelteKit', 'TypeScript', 'Tailwind CSS'] },
		{ category: 'DevOps', items: ['AWS', 'Docker', 'Vercel', 'Render', 'Serverless'] },
		{ category: 'Mobile', items: ['Flutter', 'Dart', 'React Native'] },
		{ category: 'AI/ML', items: ['LLaMA', 'Whisper AI', 'RAG', 'Qdrant'] },
		{ category: 'Tools', items: ['Git', 'GitHub', 'VS Code', 'Postman'] }
	];

	onMount(() => {
		// Rotate quotes every 3 seconds
		const quoteInterval = setInterval(() => {
			currentQuoteIndex = (currentQuoteIndex + 1) % quotes.length;
		}, 3000);

		// Smooth scroll navigation
		const links = document.querySelectorAll('a[href^="#"]');
		links.forEach(link => {
			link.addEventListener('click', (e) => {
				const href = (e.target as HTMLElement).closest('a')?.getAttribute('href');
				if (href && href.startsWith('#')) {
					e.preventDefault();
					const section = document.querySelector(href);
					if (section) {
						section.scrollIntoView({ behavior: 'smooth' });
						activeSection = href.slice(1);
					}
				}
			});
		});

		// Track active section
		window.addEventListener('scroll', () => {
			scrollY = window.scrollY;
			const sections = ['about', 'experience', 'projects', 'skills', 'contact'];
			for (const sectionId of sections) {
				const element = document.getElementById(sectionId);
				if (element) {
					const rect = element.getBoundingClientRect();
					if (rect.top <= 200 && rect.bottom > 200) {
						activeSection = sectionId;
						break;
					}
				}
			}
		});

		return () => {
			clearInterval(quoteInterval);
		};
	});
</script>

<svelte:window bind:scrollY />

<div class="min-h-screen bg-black text-white">
	<!-- Navbar -->
	<nav class="fixed top-0 w-full z-50 bg-black/95 backdrop-blur-md border-b border-white/10">
		<div class="max-w-6xl mx-auto px-6 py-4 flex justify-between items-center">
			<div class="font-serif text-xl font-bold">SK</div>
			<div class="hidden md:flex gap-8 text-sm">
				<a href="#about" class="hover:text-white/60 transition-colors {activeSection === 'about' ? 'text-white font-semibold' : 'text-white/60'}">About</a>
				<a href="#experience" class="hover:text-white/60 transition-colors {activeSection === 'experience' ? 'text-white font-semibold' : 'text-white/60'}">Experience</a>
				<a href="#projects" class="hover:text-white/60 transition-colors {activeSection === 'projects' ? 'text-white font-semibold' : 'text-white/60'}">Projects</a>
				<a href="#skills" class="hover:text-white/60 transition-colors {activeSection === 'skills' ? 'text-white font-semibold' : 'text-white/60'}">Skills</a>
				<a href="#contact" class="hover:text-white/60 transition-colors {activeSection === 'contact' ? 'text-white font-semibold' : 'text-white/60'}">Contact</a>
			</div>
			<a href="#contact" class="text-sm font-medium border border-white/30 px-4 py-2 rounded hover:bg-white hover:text-black transition-colors">Contact</a>
		</div>
	</nav>

	<!-- Hero Section -->
	<section id="about" class="relative min-h-screen flex items-center pt-24">
		<div class="max-w-6xl mx-auto px-6 py-20 w-full">
			<div class="max-w-4xl">
				<!-- Static welcome text, one line, no typing animation -->
				<h1 class="text-6xl md:text-7xl font-serif font-bold leading-tight mb-12">
					Welcome to my portfolio
				</h1>

				<p class="text-lg text-white/70 mb-12 leading-relaxed max-w-2xl">
					Full-stack developer and founder of Levion Studio. Specializing in serverless architectures, scalable backends, and modern web applications.
				</p>

				<!-- Rotating quote -->
				<div class="mb-16 border-l border-white/30 pl-6">
					<p class="text-lg italic text-white/60">"{quotes[currentQuoteIndex]}"</p>
				</div>

				<div class="flex gap-4">
					<a href="#contact" class="px-8 py-3 bg-white text-black font-medium rounded hover:bg-white/90 transition-colors">
						Get in touch
					</a>
					<a href="#projects" class="px-8 py-3 border border-white/30 text-white font-medium rounded hover:bg-white hover:text-black transition-colors">
						View work
					</a>
				</div>
			</div>
		</div>
	</section>

	<!-- Experience Section -->
	<section id="experience" class="relative py-40 border-t border-white/10">
		<div class="max-w-6xl mx-auto px-6">
			<h2 class="text-5xl font-serif font-bold mb-20">Experience</h2>
			
			<div class="space-y-12">
				<div class="pb-12 border-b border-white/10">
					<div class="flex justify-between items-start mb-4">
						<div>
							<h3 class="text-2xl font-serif font-bold mb-1">Vithsutra Technologies</h3>
							<p class="text-white/60">Software Developer Intern</p>
						</div>
						<p class="text-white/60 text-sm whitespace-nowrap">Jun 2025</p>
					</div>
					<p class="text-white/60 mb-4 text-sm">Moodbidri, Karnataka</p>
					<ul class="space-y-2 text-white/70">
						<li>• Engineered serverless backend with Golang (Echo) and AWS Lambda, leveraging MongoDB</li>
						<li>• Developed biometric system with Flutter dashboard for real-time analytics</li>
						<li>• Integrated RabbitMQ and Resend API for async workflows</li>
					</ul>
				</div>

				<div>
					<div class="flex justify-between items-start mb-4">
						<div>
							<h3 class="text-2xl font-serif font-bold mb-1">SR Automation</h3>
							<p class="text-white/60">Full Stack Developer Intern</p>
						</div>
						<p class="text-white/60 text-sm whitespace-nowrap">Dec 2024</p>
					</div>
					<p class="text-white/60 mb-4 text-sm">Bangalore, Karnataka</p>
					<ul class="space-y-2 text-white/70">
						<li>• Designed full-stack dashboard with SvelteKit and PostgreSQL</li>
						<li>• Developed Go backend with optimized REST APIs</li>
						<li>• Deployed infrastructure on Vercel and Render</li>
					</ul>
				</div>
			</div>
		</div>
	</section>

	<!-- Projects Section -->
	<section id="projects" class="relative py-40 border-t border-white/10">
		<div class="max-w-6xl mx-auto px-6">
			<h2 class="text-5xl font-serif font-bold mb-20">Projects</h2>
			
			<!-- Projects in horizontal row with cards -->
			<div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
				{#each projects as project (project.title)}
					<div class="border border-white/10 rounded p-8 hover:border-white/30 hover:bg-white/5 transition-all duration-300">
						<h3 class="text-xl font-serif font-bold mb-3">{project.title}</h3>
						<p class="text-white/60 mb-6 text-sm leading-relaxed">{project.description}</p>
						<div class="flex flex-wrap gap-2">
							{#each project.tags as tag}
								<span class="text-xs font-medium px-2 py-1 bg-white/10 border border-white/20 rounded">{tag}</span>
							{/each}
						</div>
					</div>
				{/each}
			</div>
		</div>
	</section>

	<!-- Skills Section -->
	<section id="skills" class="relative py-40 border-t border-white/10">
		<div class="max-w-6xl mx-auto px-6">
			<h2 class="text-5xl font-serif font-bold mb-20">Skills</h2>
			
			<div class="grid md:grid-cols-2 lg:grid-cols-3 gap-8">
				{#each skills as skillGroup (skillGroup.category)}
					<div>
						<h3 class="text-lg font-serif font-bold mb-4">{skillGroup.category}</h3>
						<div class="flex flex-wrap gap-2">
							{#each skillGroup.items as skill (skill)}
								<span class="text-sm px-3 py-1 bg-white/5 border border-white/10 rounded">{skill}</span>
							{/each}
						</div>
					</div>
				{/each}
			</div>
		</div>
	</section>

	<!-- Contact Section -->
	<section id="contact" class="relative py-40 border-t border-white/10">
		<div class="max-w-4xl mx-auto px-6 text-center">
			<h2 class="text-5xl font-serif font-bold mb-8">Get in touch</h2>
			<p class="text-lg text-white/60 mb-16">
				Let's collaborate on your next project.
			</p>

			<!-- Direct working links -->
			<div class="flex flex-col md:flex-row justify-center gap-4 mb-12">
				<a href="mailto:developersrujan12@gmail.com" class="px-8 py-3 bg-white text-black font-medium rounded hover:bg-white/90 transition-colors">
					Send email
				</a>
				<a href="https://linkedin.com/in/srujan-km-12" target="_blank" rel="noopener noreferrer" class="px-8 py-3 border border-white/30 text-white font-medium rounded hover:bg-white hover:text-black transition-colors">
					LinkedIn
				</a>
				<a href="https://github.com/Srujankm12" target="_blank" rel="noopener noreferrer" class="px-8 py-3 border border-white/30 text-white font-medium rounded hover:bg-white hover:text-black transition-colors">
					GitHub
				</a>
			</div>

			<div class="flex flex-col md:flex-row justify-center gap-4 text-white/60">
				<a href="tel:+918310029635" class="hover:text-white transition-colors">+91 8310029635</a>
				<span class="hidden md:inline">•</span>
				<a href="mailto:developersrujan12@gmail.com" class="hover:text-white transition-colors">developersrujan12@gmail.com</a>
			</div>
		</div>
	</section>

	<!-- Footer -->
	<footer class="py-8 border-t border-white/10 text-center text-white/60 text-sm">
		<p>© 2025 Srujan KM • Founder @ Levion Studio</p>
	</footer>
</div>

<style>
	:global(body) {
		scroll-behavior: smooth;
	}
</style>
