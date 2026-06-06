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
  <div class="min-h-screen font-sans bg-[#fafafa] text-[#171717] selection:bg-[#eaeaea] selection:text-black">
    <!-- Header / Sticky Navigation -->
    <header 
      class="fixed top-0 left-0 w-full z-50 transition-all duration-300 border-b"
      :class="isScrolled ? 'bg-white/80 backdrop-blur-md border-[#eaeaea] py-4' : 'bg-transparent border-transparent py-6'"
    >
      <div class="max-w-6xl mx-auto px-6 md:px-12 flex items-center justify-between">
        <!-- Logo -->
        <a href="#home" class="group flex items-center space-x-2">
            <span class="w-8 h-8 rounded-full border-2 border-[#171717] flex items-center justify-center font-serif font-semibold text-lg transition-transform duration-300 group-hover:rotate-12">BJ</span>
        </a>

        <!-- Menu Links -->
        <nav class="hidden md:flex items-center space-x-8">
          <a 
            v-for="link in navLinks" 
            :key="link.id" 
            :href="`#${link.id}`"
            class="text-xs tracking-widest uppercase transition-colors duration-200 relative py-1"
            :class="activeSection === link.id ? 'text-[#171717] font-medium' : 'text-[#737373] hover:text-[#171717]'"
          >
            {{ link.label }}
            <!-- Active underline transition -->
            <span 
              class="absolute bottom-0 left-0 w-full h-[1px] bg-[#171717] scale-x-0 transition-transform duration-300 origin-left"
              :class="{ 'scale-x-100': activeSection === link.id }"
            ></span>
          </a>
        </nav>

        <!-- Right Side Nav: Desktop CTA + Mobile Hamburger -->
        <div class="flex items-center space-x-4">
          <!-- CTA Action -->
          <a 
              href="#contact" 
              class="hidden md:inline-block border border-[#171717] px-4 py-2 text-xs tracking-widest uppercase transition-all duration-300 hover:bg-[#171717] hover:text-[#fafafa]"
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
              class="w-full h-[2px] bg-[#171717] transition-all duration-300"
              :class="{ 'rotate-45 translate-y-[9px]': isMobileMenuOpen }"
            ></span>
            <span 
              class="w-full h-[2px] bg-[#171717] transition-all duration-300"
              :class="{ 'opacity-0 scale-x-0': isMobileMenuOpen }"
            ></span>
            <span 
              class="w-full h-[2px] bg-[#171717] transition-all duration-300"
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
        class="fixed inset-0 z-40 bg-white flex flex-col justify-center px-8"
      >
        <nav class="flex flex-col space-y-8 text-center">
          <a 
            v-for="link in navLinks" 
            :key="link.id" 
            :href="`#${link.id}`"
            @click="isMobileMenuOpen = false"
            class="font-serif text-3xl font-light tracking-wide text-[#171717] hover:italic hover:font-normal transition-all duration-200"
          >
            {{ link.label }}
          </a>
          <!-- Mobile CTA in Overlay -->
          <a 
            href="#contact" 
            @click="isMobileMenuOpen = false"
            class="mt-4 border border-[#171717] px-6 py-3 text-xs tracking-widest uppercase transition-all duration-300 bg-[#171717] text-[#fafafa] hover:bg-transparent hover:text-[#171717] max-w-xs mx-auto w-full text-center"
          >
            Hire Me
          </a>
        </nav>
      </div>
    </transition>

    <!-- Main Container -->
    <main class="pt-24 max-w-6xl mx-auto px-6 md:px-12">
      <!-- Section 1: Hero / About -->
      <section id="home" class="min-h-[85vh] flex flex-col justify-center py-12 md:py-20 border-b border-[#eaeaea]">
        <div class="grid grid-cols-1 lg:grid-cols-12 gap-12 lg:gap-16 items-start fade-in-up">
          <!-- Text Content -->
          <div class="lg:col-span-7 order-2 lg:order-1">
            <!-- Small Label -->
            <div class="inline-flex items-center space-x-2 bg-[#f0f0f0] border border-[#e5e5e5] px-3 py-1 rounded-full text-xs font-medium text-[#737373] mb-6">
              <span class="w-2 h-2 rounded-full bg-emerald-500 animate-pulse"></span>
              <span>Available for New Projects</span>
            </div>

            <!-- Headline -->
            <h1 class="font-serif text-4xl sm:text-5xl md:text-6xl lg:text-7xl font-light leading-tight tracking-tight text-[#171717] mb-8">
              Building complete, <br class="hidden sm:inline" />
              <span class="italic font-normal">scalable systems</span> <br class="hidden sm:inline" />
              and interfaces.
            </h1>

            <!-- Intro Paragraph -->
            <p class="text-base sm:text-lg text-[#525252] leading-relaxed max-w-2xl mb-10">
              Hi, I'm <strong class="text-[#171717] font-semibold">Bernie Jay Manalo</strong>. 
              I'm a Full-Stack Developer focused on building clean, scalable, and high-performance web applications. 
              By combining robust backend systems, detailed design systems, and modern frameworks like Vue.js, 
              I construct end-to-end digital experiences that are highly performant and accessible.
            </p>

            <!-- Action Buttons -->
            <div class="flex flex-wrap items-center gap-4">
              <a 
                href="#projects" 
                class="bg-[#171717] text-[#fafafa] border border-[#171717] px-6 py-3.5 text-xs tracking-widest uppercase font-semibold transition-all duration-300 hover:bg-transparent hover:text-[#171717] hover:shadow-sm"
              >
                Explore Selected Work
              </a>
              <a 
                href="#contact" 
                class="border border-[#eaeaea] bg-white text-[#171717] px-6 py-3.5 text-xs tracking-widest uppercase font-semibold transition-all duration-300 hover:border-[#171717] hover:bg-[#fafafa]"
              >
                Get In Touch
              </a>
            </div>
          </div>

          <!-- Image Content -->
          <div class="lg:col-span-5 order-1 lg:order-2 flex justify-center lg:justify-end mb-6 lg:mb-0 lg:pt-14">
            <div class="relative group w-full max-w-[280px] sm:max-w-[320px] aspect-square">
              <!-- Decorative background frame -->
              <div class="absolute inset-0 border-2 border-[#171717] translate-x-3 translate-y-3 transition-transform duration-300 group-hover:translate-x-1.5 group-hover:translate-y-1.5"></div>
              
              <!-- Image container -->
              <div class="relative w-full h-full bg-white border border-[#eaeaea] p-2.5 overflow-hidden shadow-md">
                <img 
                  src="/profile.jpg" 
                  alt="Bernie Jay Manalo" 
                  class="w-full h-full object-cover transition-transform duration-500 group-hover:scale-105"
                />
              </div>

              <!-- Status Badge -->
              <div class="absolute -bottom-3 -left-3 bg-white border border-[#171717] px-3 py-1.5 shadow-sm flex items-center space-x-2">
                <span class="w-1.5 h-1.5 rounded-full bg-emerald-500 animate-pulse"></span>
                <span class="text-[9px] font-mono uppercase tracking-wider text-[#171717] font-semibold">BJ MANALO / DEV</span>
              </div>
            </div>
          </div>
        </div>
      </section>

      <!-- Section 2: Selected Projects -->
      <section id="projects" class="py-20 md:py-28 border-b border-[#eaeaea]">
        <div class="flex flex-col md:flex-row md:items-end justify-between mb-16 gap-6">
          <div>
            <span class="text-xs tracking-widest uppercase text-[#737373] block mb-3 font-semibold">Selected Showcase</span>
            <h2 class="font-serif text-3xl md:text-4xl font-light">Featured Projects</h2>
          </div>
          
          <!-- Category Filters -->
          <div class="flex flex-wrap gap-2">
            <button 
              v-for="cat in categories" 
              :key="cat.value"
              @click="activeFilter = cat.value"
              class="px-4 py-2 text-xs tracking-wider transition-all duration-200 border rounded-none"
              :class="activeFilter === cat.value 
                ? 'bg-[#171717] border-[#171717] text-[#fafafa] font-semibold' 
                : 'border-[#eaeaea] bg-white hover:border-[#737373] text-[#525252]'"
            >
              {{ cat.label }}
            </button>
          </div>
        </div>

        <!-- Projects Grid -->
        <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
          <div 
            v-for="project in filteredProjects" 
            :key="project.title"
            class="group bg-white border border-[#eaeaea] p-6 sm:p-8 md:p-10 flex flex-col justify-between transition-all duration-300 hover:border-[#171717] hover:shadow-[0_10px_30px_rgba(0,0,0,0.02)]"
          >
            <div>
              <!-- Tags -->
              <div class="flex flex-wrap gap-2 mb-6">
                <span 
                  v-for="tech in project.tech" 
                  :key="tech"
                  class="bg-[#fafafa] border border-[#eaeaea] text-[10px] tracking-wider uppercase text-[#737373] px-2 py-0.5"
                >
                  {{ tech }}
                </span>
              </div>

              <!-- Title -->
              <h3 class="font-serif text-2xl font-normal group-hover:text-[#171717] mb-4 transition-colors duration-200">
                {{ project.title }}
              </h3>

              <!-- Description -->
              <p class="text-sm text-[#525252] leading-relaxed mb-8">
                {{ project.description }}
              </p>
            </div>

            <!-- Links -->
            <div class="flex items-center border-t border-[#f5f5f5] pt-6 mt-4">
              <!-- Demo Link -->
              <a 
                :href="project.link" 
                class="inline-flex items-center text-xs tracking-wider uppercase text-[#171717] font-semibold group/link"
              >
                <span>Live Site</span>
                <svg xmlns="http://www.w3.org/2000/svg" class="h-3.5 w-3.5 ml-1.5 transition-transform duration-300 group-hover/link:translate-x-1" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                  <path stroke-linecap="round" stroke-linejoin="round" d="M14 5l7 7m0 0l-7 7m7-7H3" />
                </svg>
              </a>
            </div>
          </div>
        </div>
      </section>

      <!-- Section 3: Skills & Expertise -->
      <section id="skills" class="py-20 md:py-28 border-b border-[#eaeaea]">
        <div class="mb-16">
          <span class="text-xs tracking-widest uppercase text-[#737373] block mb-3 font-semibold">My Toolkit</span>
          <h2 class="font-serif text-3xl md:text-4xl font-light">Capabilities & Expertise</h2>
        </div>

        <div class="grid grid-cols-1 md:grid-cols-3 gap-12">
          <!-- Skill category 1 -->
          <div>
            <div class="flex items-center space-x-3 mb-6 pb-2 border-b border-[#171717]">
              <span class="font-serif text-sm italic font-semibold text-[#171717]">01.</span>
              <h3 class="text-xs tracking-widest uppercase font-semibold text-[#171717]">Core Engineering</h3>
            </div>
            <ul class="space-y-4">
              <li class="flex flex-row items-baseline justify-between gap-4 text-sm">
                <span class="text-[#525252]">Vue.js / Vuex / Pinia</span>
                <span class="text-[#a3a3a3] font-mono text-[11px] shrink-0">Advanced</span>
              </li>
              <li class="flex flex-row items-baseline justify-between gap-4 text-sm">
                <span class="text-[#525252]">JavaScript (ES6+) & TypeScript</span>
                <span class="text-[#a3a3a3] font-mono text-[11px] shrink-0">Proficient</span>
              </li>
              <li class="flex flex-row items-baseline justify-between gap-4 text-sm">
                <span class="text-[#525252]">Tailwind CSS v3 & v4</span>
                <span class="text-[#a3a3a3] font-mono text-[11px] shrink-0">Advanced</span>
              </li>
              <li class="flex flex-row items-baseline justify-between gap-4 text-sm">
                <span class="text-[#525252]">HTML5 / Semantic Layouts</span>
                <span class="text-[#a3a3a3] font-mono text-[11px] shrink-0">Expert</span>
              </li>
            </ul>
          </div>

          <!-- Skill category 2 -->
          <div>
            <div class="flex items-center space-x-3 mb-6 pb-2 border-b border-[#171717]">
              <span class="font-serif text-sm italic font-semibold text-[#171717]">02.</span>
              <h3 class="text-xs tracking-widest uppercase font-semibold text-[#171717]">Tooling & Bundlers</h3>
            </div>
            <ul class="space-y-4">
              <li class="flex flex-row items-baseline justify-between gap-4 text-sm">
                <span class="text-[#525252]">Vite & Rollup</span>
                <span class="text-[#a3a3a3] font-mono text-[11px] shrink-0">Highly Proficient</span>
              </li>
              <li class="flex flex-row items-baseline justify-between gap-4 text-sm">
                <span class="text-[#525252]">Git / GitHub Workflows</span>
                <span class="text-[#a3a3a3] font-mono text-[11px] shrink-0">Advanced</span>
              </li>
              <li class="flex flex-row items-baseline justify-between gap-4 text-sm">
                <span class="text-[#525252]">npm / yarn / pnpm</span>
                <span class="text-[#a3a3a3] font-mono text-[11px] shrink-0">Experienced</span>
              </li>
              <li class="flex flex-row items-baseline justify-between gap-4 text-sm">
                <span class="text-[#525252]">Chrome DevTools & Perf Profiling</span>
                <span class="text-[#a3a3a3] font-mono text-[11px] shrink-0">Intermediate</span>
              </li>
            </ul>
          </div>

          <!-- Skill category 3 -->
          <div>
            <div class="flex items-center space-x-3 mb-6 pb-2 border-b border-[#171717]">
              <span class="font-serif text-sm italic font-semibold text-[#171717]">03.</span>
              <h3 class="text-xs tracking-widest uppercase font-semibold text-[#171717]">Design & UX</h3>
            </div>
            <ul class="space-y-4">
              <li class="flex flex-row items-baseline justify-between gap-4 text-sm">
                <span class="text-[#525252]">User Interface (UI) Design</span>
                <span class="text-[#a3a3a3] font-mono text-[11px] shrink-0">Creative</span>
              </li>
              <li class="flex flex-row items-baseline justify-between gap-4 text-sm">
                <span class="text-[#525252]">Figma Prototyping</span>
                <span class="text-[#a3a3a3] font-mono text-[11px] shrink-0">Intermediate</span>
              </li>
              <li class="flex flex-row items-baseline justify-between gap-4 text-sm">
                <span class="text-[#525252]">Typography & Visual Hierarchy</span>
                <span class="text-[#a3a3a3] font-mono text-[11px] shrink-0">Refined</span>
              </li>
              <li class="flex flex-row items-baseline justify-between gap-4 text-sm">
                <span class="text-[#525252]">Responsive Design Standards</span>
                <span class="text-[#a3a3a3] font-mono text-[11px] shrink-0">Expert</span>
              </li>
            </ul>
          </div>
        </div>
      </section>

      <!-- Section 4: Work Experience timeline -->
      <section id="experience" class="py-20 md:py-28 border-b border-[#eaeaea]">
        <div class="mb-16">
          <span class="text-xs tracking-widest uppercase text-[#737373] block mb-3 font-semibold">Background</span>
          <h2 class="font-serif text-3xl md:text-4xl font-light">Experience Timeline</h2>
        </div>

        <div class="space-y-12 max-w-3xl">
          <!-- Exp Item 1 -->
          <div class="relative pl-6 sm:pl-8 border-l border-[#eaeaea] group">
            <!-- Bullet dot -->
            <div class="absolute top-[9px] -left-[4.5px] w-2 h-2 rounded-full bg-[#171717] transition-all duration-300 group-hover:scale-150"></div>
            
            <div class="flex flex-col md:flex-row md:items-baseline md:justify-between mb-2 gap-1">
              <h3 class="text-lg font-semibold text-[#171717]">Full-Stack Developer</h3>
              <span class="text-xs text-[#737373] font-mono">Jan 2024 — Present</span>
            </div>
            <p class="text-sm font-serif italic text-[#525252] mb-3">Freelance Studio & Consulting</p>
            <p class="text-sm text-[#525252] leading-relaxed">
              Engineering scalable full-stack applications, robust APIs, and interactive client portals. Standardizing deployment flows and modern web architectures using fast bundlers (Vite), relational databases, and Vue 3 frontends.
            </p>
          </div>

          <!-- Exp Item 2 -->
          <div class="relative pl-6 sm:pl-8 border-l border-[#eaeaea] group">
            <div class="absolute top-[9px] -left-[4.5px] w-2 h-2 rounded-full bg-[#a3a3a3] group-hover:bg-[#171717] transition-all duration-300 group-hover:scale-150"></div>
            
            <div class="flex flex-col md:flex-row md:items-baseline md:justify-between mb-2 gap-1">
              <h3 class="text-lg font-semibold text-[#171717]">Junior Full-Stack Engineer</h3>
              <span class="text-xs text-[#737373] font-mono">Jul 2022 — Dec 2023</span>
            </div>
            <p class="text-sm font-serif italic text-[#525252] mb-3">Interactive Tech Solutions</p>
            <p class="text-sm text-[#525252] leading-relaxed">
              Designed client dashboards using Vue 3 and integrated headless CMS engines. Improved site speed indices by 35% through bundle splitting, dynamic component loading, and asset tuning.
            </p>
          </div>

          <!-- Exp Item 3 -->
          <div class="relative pl-6 sm:pl-8 border-l border-[#eaeaea] group">
            <div class="absolute top-[9px] -left-[4.5px] w-2 h-2 rounded-full bg-[#a3a3a3] group-hover:bg-[#171717] transition-all duration-300 group-hover:scale-150"></div>
            
            <div class="flex flex-col md:flex-row md:items-baseline md:justify-between mb-2 gap-1">
              <h3 class="text-lg font-semibold text-[#171717]">Web Development Intern</h3>
              <span class="text-xs text-[#737373] font-mono">Mar 2022 — Jun 2022</span>
            </div>
            <p class="text-sm font-serif italic text-[#525252] mb-3">WebCraft Labs</p>
            <p class="text-sm text-[#525252] leading-relaxed">
              Maintained responsive websites, styled email newsletters, and wrote semantic HTML templates. Gained early experience with client-side reactive models and version control pipelines.
            </p>
          </div>
        </div>
      </section>

      <!-- Section 5: Contact -->
      <section id="contact" class="py-20 md:py-28">
        <div class="grid grid-cols-1 md:grid-cols-2 gap-12 md:gap-16">
          <div>
            <span class="text-xs tracking-widest uppercase text-[#737373] block mb-3 font-semibold">Contact Info</span>
            <h2 class="font-serif text-3xl md:text-4xl font-light mb-8">Let's build something exceptional together.</h2>
            <p class="text-sm text-[#525252] leading-relaxed mb-10 max-w-md">
              I am interested in freelance opportunities, contract work, or joining engineering teams that value attention to detail, code architecture, and high-end visual design.
            </p>

            <div class="space-y-6">
              <!-- Email -->
              <div class="flex items-center space-x-4">
                <div class="w-10 h-10 border border-[#eaeaea] bg-white flex items-center justify-center">
                  <svg xmlns="http://www.w3.org/2000/svg" class="h-4.5 w-4.5 text-[#171717]" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="1.5">
                    <path stroke-linecap="round" stroke-linejoin="round" d="M3 8l7.89 5.26a2 2 0 002.22 0L21 8M5 19h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v10a2 2 0 002 2z" />
                  </svg>
                </div>
                <div>
                  <span class="text-[10px] tracking-wider uppercase text-[#737373] block">Direct Email</span>
                  <a href="mailto:manalobj19@gmail.com" class="text-sm font-semibold hover:underline text-[#171717]">manalobj19@gmail.com</a>
                </div>
              </div>

              <!-- Location -->
              <div class="flex items-center space-x-4">
                <div class="w-10 h-10 border border-[#eaeaea] bg-white flex items-center justify-center">
                  <svg xmlns="http://www.w3.org/2000/svg" class="h-4.5 w-4.5 text-[#171717]" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="1.5">
                    <path stroke-linecap="round" stroke-linejoin="round" d="M17.657 16.657L13.414 20.9a1.998 1.998 0 01-2.827 0l-4.244-4.243a8 8 0 1111.314 0z" />
                    <path stroke-linecap="round" stroke-linejoin="round" d="M15 11a3 3 0 11-6 0 3 3 0 016 0z" />
                  </svg>
                </div>
                <div>
                  <span class="text-[10px] tracking-wider uppercase text-[#737373] block">Location</span>
                  <span class="text-sm font-semibold text-[#171717]">Barbaza, Antique, Philippines</span>
                </div>
              </div>
            </div>
          </div>

          <!-- Contact Form -->
          <div class="bg-white border border-[#eaeaea] p-6 sm:p-8 md:p-10">
            <h3 class="font-serif text-xl mb-6 font-medium">Send a Message</h3>
            
            <form action="https://api.web3forms.com/submit" method="POST" class="space-y-6">
              <input type="hidden" name="access_key" value="5bd6a01f-6beb-408c-996d-b480c3a0888b" />
              <!-- Name Input -->
              <div>
                <label for="name" class="block text-[10px] tracking-wider uppercase font-semibold text-[#737373] mb-2">Your Name</label>
                <input 
                  id="name"
                  name="name"
                  type="text" 
                  v-model="form.name"
                  required
                  class="w-full border border-[#eaeaea] bg-[#fafafa] px-4 py-3 text-sm focus:outline-none focus:border-[#171717] focus:bg-white transition-colors duration-200"
                  placeholder="e.g. Jane Doe"
                />
              </div>

              <!-- Email Input -->
              <div>
                <label for="email" class="block text-[10px] tracking-wider uppercase font-semibold text-[#737373] mb-2">Email Address</label>
                <input 
                  id="email"
                  name="email"
                  type="email" 
                  v-model="form.email"
                  required
                  class="w-full border border-[#eaeaea] bg-[#fafafa] px-4 py-3 text-sm focus:outline-none focus:border-[#171717] focus:bg-white transition-colors duration-200"
                  placeholder="name@company.com"
                />
              </div>

              <!-- Message Input -->
              <div>
                <label for="message" class="block text-[10px] tracking-wider uppercase font-semibold text-[#737373] mb-2">Message</label>
                <textarea 
                  id="message"
                  name="message"
                  v-model="form.message"
                  required
                  rows="4"
                  class="w-full border border-[#eaeaea] bg-[#fafafa] px-4 py-3 text-sm focus:outline-none focus:border-[#171717] focus:bg-white transition-colors duration-200 resize-none"
                  placeholder="Tell me about your project..."
                ></textarea>
              </div>

              <!-- Submit Button -->
              <button 
                type="submit" 
                :disabled="isSubmitting"
                class="w-full bg-[#171717] text-[#fafafa] border border-[#171717] py-3.5 text-xs tracking-widest uppercase font-semibold transition-all duration-300 hover:bg-transparent hover:text-[#171717] disabled:opacity-50"
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
    <footer class="border-t border-[#eaeaea] py-12 mt-12 bg-white">
      <div class="max-w-6xl mx-auto px-6 md:px-12 flex flex-col md:flex-row items-center justify-between gap-6">
        <div class="text-xs text-[#737373]">
          &copy; 2026 Bernie Jay Manalo. All rights reserved.
        </div>
      </div>
    </footer>
  </div>
</template>
