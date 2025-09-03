<script setup>
import { ref, computed, onMounted, nextTick } from 'vue'
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'

// Reactive data
const activeSection = ref('hero')
const mobileMenuOpen = ref(false)
const selectedCategory = ref('All')
const skillsAnimated = ref(false)
const isSubmitting = ref(false)
const showSuccessModal = ref(false)

// Form data
const form = ref({
  name: '',
  email: '',
  message: ''
})

// Personal information
const personalInfo = ref({
  name: "Pratchaya Champates",
  title: "IT Support",
  bio: "A quick learner with excellent problem-solving skills, seeking to gain practical experience in IT support. Ready to assist with troubleshooting, system maintenance, and providing reliable technical assistance.",
  email: "pratchjp@hotmail.com",
  phone: "099-972-3075",
  location: "Bangkok, Thailand",
  avatar: "/profile/mypic.jpg"
})

// Navigation sections
const sections = ref([
  { id: 'hero', name: 'Home' },
  { id: 'about', name: 'About' },
  { id: 'skills', name: 'Skills' },
  { id: 'experience', name: 'Experience' },
  { id: 'projects', name: 'Projects' },
  { id: 'contact', name: 'Contact' }
])

// Contact links
const contactLinks = ref([
  { type: 'email', icon: 'fas fa-envelope', label: 'Email', url: 'pratchjp@hotmail.com' },
  { type: 'github', icon: 'fab fa-github', label: 'GitHub', url: 'https://github.com/Pratchaya-jamp' },
  { type: 'linkedin', icon: 'fab fa-linkedin', label: 'LinkedIn', url: 'https://www.linkedin.com/in/pratchaya-champates-a77012381/' }
])

// Stats
const stats = ref([
  { value: '7', label: 'Projects' },
  { value: '3', label: 'Years Education' },
  { value: '5', label: 'Frameworks' },
  { value: '2', label: 'Languages' }
])

// Quick facts
const quickFacts = ref([
  'Agile/Scrum Enthusiast',
  'Continuous Learner',
  'Team Player & Mentor'
])

// Skills data
const skills = ref([
  { name: "Vue.js", level: 65, category: "Frontend" },
  { name: "TypeScript", level: 50, category: "Language" },
  { name: "JavaScript", level: 60, category: "Language" },
  { name: "Java", level: 60, category: "Backend" },
  { name: "ElysiaJS", level: 55, category: "Backend" },
  { name: "MySQL", level: 70, category: "Database" },
  { name: "JSON", level: 75, category: "Database" },
  { name: "XML", level: 53, category: "Database" },
  { name: "Docker", level: 76, category: "DevOps" },
  { name: "NGINX", level: 52, category: "DevOps" },
  { name: "Linux", level: 76, category: "OS" },
  { name: "MacOS", level: 40, category: "OS" },
  { name: "Windows", level: 85, category: "OS" },
  { name: "Git", level: 75, category: "Tools" },
  { name: "Wireshark", level: 60, category: "Tools" },
  { name: "Networking", level: 60, category: "Tools" },
  { name: "Postman", level: 75, category: "Tools" },
  { name: "Maven", level: 50, category: "Tools" },
  { name: "Tailwind CSS", level: 50, category: "Frontend" }
])

// Experience data
const experience = ref([
  {
    company: "IT3K#19",
    position: "Wireless Networking",
    period: "09 March 2025",
    description: "planned and configured a secure wireless network, which included surveying the site, placing Access Points, and setting up secure access via LDAP/RADIUS and WPA2 Personal.",
    technologies: ['Networking', 'Switch Configuration', 'WLAN Monitoring']
  },
  {
    company: "ITB-MSHOP Web Application",
    position: "Frontend Developer & Infrastructure",
    period: "In-Progress",
    description: "Working in a Scrum team, I helped develop and deploy a task management web application, setting up the server using Docker and Nginx.",
    technologies: ["Vue.js", "Java", "MySQL", "Linux", "HTML", "CSS"]
  },
  {
    company: "Vue NoteApp",
    position: "Web Developer",
    period: "2019 - 2020",
    description: "Developed custom websites and web applications for various clients. Focused on performance optimization and SEO implementation.",
    technologies: ["JavaScript", "PHP", "MySQL", "WordPress", "jQuery"]
  }
])

