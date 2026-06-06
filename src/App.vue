<script setup>
import { ref, onMounted, onUnmounted, computed, watch } from 'vue'

// Active section for navigation highlighters
const activeSection = ref('home')

// Mobile menu active state
const isMobileMenuOpen = ref(false)

watch(isMobileMenuOpen, (isOpen) => {
  if (isOpen) {
    document.body.classList.add('overflow-hidden')
  } else {
    document.body.classList.remove('overflow-hidden')
  }
})

// Nav links definition
const navLinks = [
  { id: 'home', label: 'About' },
  { id: 'projects', label: 'Projects' },
  { id: 'skills', label: 'Skills' },
  { id: 'experience', label: 'Experience' },
  { id: 'contact', label: 'Contact' }
]

// Projects data
const projects = [
  {
    title: 'BioID: Biometric Accident Response System',
    description: 'A cross-platform biometric recognition system designed for emergency first responders. Interfaces with hardware USB fingerprint sensors to rapidly identify accident victims, retrieve critical medical records, and notify emergency contacts. Syncs seamlessly via Supabase and local SQLite storage.',
    tech: ['Electron', 'Capacitor', 'Supabase', 'SQLite3', 'Express', 'USB Serial API'],
    category: 'apps',
    link: 'https://bio-id-biometric-recognition-system.vercel.app/',
    github: 'https://github.com/BJManalo/BioID-Biometric-Recognition-System'
  },
  {
    title: 'eBurial: 3D Cemetery Map & Payments',
    description: 'An integrated municipal database and payment portal featuring an interactive 3D cemetery spatial layout rendered in Three.js. Allows administrators and citizens to search graves, track ownership documents, manage burial fees, and dispatch automatic payment reminders via SMS.',
    tech: ['Three.js', 'Firebase', 'Lucide Icons', 'SMS Gateway', 'CSS Variables'],
    category: 'creative',
    link: 'https://eburial-system.web.app/',
    github: 'https://github.com/BJManalo/eburial-system'
  },
  {
    title: 'CCIS Defense Paneling System',
    description: 'A scheduling, allocation, and grading platform custom-built for capstone defense groups at the University of Antique College of Computer and Information Sciences. Leverages Supabase real-time subscriptions, secure grading logs, and installable PWA functionality.',
    tech: ['Supabase Auth & DB', 'PWA (Service Workers)', 'Vanilla JS', 'Tailwind CSS'],
    category: 'utilities',
    link: 'https://ccis-paneling-system.vercel.app/',
    github: 'https://github.com/BJManalo/ccis-paneling-system'
  },
  {
    title: 'Tourist Digital: Logging & Monitoring',
    description: 'An eco-governance registry and ticketing dashboard designed to automate tourist check-ins, local tour guide assignments, and safety certifications for Tibiao Eco-Adventures. Provides real-time capacity monitoring and visitor trend analytics.',
    tech: ['Express.js', 'PostgreSQL', 'SQLite3', 'Vercel Serverless', 'JS Modules'],
    category: 'apps',
    link: 'https://tourist-digital-logging-and-monitor-three.vercel.app/',
    github: 'https://github.com/BJManalo/tibiao-tourism-system'
  }
]

// Active filter state
const activeFilter = ref('all')
const categories = [
  { value: 'all', label: 'All Work' },
  { value: 'apps', label: 'Web Apps' },
  { value: 'creative', label: 'Creative Tech' },
  { value: 'utilities', label: 'Utilities' }
]

// Filtered projects computed
const filteredProjects = computed(() => {
  if (activeFilter.value === 'all') return projects
  return projects.filter(p => p.category === activeFilter.value)
})

// Navbar dynamic styling on scroll
const isScrolled = ref(false)
const handleScroll = () => {
  isScrolled.value = window.scrollY > 40
  
  // Track current visible section
  const scrollPosition = window.scrollY + 200
  for (const link of navLinks) {
    const el = document.getElementById(link.id)
    if (el) {
      const top = el.offsetTop
      const height = el.offsetHeight
      if (scrollPosition >= top && scrollPosition < top + height) {
        activeSection.value = link.id
      }
    }
  }
}