// Projects data
const projects = ref([
  {
    id: 1,
    name: "E-Commerce Platform",
    description: "Full-stack e-commerce solution with real-time inventory, payment processing, and admin dashboard.",
    technologies: ["Vue.js", "Node.js", "Stripe", "PostgreSQL"],
    github: "https://github.com/alexjohnson/ecommerce",
    demo: "https://demo.ecommerce.com",
    icon: "fas fa-shopping-cart"
  },
  {
    id: 2,
    name: "Task Management App",
    description: "Collaborative project management tool with real-time updates, file sharing, and team chat.",
    technologies: ["React", "Socket.io", "Express", "MongoDB"],
    github: "https://github.com/alexjohnson/taskmanager",
    demo: "https://demo.taskmanager.com",
    icon: "fas fa-tasks"
  },
  {
    id: 3,
    name: "Weather Dashboard",
    description: "Beautiful weather application with interactive maps, forecasts, and location-based alerts.",
    technologies: ["Vue.js", "Chart.js", "Weather API", "Tailwind"],
    github: "https://github.com/alexjohnson/weather",
    demo: "https://demo.weather.com",
    icon: "fas fa-cloud-sun"
  },
  {
    id: 4,
    name: "Social Media Analytics",
    description: "Dashboard for tracking social media metrics with data visualization and automated reporting.",
    technologies: ["React", "D3.js", "Python", "FastAPI"],
    github: "https://github.com/alexjohnson/analytics",
    demo: "https://demo.analytics.com",
    icon: "fas fa-chart-line"
  },
  {
    id: 5,
    name: "Recipe Sharing Platform",
    description: "Community-driven recipe platform with user ratings, meal planning, and grocery list generation.",
    technologies: ["Vue.js", "Firebase", "Nuxt.js", "Vuetify"],
    github: "https://github.com/alexjohnson/recipes",
    demo: "https://demo.recipes.com",
    icon: "fas fa-utensils"
  },
  {
    id: 6,
    name: "Cryptocurrency Tracker",
    description: "Real-time crypto portfolio tracker with price alerts, news integration, and market analysis.",
    technologies: ["React", "Redux", "CoinGecko API", "Chart.js"],
    github: "https://github.com/alexjohnson/crypto",
    demo: "https://demo.crypto.com",
    icon: "fab fa-bitcoin"
  }
])

// Computed properties
const skillCategories = computed(() => {
  const categories = ['All', ...new Set(skills.value.map(skill => skill.category))]
  return categories
})

const filteredSkills = computed(() => {
  if (selectedCategory.value === 'All') {
    return skills.value
  }
  return skills.value.filter(skill => skill.category === selectedCategory.value)
})

const yearsOfExperience = computed(() => {
  const startYear = 2019
  const currentYear = new Date().getFullYear()
  return currentYear - startYear
})

const currentYear = computed(() => new Date().getFullYear())

// Methods
const scrollToSection = (sectionId) => {
  const element = document.getElementById(sectionId)
  if (element) {
    element.scrollIntoView({ behavior: 'smooth' })
    activeSection.value = sectionId
    mobileMenuOpen.value = false
  }
}

const toggleMobileMenu = () => {
  mobileMenuOpen.value = !mobileMenuOpen.value
}

const submitForm = async () => {
  isSubmitting.value = true
  
  // Simulate API call
  setTimeout(() => {
    isSubmitting.value = false
    showSuccessModal.value = true
    form.value = { name: '', email: '', message: '' }
  }, 2000)
}