// Contact form fields
const form = ref({ name: '', email: '', message: '' })
const isSubmitting = ref(false)
const submitSuccess = ref(false)

const handleContactSubmit = () => {
  isSubmitting.value = true
  setTimeout(() => {
    isSubmitting.value = false
    submitSuccess.value = true
    form.value = { name: '', email: '', message: '' }
    setTimeout(() => {
      submitSuccess.value = false
    }, 4000)
  }, 1200)
}

onMounted(() => {
  window.addEventListener('scroll', handleScroll)
})

onUnmounted(() => {
  window.removeEventListener('scroll', handleScroll)
})
</script>

<template>
  <div class="min-h-screen font-sans bg-warm-light-bg text-brand-dark relative overflow-hidden selection:bg-brand-orange/20 selection:text-brand-dark">
    <!-- Header / Sticky Navigation -->
    <header 
      class="fixed top-0 left-0 w-full z-50 transition-all duration-300 border-b"
      :class="isScrolled ? 'bg-white/80 backdrop-blur-md border-brand-border/40 py-4' : 'bg-transparent border-transparent py-6'"
    >
      <div class="max-w-6xl mx-auto px-6 md:px-12 flex items-center justify-between">
        <!-- Logo -->
        <a href="#home" class="group flex items-center space-x-2">
            <span class="w-8 h-8 rounded-full border-2 border-brand-dark flex items-center justify-center font-serif font-semibold text-lg transition-transform duration-300 group-hover:rotate-12 group-hover:border-brand-orange group-hover:text-brand-amber">BJ</span>
        </a>

        <!-- Menu Links -->
        <nav class="hidden md:flex items-center space-x-8">
          <a 
            v-for="link in navLinks" 
            :key="link.id" 
            :href="`#${link.id}`"
            class="text-xs tracking-widest uppercase transition-colors duration-200 relative py-1"
            :class="activeSection === link.id ? 'text-brand-amber font-medium' : 'text-brand-muted hover:text-brand-dark'"
          >
            {{ link.label }}
            <!-- Active underline transition -->
            <span 
              class="absolute bottom-0 left-0 w-full h-[1px] bg-gradient-to-r from-brand-orange to-brand-coral scale-x-0 transition-transform duration-300 origin-left"
              :class="{ 'scale-x-100': activeSection === link.id }"
            ></span>
          </a>
        </nav>

        <!-- Right Side Nav: Desktop CTA + Mobile Hamburger -->
        <div class="flex items-center space-x-4">
          <!-- CTA Action -->
          <a 
              href="#contact" 
              class="hidden md:inline-block border border-brand-orange/60 px-4 py-2 text-xs tracking-widest uppercase transition-all duration-300 text-brand-orange hover:bg-gradient-to-r hover:from-brand-orange hover:to-brand-coral hover:text-brand-cream hover:border-transparent hover:shadow-[0_0_15px_rgba(249,115,22,0.3)]"
            >
              Hire Me
            </a>
            <!-- Header profile image -->


          <!-- Hamburger Button (Mobile Only) -->
          <button 
            @click="isMobileMenuOpen = !isMobileMenuOpen" 
            class="md:hidden relative w-6 h-5 flex flex-col justify-between z-50 focus:outline-none"
            aria-label="Toggle Menu"
          >
            <span 
              class="w-full h-[2px] bg-brand-dark transition-all duration-300"
              :class="{ 'rotate-45 translate-y-[9px]': isMobileMenuOpen }"
            ></span>
            <span 
              class="w-full h-[2px] bg-brand-dark transition-all duration-300"
              :class="{ 'opacity-0 scale-x-0': isMobileMenuOpen }"
            ></span>
            <span 
              class="w-full h-[2px] bg-brand-dark transition-all duration-300"
              :class="{ '-rotate-45 -translate-y-[9px]': isMobileMenuOpen }"
            ></span>
          </button>
        </div>
      </div>
    </header>

    <!-- Mobile Menu Overlay -->
    <transition
      enter-active-class="transition-all duration-300 ease-out"
      leave-active-class="transition-all duration-200 ease-in"
      enter-from-class="opacity-0 translate-y-[-10px]"
      enter-to-class="opacity-100 translate-y-0"
      leave-from-class="opacity-100 translate-y-0"
      leave-to-class="opacity-0 translate-y-[-10px]"
    >
      <div 
        v-if="isMobileMenuOpen" 
        class="fixed inset-0 z-40 bg-warm-light-bg flex flex-col justify-center px-8"
      >
        <!-- Background Ambient Glow in Overlay -->
        <div class="glow-spot top-1/4 left-1/4"></div>
        <nav class="relative z-10 flex flex-col space-y-8 text-center">
          <a 
            v-for="link in navLinks" 
            :key="link.id" 
            :href="`#${link.id}`"
            @click="isMobileMenuOpen = false"
            class="font-serif text-3xl font-light tracking-wide text-brand-dark hover:text-brand-amber hover:italic hover:font-normal transition-all duration-200"
          >
            {{ link.label }}
          </a>
          <!-- Mobile CTA in Overlay -->
          <a 
            href="#contact" 
            @click="isMobileMenuOpen = false"
            class="mt-4 border border-brand-orange px-6 py-3 text-xs tracking-widest uppercase transition-all duration-300 bg-gradient-to-r from-brand-orange to-brand-coral text-brand-cream hover:opacity-90 max-w-xs mx-auto w-full text-center hover:shadow-[0_0_20px_rgba(249,115,22,0.4)]"
          >
            Hire Me
          </a>
        </nav>
      </div>
    </transition>

    <!-- Main Container -->
    <main class="relative pt-24 max-w-6xl mx-auto px-6 md:px-12 z-10">
      <!-- Glow spots in background -->
      <div class="glow-spot -top-20 -left-20"></div>
      <div class="glow-spot top-1/3 right-10" style="background: radial-gradient(circle, rgba(244, 63, 94, 0.05) 0%, rgba(249, 115, 22, 0.01) 60%, rgba(0,0,0,0) 100%);"></div>

      <!-- Section 1: Hero / About -->
      <section id="home" class="relative min-h-[85vh] flex flex-col justify-center py-12 md:py-20 border-b border-brand-border/40">
        <div class="grid grid-cols-1 lg:grid-cols-12 gap-12 lg:gap-16 items-start fade-in-up">
          <!-- Text Content -->
          <div class="lg:col-span-7 order-2 lg:order-1 relative z-10">
            <!-- Small Label -->
            <div class="inline-flex items-center space-x-2 bg-brand-orange/10 border border-brand-orange/20 px-3 py-1 rounded-full text-xs font-medium text-brand-amber mb-6">
              <span class="w-2 h-2 rounded-full bg-brand-coral animate-pulse"></span>
              <span>Available for New Projects</span>
            </div>

            <!-- Headline -->
            <h1 class="font-serif text-4xl sm:text-5xl md:text-6xl lg:text-7xl font-light leading-tight tracking-tight text-brand-dark mb-8">
              Building complete, <br class="hidden sm:inline" />
              <span class="italic font-normal bg-gradient-to-r from-brand-amber via-brand-orange to-brand-coral bg-clip-text text-transparent">scalable systems</span> <br class="hidden sm:inline" />
              and interfaces.
            </h1>

            <!-- Intro Paragraph -->
            <p class="text-base sm:text-lg text-brand-dark/80 leading-relaxed max-w-2xl mb-10">
              Hi, I'm <strong class="text-brand-dark font-semibold">Bernie Jay Manalo</strong>. 
              I'm a Full-Stack Developer focused on building clean, scalable, and high-performance web applications. 
              By combining robust backend systems, detailed design systems, and modern frameworks like Vue.js, 
              I construct end-to-end digital experiences that are highly performant and accessible.
            </p>

            <!-- Action Buttons -->
            <div class="flex flex-wrap items-center gap-4">
              <a 
                href="#projects" 
                class="bg-gradient-to-r from-brand-orange to-brand-coral text-brand-cream px-6 py-3.5 text-xs tracking-widest uppercase font-semibold transition-all duration-300 hover:shadow-[0_0_20px_rgba(249,115,22,0.35)] hover:scale-[1.02]"
              >
                Explore Selected Work
              </a>
              <a 
                href="#contact" 
                class="border border-brand-border bg-white text-brand-dark px-6 py-3.5 text-xs tracking-widest uppercase font-semibold transition-all duration-300 hover:border-brand-orange hover:bg-brand-orange/5 hover:text-brand-orange"
              >
                Get In Touch
              </a>
            </div>
          </div>

          <!-- Image Content -->
          <div class="lg:col-span-5 order-1 lg:order-2 flex justify-center lg:justify-end mb-6 lg:mb-0 lg:pt-14 relative z-10">
            <div class="relative group w-full max-w-[280px] sm:max-w-[320px] aspect-square">
              <!-- Decorative background frame -->
              <div class="absolute inset-0 border-2 border-brand-orange translate-x-3 translate-y-3 transition-transform duration-300 group-hover:translate-x-1.5 group-hover:translate-y-1.5"></div>
              
              <!-- Image container -->
              <div class="relative w-full h-full bg-white border border-brand-border p-2.5 overflow-hidden shadow-md">
                <img 
                  src="/profile.jpg" 
                  alt="Bernie Jay Manalo" 
                  class="w-full h-full object-cover transition-transform duration-500 group-hover:scale-105"
                />
              </div>

              <!-- Status Badge -->
              <div class="absolute -bottom-3 -left-3 bg-white border border-brand-border px-3 py-1.5 shadow-sm flex items-center space-x-2">
                <span class="w-1.5 h-1.5 rounded-full bg-brand-coral animate-pulse"></span>
                <span class="text-[9px] font-mono uppercase tracking-wider text-brand-dark font-semibold">BJ MANALO / DEV</span>
              </div>
            </div>
          </div>
        </div>
      </section>

      <!-- Section 2: Selected Projects -->
      <section id="projects" class="relative py-20 md:py-28 border-b border-brand-border/40">
        <div class="glow-spot top-1/2 left-1/3" style="background: radial-gradient(circle, rgba(245, 158, 11, 0.04) 0%, rgba(0,0,0,0) 70%);"></div>
        <div class="relative z-10 flex flex-col md:flex-row md:items-end justify-between mb-16 gap-6">
          <div>
            <span class="text-xs tracking-widest uppercase text-brand-orange block mb-3 font-semibold">Selected Showcase</span>
            <h2 class="font-serif text-3xl md:text-4xl font-light text-brand-dark">Featured Projects</h2>
          </div>
          
          <!-- Category Filters -->
          <div class="flex flex-wrap gap-2">
            <button 
              v-for="cat in categories" 
              :key="cat.value"
              @click="activeFilter = cat.value"
              class="px-4 py-2 text-xs tracking-wider transition-all duration-200 border rounded-none"
              :class="activeFilter === cat.value 
                ? 'bg-gradient-to-r from-brand-orange to-brand-coral border-transparent text-brand-cream font-semibold shadow-md' 
                : 'border-brand-border bg-white hover:border-brand-orange text-brand-dark/80 hover:text-brand-dark'"
            >
              {{ cat.label }}
            </button>
          </div>
        </div>

        <!-- Projects Grid -->
        <div class="relative z-10 grid grid-cols-1 md:grid-cols-2 gap-8">
          <div 
            v-for="project in filteredProjects" 
            :key="project.title"
            class="group bg-white border border-brand-border/60 p-6 sm:p-8 md:p-10 flex flex-col justify-between transition-all duration-300 hover:border-brand-orange/40 hover:bg-white hover:shadow-[0_10px_30px_rgba(249,115,22,0.03)] hover:scale-[1.01]"
          >
            <div>
              <!-- Tags -->
              <div class="flex flex-wrap gap-2 mb-6">
                <span 
                  v-for="tech in project.tech" 
                  :key="tech"
                  class="bg-brand-orange/5 border border-brand-orange/15 text-[10px] tracking-wider uppercase text-brand-amber px-2.5 py-0.5 rounded-none"
                >
                  {{ tech }}
                </span>
              </div>

              <!-- Title -->
              <h3 class="font-serif text-2xl font-normal text-brand-dark group-hover:text-brand-amber mb-4 transition-colors duration-200">
                {{ project.title }}
              </h3>

              <!-- Description -->
              <p class="text-sm text-brand-dark/75 leading-relaxed mb-8">
                {{ project.description }}
              </p>
            </div>

            <!-- Links -->
            <div class="flex items-center border-t border-brand-border/40 pt-6 mt-4">
              <!-- Demo Link -->
              <a 
                :href="project.link" 
                class="inline-flex items-center text-xs tracking-wider uppercase text-brand-dark font-semibold group/link hover:text-brand-orange transition-colors duration-200"
              >
                <span>Live Site</span>
                <svg xmlns="http://www.w3.org/2000/svg" class="h-3.5 w-3.5 ml-1.5 transition-transform duration-300 group-hover/link:translate-x-1 stroke-brand-dark group-hover/link:stroke-brand-orange" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                  <path stroke-linecap="round" stroke-linejoin="round" d="M14 5l7 7m0 0l-7 7m7-7H3" />
                </svg>
              </a>
            </div>
          </div>
        </div>
      </section>

      <!-- Section 3: Skills & Expertise -->
      <section id="skills" class="relative py-20 md:py-28 border-b border-brand-border/40">
        <div class="glow-spot top-1/2 right-1/4" style="background: radial-gradient(circle, rgba(244, 63, 94, 0.04) 0%, rgba(0,0,0,0) 65%);"></div>
        <div class="relative z-10 mb-16">
          <span class="text-xs tracking-widest uppercase text-brand-orange block mb-3 font-semibold">My Toolkit</span>
          <h2 class="font-serif text-3xl md:text-4xl font-light text-brand-dark">Capabilities & Expertise</h2>
        </div>

        <div class="relative z-10 grid grid-cols-1 md:grid-cols-3 gap-12">
          <!-- Skill category 1 -->
          <div>
            <div class="flex items-center space-x-3 mb-6 pb-2 border-b border-brand-border">
              <span class="font-serif text-sm italic font-semibold text-brand-orange">01.</span>
              <h3 class="text-xs tracking-widest uppercase font-semibold text-brand-dark">Core Engineering</h3>
            </div>
            <ul class="space-y-4">
              <li class="flex flex-row items-baseline justify-between gap-4 text-sm">
                <span class="text-brand-dark/80">Vue.js / Vuex / Pinia</span>
                <span class="text-brand-amber font-mono text-[11px] shrink-0">Advanced</span>
              </li>
              <li class="flex flex-row items-baseline justify-between gap-4 text-sm">
                <span class="text-brand-dark/80">JavaScript (ES6+) & TypeScript</span>
                <span class="text-brand-amber font-mono text-[11px] shrink-0">Proficient</span>
              </li>
              <li class="flex flex-row items-baseline justify-between gap-4 text-sm">
                <span class="text-brand-dark/80">Tailwind CSS v3 & v4</span>
                <span class="text-brand-amber font-mono text-[11px] shrink-0">Advanced</span>
              </li>
              <li class="flex flex-row items-baseline justify-between gap-4 text-sm">
                <span class="text-brand-dark/80">HTML5 / Semantic Layouts</span>
                <span class="text-brand-amber font-mono text-[11px] shrink-0">Expert</span>
              </li>
            </ul>
          </div>

          <!-- Skill category 2 -->
          <div>
            <div class="flex items-center space-x-3 mb-6 pb-2 border-b border-brand-border">
              <span class="font-serif text-sm italic font-semibold text-brand-orange">02.</span>
              <h3 class="text-xs tracking-widest uppercase font-semibold text-brand-dark">Tooling & Bundlers</h3>
            </div>
            <ul class="space-y-4">
              <li class="flex flex-row items-baseline justify-between gap-4 text-sm">
                <span class="text-brand-dark/80">Vite & Rollup</span>
                <span class="text-brand-amber font-mono text-[11px] shrink-0">Highly Proficient</span>
              </li>
              <li class="flex flex-row items-baseline justify-between gap-4 text-sm">
                <span class="text-brand-dark/80">Git / GitHub Workflows</span>
                <span class="text-brand-amber font-mono text-[11px] shrink-0">Advanced</span>
              </li>
              <li class="flex flex-row items-baseline justify-between gap-4 text-sm">
                <span class="text-brand-dark/80">npm / yarn / pnpm</span>
                <span class="text-brand-amber font-mono text-[11px] shrink-0">Experienced</span>
              </li>
              <li class="flex flex-row items-baseline justify-between gap-4 text-sm">
                <span class="text-brand-dark/80">Chrome DevTools & Perf Profiling</span>
                <span class="text-brand-amber font-mono text-[11px] shrink-0">Intermediate</span>
              </li>
            </ul>
          </div>

          <!-- Skill category 3 -->
          <div>
            <div class="flex items-center space-x-3 mb-6 pb-2 border-b border-brand-border">
              <span class="font-serif text-sm italic font-semibold text-brand-orange">03.</span>
              <h3 class="text-xs tracking-widest uppercase font-semibold text-brand-dark">Design & UX</h3>
            </div>
            <ul class="space-y-4">
              <li class="flex flex-row items-baseline justify-between gap-4 text-sm">
                <span class="text-brand-dark/80">User Interface (UI) Design</span>
                <span class="text-brand-amber font-mono text-[11px] shrink-0">Creative</span>
              </li>
              <li class="flex flex-row items-baseline justify-between gap-4 text-sm">
                <span class="text-brand-dark/80">Figma Prototyping</span>
                <span class="text-brand-amber font-mono text-[11px] shrink-0">Intermediate</span>
              </li>
              <li class="flex flex-row items-baseline justify-between gap-4 text-sm">
                <span class="text-brand-dark/80">Typography & Visual Hierarchy</span>
                <span class="text-brand-amber font-mono text-[11px] shrink-0">Refined</span>
              </li>
              <li class="flex flex-row items-baseline justify-between gap-4 text-sm">
                <span class="text-brand-dark/80">Responsive Design Standards</span>
                <span class="text-brand-amber font-mono text-[11px] shrink-0">Expert</span>
              </li>
            </ul>
          </div>
        </div>
      </section>

      <!-- Section 4: Work Experience timeline -->
      <section id="experience" class="relative py-20 md:py-28 border-b border-brand-border/40">
        <div class="glow-spot top-1/4 left-10" style="background: radial-gradient(circle, rgba(249, 115, 22, 0.02) 0%, rgba(0,0,0,0) 60%);"></div>
        <div class="relative z-10 mb-16">
          <span class="text-xs tracking-widest uppercase text-brand-orange block mb-3 font-semibold">Background</span>
          <h2 class="font-serif text-3xl md:text-4xl font-light text-brand-dark">Experience Timeline</h2>
        </div>

        <div class="relative z-10 space-y-12 max-w-3xl">
          <!-- Exp Item 1 -->
          <div class="relative pl-6 sm:pl-8 border-l border-brand-border group">
            <!-- Bullet dot -->
            <div class="absolute top-[9px] -left-[4.5px] w-2 h-2 rounded-full bg-brand-orange group-hover:bg-brand-coral transition-all duration-300 group-hover:scale-150 group-hover:shadow-[0_0_10px_rgba(249,115,22,0.5)]"></div>
            
            <div class="flex flex-col md:flex-row md:items-baseline md:justify-between mb-2 gap-1">
              <h3 class="text-lg font-semibold text-brand-dark">Full-Stack Developer</h3>
              <span class="text-xs text-brand-muted font-mono">Jan 2024 — Present</span>
            </div>
            <p class="text-sm font-serif italic text-brand-amber mb-3">Freelance Studio & Consulting</p>
            <p class="text-sm text-brand-dark/75 leading-relaxed">
              Engineering scalable full-stack applications, robust APIs, and interactive client portals. Standardizing deployment flows and modern web architectures using fast bundlers (Vite), relational databases, and Vue 3 frontends.
            </p>
          </div>

          <!-- Exp Item 2 -->
          <div class="relative pl-6 sm:pl-8 border-l border-brand-border group">
            <div class="absolute top-[9px] -left-[4.5px] w-2 h-2 rounded-full bg-stone-300 group-hover:bg-brand-coral transition-all duration-300 group-hover:scale-150 group-hover:shadow-[0_0_10px_rgba(244,63,94,0.5)]"></div>
            
            <div class="flex flex-col md:flex-row md:items-baseline md:justify-between mb-2 gap-1">
              <h3 class="text-lg font-semibold text-brand-dark">Junior Full-Stack Engineer</h3>
              <span class="text-xs text-brand-muted font-mono">Jul 2022 — Dec 2023</span>
            </div>
            <p class="text-sm font-serif italic text-brand-amber mb-3">Interactive Tech Solutions</p>
            <p class="text-sm text-brand-dark/75 leading-relaxed">
              Designed client dashboards using Vue 3 and integrated headless CMS engines. Improved site speed indices by 35% through bundle splitting, dynamic component loading, and asset tuning.
            </p>
          </div>

          <!-- Exp Item 3 -->
          <div class="relative pl-6 sm:pl-8 border-l border-brand-border group">
            <div class="absolute top-[9px] -left-[4.5px] w-2 h-2 rounded-full bg-stone-300 group-hover:bg-brand-coral transition-all duration-300 group-hover:scale-150 group-hover:shadow-[0_0_10px_rgba(244,63,94,0.5)]"></div>
            
            <div class="flex flex-col md:flex-row md:items-baseline md:justify-between mb-2 gap-1">
              <h3 class="text-lg font-semibold text-brand-dark">Web Development Intern</h3>
              <span class="text-xs text-brand-muted font-mono">Mar 2022 — Jun 2022</span>
            </div>
            <p class="text-sm font-serif italic text-brand-amber mb-3">WebCraft Labs</p>
            <p class="text-sm text-brand-dark/75 leading-relaxed">
              Maintained responsive websites, styled email newsletters, and wrote semantic HTML templates. Gained early experience with client-side reactive models and version control pipelines.
            </p>
          </div>
        </div>
      </section>

      <!-- Section 5: Contact -->
      <section id="contact" class="relative py-20 md:py-28">
        <div class="glow-spot bottom-10 right-10" style="background: radial-gradient(circle, rgba(244, 63, 94, 0.04) 0%, rgba(0,0,0,0) 60%);"></div>
        <div class="relative z-10 grid grid-cols-1 md:grid-cols-2 gap-12 md:gap-16">
          <div>
            <span class="text-xs tracking-widest uppercase text-brand-orange block mb-3 font-semibold">Contact Info</span>
            <h2 class="font-serif text-3xl md:text-4xl font-light mb-8 text-brand-dark">Let's build something exceptional together.</h2>
            <p class="text-sm text-brand-dark/75 leading-relaxed mb-10 max-w-md">
              I am interested in freelance opportunities, contract work, or joining engineering teams that value attention to detail, code architecture, and high-end visual design.
            </p>

            <div class="space-y-6">
              <!-- Email -->
              <div class="flex items-center space-x-4">
                <div class="w-10 h-10 border border-brand-border bg-white flex items-center justify-center">
                  <svg xmlns="http://www.w3.org/2000/svg" class="h-4.5 w-4.5 text-brand-orange" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="1.5">
                    <path stroke-linecap="round" stroke-linejoin="round" d="M3 8l7.89 5.26a2 2 0 002.22 0L21 8M5 19h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v10a2 2 0 002 2z" />
                  </svg>
                </div>
                <div>
                  <span class="text-[10px] tracking-wider uppercase text-brand-muted block">Direct Email</span>
                  <a href="mailto:manalobj19@gmail.com" class="text-sm font-semibold hover:underline text-brand-dark">manalobj19@gmail.com</a>
                </div>
              </div>

              <!-- Location -->
              <div class="flex items-center space-x-4">
                <div class="w-10 h-10 border border-brand-border bg-white flex items-center justify-center">
                  <svg xmlns="http://www.w3.org/2000/svg" class="h-4.5 w-4.5 text-brand-orange" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="1.5">
                    <path stroke-linecap="round" stroke-linejoin="round" d="M17.657 16.657L13.414 20.9a1.998 1.998 0 01-2.827 0l-4.244-4.243a8 8 0 1111.314 0z" />
                    <path stroke-linecap="round" stroke-linejoin="round" d="M15 11a3 3 0 11-6 0 3 3 0 016 0z" />
                  </svg>
                </div>
                <div>
                  <span class="text-[10px] tracking-wider uppercase text-brand-muted block">Location</span>
                  <span class="text-sm font-semibold text-brand-dark">Barbaza, Antique, Philippines</span>
                </div>
              </div>
            </div>
          </div>

          <!-- Contact Form -->
          <div class="bg-white border border-brand-border/60 p-6 sm:p-8 md:p-10 shadow-sm">
            <h3 class="font-serif text-xl mb-6 font-medium text-brand-dark">Send a Message</h3>
            
            <form action="https://api.web3forms.com/submit" method="POST" class="space-y-6">
              <input type="hidden" name="access_key" value="5bd6a01f-6beb-408c-996d-b480c3a0888b" />
              <!-- Name Input -->
              <div>
                <label for="name" class="block text-[10px] tracking-wider uppercase font-semibold text-brand-muted mb-2">Your Name</label>
                <input 
                  id="name"
                  name="name"
                  type="text" 
                  v-model="form.name"
                  required
                  class="w-full border border-brand-border bg-warm-light-bg/50 px-4 py-3 text-sm focus:outline-none focus:border-brand-orange focus:bg-white transition-colors duration-200 text-brand-dark placeholder-brand-muted/50"
                  placeholder="e.g. Jane Doe"
                />
              </div>

              <!-- Email Input -->
              <div>
                <label for="email" class="block text-[10px] tracking-wider uppercase font-semibold text-brand-muted mb-2">Email Address</label>
                <input 
                  id="email"
                  name="email"
                  type="email" 
                  v-model="form.email"
                  required
                  class="w-full border border-brand-border bg-warm-light-bg/50 px-4 py-3 text-sm focus:outline-none focus:border-brand-orange focus:bg-white transition-colors duration-200 text-brand-dark placeholder-brand-muted/50"
                  placeholder="name@company.com"
                />
              </div>

              <!-- Message Input -->
              <div>
                <label for="message" class="block text-[10px] tracking-wider uppercase font-semibold text-brand-muted mb-2">Message</label>
                <textarea 
                  id="message"
                  name="message"
                  v-model="form.message"
                  required
                  rows="4"
                  class="w-full border border-brand-border bg-warm-light-bg/50 px-4 py-3 text-sm focus:outline-none focus:border-brand-orange focus:bg-white transition-colors duration-200 resize-none text-brand-dark placeholder-brand-muted/50"
                  placeholder="Tell me about your project..."
                ></textarea>
              </div>

              <!-- Submit Button -->
              <button 
                type="submit" 
                :disabled="isSubmitting"
                class="w-full bg-gradient-to-r from-brand-orange to-brand-coral text-brand-cream py-3.5 text-xs tracking-widest uppercase font-semibold transition-all duration-300 hover:opacity-90 disabled:opacity-50 hover:shadow-[0_0_15px_rgba(249,115,22,0.3)]"
              >
                <span v-if="isSubmitting">Sending...</span>
                <span v-else>Send Message</span>
              </button>

              <!-- Success Message -->
              <div 
                v-if="submitSuccess"
                class="bg-emerald-50 border border-emerald-200 text-emerald-800 text-xs p-4 text-center transition-all duration-300"
              >
                Thank you! Your message was sent successfully.
              </div>
            </form>
          </div>
        </div>
      </section>
    </main>

    <!-- Footer -->
    <footer class="relative z-10 border-t border-brand-border/40 py-12 mt-12 bg-white">
      <div class="max-w-6xl mx-auto px-6 md:px-12 flex flex-col md:flex-row items-center justify-between gap-6">
        <div class="text-xs text-brand-muted">
          &copy; 2026 Bernie Jay Manalo. All rights reserved.
        </div>
      </div>
    </footer>
  </div>
</template>