const handleScroll = () => {
  const sections = ['hero', 'about', 'skills', 'experience', 'projects', 'contact']
  
  for (let section of sections) {
    const element = document.getElementById(section)
    if (element) {
      const rect = element.getBoundingClientRect()
      if (rect.top <= 100 && rect.bottom >= 100) {
        activeSection.value = section
        
        // Animate skills when skills section is visible
        if (section === 'skills' && !skillsAnimated.value) {
          setTimeout(() => {
            skillsAnimated.value = true
          }, 200)
        }
        break
      }
    }
  }
}

// Lifecycle
onMounted(() => {
  window.addEventListener('scroll', handleScroll)
  
  // Initial scroll check
  nextTick(() => {
    handleScroll()
  })
})
</script>

<template>
  <div class="min-h-screen bg-gradient-to-br from-purple-600 via-blue-600 to-indigo-800">
    <!-- Navigation -->
    <nav class="fixed top-0 left-0 right-0 z-50 bg-white/10 backdrop-blur-md border-b border-white/20">
      <div class="max-w-6xl mx-auto px-6 py-4">
        <div class="flex justify-between items-center">
          <h1 class="text-2xl font-bold text-white">{{ personalInfo.name }}</h1>
          <div class="hidden md:flex space-x-6">
            <button 
              v-for="section in sections" 
              :key="section.id"
              @click="scrollToSection(section.id)"
              :class="[
                'px-4 py-2 rounded-full transition-all duration-300',
                activeSection === section.id 
                  ? 'bg-white text-purple-600 shadow-lg' 
                  : 'text-white hover:bg-white/20'
              ]"
            >
              {{ section.name }}
            </button>
          </div>
          <button 
            @click="toggleMobileMenu"
            class="md:hidden text-white text-xl"
          >
            <i class="fas fa-bars"></i>
          </button>
        </div>
      </div>
    </nav>

    <!-- Mobile Menu -->
    <div 
      v-show="mobileMenuOpen"
      class="fixed inset-0 z-40 bg-black/50 md:hidden"
      @click="toggleMobileMenu"
    >
      <div class="fixed top-20 right-4 bg-white/90 backdrop-blur-md rounded-lg p-6 space-y-4">
        <button 
          v-for="section in sections" 
          :key="section.id"
          @click="scrollToSection(section.id)"
          class="block w-full text-left px-4 py-2 rounded-lg hover:bg-purple-100 transition-colors"
        >
          {{ section.name }}
        </button>
      </div>
    </div>

    <!-- Hero Section -->
    <section id="hero" class="pt-32 pb-20 px-6">
      <div class="max-w-6xl mx-auto text-center">
        <div class="mb-8">
          <img 
            :src="personalInfo.avatar" 
            :alt="personalInfo.name"
            class="w-32 h-32 rounded-full mx-auto mb-6 border-4 border-white/30 shadow-2xl floating"
          >
          <h1 class="text-5xl md:text-7xl font-bold text-white mb-4 fade-in">
            {{ personalInfo.name }}
          </h1>
          <p class="text-2xl md:text-3xl text-purple-200 mb-8 fade-in">
            {{ personalInfo.title }}
          </p>
          <p class="text-lg text-white/80 max-w-2xl mx-auto mb-8 fade-in">
            {{ personalInfo.bio }}
          </p>
        </div>
        
        <!-- Contact Info -->
        <div class="flex flex-wrap justify-center gap-6 mb-8">
          <a 
            v-for="contact in contactLinks" 
            :key="contact.type"
            :href="contact.url" 
            class="flex items-center space-x-2 bg-white/20 backdrop-blur-sm px-4 py-2 rounded-full text-white hover:bg-white/30 transition-all duration-300 hover:scale-105"
          >
            <i :class="contact.icon"></i>
            <span>{{ contact.label }}</span>
          </a>
        </div>
        
        <button 
          @click="scrollToSection('about')"
          class="animate-bounce text-white text-2xl"
        >
          <i class="fas fa-chevron-down"></i>
        </button>
      </div>
    </section>

    <!-- About Section -->
    <section id="about" class="py-20 px-6 bg-white">
      <div class="max-w-6xl mx-auto">
        <h2 class="text-4xl font-bold text-gray-800 mb-12 text-center">About Me</h2>
        <div class="grid md:grid-cols-2 gap-12 items-center">
          <div>
            <p class="text-lg text-gray-600 mb-6 leading-relaxed">
              A third-year student from the School of Information Technology (SIT), 
              King Mongkut’s University of Technology Thonburi, with a strong passion for IT Support, 
              especially in technical troubleshooting, system maintenance, and end-user services. 
            </p>
            <p class="text-lg text-gray-600 mb-8 leading-relaxed">
              I have skills in teamwork, effective communication, and problem-solving. 
              I am eager to learn, grow, and apply my knowledge through real-world experience, 
              with the goal of becoming a skilled and reliable IT professional.
            </p>
            <div class="grid grid-cols-2 gap-6">
              <div v-for="stat in stats" :key="stat.label" class="text-center">
                <div class="text-3xl font-bold text-purple-600 mb-2">{{ stat.value }}</div>
                <div class="text-gray-600">{{ stat.label }}</div>
              </div>
            </div>
          </div>
          <div class="relative">
            <div class="bg-gradient-to-r from-purple-500 to-blue-500 rounded-2xl p-8 text-white shadow-2xl transform rotate-3 hover:rotate-0 transition-transform duration-300">
              <h3 class="text-2xl font-bold mb-4">Quick Facts</h3>
              <ul class="space-y-3">
                <li v-for="fact in quickFacts" :key="fact" class="flex items-center space-x-2">
                  <i class="fas fa-check-circle text-yellow-300"></i>
                  <span>{{ fact }}</span>
                </li>
              </ul>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- Skills Section -->
    <section id="skills" class="py-20 px-6 bg-gray-50">
      <div class="max-w-6xl mx-auto">
        <h2 class="text-4xl font-bold text-gray-800 mb-12 text-center">Technical Skills</h2>
        
        <!-- Skill Categories -->
        <div class="flex flex-wrap justify-center mb-8 space-x-4">
          <button 
            v-for="category in skillCategories" 
            :key="category"
            @click="selectedCategory = category"
            :class="[
              'px-6 py-2 rounded-full transition-all duration-300 mb-2',
              selectedCategory === category 
                ? 'bg-purple-600 text-white shadow-lg' 
                : 'bg-white text-gray-600 hover:bg-purple-100'
            ]"
          >
            {{ category }}
          </button>
        </div>

        <!-- Skills Grid -->
        <div class="grid md:grid-cols-2 lg:grid-cols-3 gap-6">
          <div 
            v-for="skill in filteredSkills" 
            :key="skill.name"
            class="bg-white rounded-xl p-6 shadow-lg hover:shadow-xl transition-all duration-300 hover:scale-105"
          >
            <div class="flex justify-between items-center mb-4">
              <h3 class="text-lg font-semibold text-gray-800">{{ skill.name }}</h3>
              <span class="text-purple-600 font-bold">{{ skill.level }}%</span>
            </div>
            <div class="w-full bg-gray-200 rounded-full h-3">
              <div 
                class="bg-gradient-to-r from-purple-500 to-blue-500 h-3 rounded-full skill-bar"
                :style="{ width: skillsAnimated ? skill.level + '%' : '0%' }"
              ></div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- Experience Section -->
    <section id="experience" class="py-20 px-6 bg-white">
      <div class="max-w-6xl mx-auto">
        <h2 class="text-4xl font-bold text-gray-800 mb-12 text-center">Work Experience</h2>
        <div class="relative">
          <!-- Timeline Line -->
          <div class="absolute left-8 top-0 bottom-0 w-0.5 bg-purple-300 hidden md:block"></div>
          
          <div class="space-y-12">
            <div 
              v-for="(job, index) in experience" 
              :key="index"
              class="relative flex flex-col md:flex-row items-start"
            >
              <!-- Timeline Dot -->
              <div class="hidden md:flex absolute left-6 w-4 h-4 bg-purple-600 rounded-full border-4 border-white shadow-lg"></div>
              
              <!-- Content -->
              <div class="md:ml-20 bg-gray-50 rounded-xl p-8 shadow-lg hover:shadow-xl transition-all duration-300 w-full">
                <div class="flex flex-col md:flex-row md:justify-between md:items-start mb-4">
                  <div>
                    <h3 class="text-2xl font-bold text-gray-800 mb-2">{{ job.position }}</h3>
                    <h4 class="text-xl text-purple-600 font-semibold mb-2">{{ job.company }}</h4>
                  </div>
                  <span class="text-gray-600 bg-white px-4 py-2 rounded-full">{{ job.period }}</span>
                </div>
                <p class="text-gray-600 mb-6">{{ job.description }}</p>
                <div class="flex flex-wrap gap-2">
                  <span 
                    v-for="tech in job.technologies" 
                    :key="tech"
                    class="bg-purple-100 text-purple-700 px-3 py-1 rounded-full text-sm font-medium"
                  >
                    {{ tech }}
                  </span>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- Projects Section -->
    <section id="projects" class="py-20 px-6 bg-gray-50">
      <div class="max-w-6xl mx-auto">
        <h2 class="text-4xl font-bold text-gray-800 mb-12 text-center">Featured Projects</h2>
        <div class="grid md:grid-cols-2 lg:grid-cols-3 gap-8">
          <div 
            v-for="project in projects" 
            :key="project.id"
            class="bg-white rounded-xl overflow-hidden shadow-lg hover:shadow-2xl transition-all duration-300 hover:scale-105 group"
          >
            <div class="h-48 bg-gradient-to-br from-purple-400 to-blue-500 relative overflow-hidden">
              <div class="absolute inset-0 bg-black/20 group-hover:bg-black/10 transition-all duration-300"></div>
              <div class="absolute bottom-4 left-4 text-white">
                <i :class="project.icon + ' text-3xl'"></i>
              </div>
            </div>
            <div class="p-6">
              <h3 class="text-xl font-bold text-gray-800 mb-3">{{ project.name }}</h3>
              <p class="text-gray-600 mb-4">{{ project.description }}</p>
              <div class="flex flex-wrap gap-2 mb-4">
                <span 
                  v-for="tech in project.technologies" 
                  :key="tech"
                  class="bg-gray-100 text-gray-700 px-2 py-1 rounded text-sm"
                >
                  {{ tech }}
                </span>
              </div>
              <div class="flex space-x-4">
                <a 
                  :href="project.github" 
                  class="flex items-center space-x-2 text-purple-600 hover:text-purple-800 transition-colors"
                >
                  <Github size="16" />
                  <span>Code</span>
                </a>
                <a 
                  :href="project.demo" 
                  class="flex items-center space-x-2 text-blue-600 hover:text-blue-800 transition-colors"
                >
                  <Globe size="16" />
                  <span>Demo</span>
                </a>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- Contact Section -->
    <section id="contact" class="py-20 px-6 bg-gradient-to-r from-purple-600 to-blue-600">
      <div class="max-w-4xl mx-auto text-center">
        <h2 class="text-4xl font-bold text-white mb-8">Let's Work Together</h2>
        <p class="text-xl text-purple-100 mb-12">
          Ready to bring your ideas to life? Let's discuss your next project!
        </p>
        
        <div class="grid md:grid-cols-3 gap-8 mb-12">
          <div class="bg-white/10 backdrop-blur-sm rounded-xl p-6 text-white hover:bg-white/20 transition-all duration-300">
            <h3 class="text-lg font-semibold mb-2">Email</h3>
            <p class="text-purple-100">{{ personalInfo.email }}</p>
          </div>
          <div class="bg-white/10 backdrop-blur-sm rounded-xl p-6 text-white hover:bg-white/20 transition-all duration-300">
            <Phone class="mx-auto mb-4" size="32" />
            <h3 class="text-lg font-semibold mb-2">Phone</h3>
            <p class="text-purple-100">{{ personalInfo.phone }}</p>
          </div>
          <div class="bg-white/10 backdrop-blur-sm rounded-xl p-6 text-white hover:bg-white/20 transition-all duration-300">
            <MapPin class="mx-auto mb-4" size="32" />
            <h3 class="text-lg font-semibold mb-2">Location</h3>
            <p class="text-purple-100">{{ personalInfo.location }}</p>
          </div>
        </div>

        <!-- Contact Form -->
        <form @submit.prevent="submitForm" class="bg-white/10 backdrop-blur-sm rounded-xl p-8 max-w-2xl mx-auto">
          <div class="grid md:grid-cols-2 gap-6 mb-6">
            <input 
              v-model="form.name"
              type="text" 
              placeholder="Your Name"
              class="w-full px-4 py-3 rounded-lg bg-white/20 border border-white/30 text-white placeholder-purple-200 focus:outline-none focus:ring-2 focus:ring-white/50"
              required
            >
            <input 
              v-model="form.email"
              type="email" 
              placeholder="Your Email"
              class="w-full px-4 py-3 rounded-lg bg-white/20 border border-white/30 text-white placeholder-purple-200 focus:outline-none focus:ring-2 focus:ring-white/50"
              required
            >
          </div>
          <textarea 
            v-model="form.message"
            placeholder="Your Message"
            rows="5"
            class="w-full px-4 py-3 rounded-lg bg-white/20 border border-white/30 text-white placeholder-purple-200 focus:outline-none focus:ring-2 focus:ring-white/50 mb-6"
            required
          ></textarea>
          <button 
            type="submit"
            :disabled="isSubmitting"
            class="w-full bg-white text-purple-600 py-3 px-8 rounded-lg font-semibold hover:bg-purple-50 transition-all duration-300 disabled:opacity-50"
          >
            {{ isSubmitting ? 'Sending...' : 'Send Message' }}
          </button>
        </form>
      </div>
    </section>

    <!-- Footer -->
    <footer class="bg-gray-900 text-white py-8 px-6">
      <div class="max-w-6xl mx-auto text-center">
        <p>&copy; {{ currentYear }} {{ personalInfo.name }}. Built with Vue.js & Tailwind CSS.</p>
      </div>
    </footer>

    <!-- Success Modal -->
    <div 
      v-if="showSuccessModal"
      class="fixed inset-0 bg-black/50 flex items-center justify-center z-50 px-6"
      @click="showSuccessModal = false"
    >
      <div class="bg-white rounded-xl p-8 max-w-md mx-auto text-center">
        <div class="text-green-500 text-5xl mb-4">
          <i class="fas fa-check-circle"></i>
        </div>
        <h3 class="text-2xl font-bold text-gray-800 mb-4">Message Sent!</h3>
        <p class="text-gray-600 mb-6">Thanks for reaching out. I'll get back to you soon!</p>
        <button 
          @click="showSuccessModal = false"
          class="bg-purple-600 text-white px-6 py-2 rounded-lg hover:bg-purple-700 transition-colors"
        >
          Close
        </button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.floating {
  animation: floating 3s ease-in-out infinite;
}

@keyframes floating {
  0%, 100% { transform: translateY(0px); }
  50% { transform: translateY(-10px); }
}

.fade-in {
  animation: fadeIn 0.8s ease-out forwards;
}

@keyframes fadeIn {
  from { opacity: 0; transform: translateY(20px); }
  to { opacity: 1; transform: translateY(0); }
}

.skill-bar {
  transition: width 1.5s ease-out;
}

/* Smooth scroll */
html {
  scroll-behavior: smooth;
}

/* Custom scrollbar */
::-webkit-scrollbar {
  width: 8px;
}

::-webkit-scrollbar-track {
  background: #f1f1f1;
}

::-webkit-scrollbar-thumb {
  background: linear-gradient(45deg, #667eea, #764ba2);
  border-radius: 4px;
}

::-webkit-scrollbar-thumb:hover {
  background: linear-gradient(45deg, #5a6fd8, #6a4190);
}
</style>