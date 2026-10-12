<script setup>
import { computed, onMounted, onBeforeUnmount, ref } from 'vue'

const darkTheme = ref(false)
const mobileMenuOpen = ref(false)
const searchQuery = ref('')
const activeFilter = ref('Todos')
const selectedProject = ref(null)
const contactStatus = ref('')
const contactForm = ref({ name: '', email: '', message: '' })

const filters = ['Todos', 'WordPress', 'Frontend', 'SEO', 'Vue.js']

const projects = ref([
  {
    id: 1,
    title: 'Portal institucional',
    category: 'WordPress',
    description: 'Administración y evolución de plataformas institucionales, con atención a la estructura de contenidos, experiencia de usuario y calidad técnica.',
    challenge: 'Mantener una plataforma web organizada, accesible y preparada para evolucionar.',
    solution: 'Trabajo con CMS, estructura de contenidos, revisión técnica y buenas prácticas de publicación.',
    technologies: ['WordPress', 'PHP', 'SEO técnico'],
    icon: '▦',
    tone: 'indigo',
    demo: '',
    repository: ''
  },
  {
    id: 2,
    title: 'Multiverse For Men',
    category: 'Frontend',
    description: 'Proyecto editorial digital sobre tecnología, videojuegos, cine, series, streaming, inteligencia artificial y cultura geek.',
    challenge: 'Organizar contenido editorial y ofrecer una experiencia consistente entre frontend y CMS.',
    solution: 'Arquitectura frontend desacoplada con WordPress como gestor de contenidos.',
    technologies: ['Astro', 'Tailwind CSS', 'WordPress Headless'],
    icon: '✦',
    tone: 'cyan',
    demo: 'https://multiversemen.com/',
    repository: ''
  },
  {
    id: 3,
    title: 'SEO Audit Tool',
    category: 'Vue.js',
    description: 'Prototipo de interfaz para organizar revisiones de SEO técnico y visualizar hallazgos de auditoría.',
    challenge: 'Presentar observaciones técnicas de forma clara y priorizable.',
    solution: 'Interfaz reactiva con filtros, estados y tarjetas de resultados. La auditoría real requiere conectar un backend o API.',
    technologies: ['Vue 3', 'JavaScript', 'API REST'],
    icon: '⌕',
    tone: 'emerald',
    demo: '',
    repository: ''
  },
  {
    id: 4,
    title: 'Panel de rendimiento web',
    category: 'SEO',
    description: 'Concepto de dashboard para presentar indicadores de rendimiento, optimización y salud técnica de un sitio.',
    challenge: 'Convertir métricas técnicas en información fácil de interpretar.',
    solution: 'Componentes de indicadores y filtros preparados para recibir datos de una API.',
    technologies: ['Vue 3', 'Charts', 'Web Vitals'],
    icon: '↗',
    tone: 'orange',
    demo: '',
    repository: ''
  },
  {
    id: 5,
    title: 'Gestor de tareas web',
    category: 'Vue.js',
    description: 'Aplicación demostrativa para organizar tareas, cambiar estados y visualizar el trabajo pendiente.',
    challenge: 'Gestionar tareas sin recargar la página.',
    solution: 'Estado reactivo, formularios y componentes reutilizables en Vue 3.',
    technologies: ['Vue 3', 'Composition API', 'CSS'],
    icon: '☷',
    tone: 'violet',
    demo: '',
    repository: ''
  },
  {
    id: 6,
    title: 'Sitio web para cafetería',
    category: 'Frontend',
    description: 'Ejemplo de sitio comercial con una presentación visual cálida, estructura adaptable y tarjetas de contenido.',
    challenge: 'Comunicar una marca y sus servicios de forma directa.',
    solution: 'Diseño adaptable con HTML semántico, CSS y componentes de interfaz.',
    technologies: ['HTML', 'CSS', 'JavaScript'],
    icon: '☕',
    tone: 'rose',
    demo: '',
    repository: ''
  }
])

const filteredProjects = computed(() => {
  const query = searchQuery.value.trim().toLowerCase()
  return projects.value.filter((project) => {
    const matchesCategory = activeFilter.value === 'Todos'
      || project.category === activeFilter.value
      || (activeFilter.value === 'WordPress' && project.technologies.some(t => t.toLowerCase().includes('wordpress')))
      || (activeFilter.value === 'Frontend' && project.technologies.some(t => ['astro', 'html', 'css', 'tailwind css'].some(k => t.toLowerCase().includes(k))))
      || (activeFilter.value === 'SEO' && project.technologies.some(t => t.toLowerCase().includes('seo')))
    const searchable = `${project.title} ${project.description} ${project.technologies.join(' ')}`.toLowerCase()
    return matchesCategory && searchable.includes(query)
  })
})

const skills = [
  { name: 'WordPress', group: 'CMS', description: 'Administración, personalización y evolución de sitios basados en WordPress.' },
  { name: 'Drupal', group: 'CMS', description: 'Gestión de contenidos y operación de plataformas CMS.' },
  { name: 'Vue.js', group: 'Frontend', description: 'Componentes, estado reactivo y desarrollo de interfaces modernas.' },
  { name: 'JavaScript', group: 'Frontend', description: 'Interactividad, lógica de interfaz y consumo de servicios.' },
  { name: 'HTML y CSS', group: 'Frontend', description: 'Estructura semántica, accesibilidad y diseño responsivo.' },
  { name: 'PHP', group: 'Backend', description: 'Desarrollo y personalización de funcionalidades del lado del servidor.' },
  { name: 'Python', group: 'Backend', description: 'Automatización y desarrollo de herramientas.' },
  { name: 'Linux y servidores', group: 'Infraestructura', description: 'Administración técnica, diagnóstico y operación web.' },
  { name: 'SEO técnico', group: 'Optimización', description: 'Indexación, migraciones, arquitectura y calidad técnica.' },
  { name: 'Rendimiento web', group: 'Optimización', description: 'Optimización de recursos, carga y experiencia de usuario.' },
  { name: 'Git y Docker', group: 'Herramientas', description: 'Control de versiones y entornos de desarrollo.' },
  { name: 'Seguridad web', group: 'Infraestructura', description: 'Buenas prácticas, actualizaciones, backups y control de accesos.' }
]

const skillGroups = ['Todas', ...new Set(skills.map(skill => skill.group))]
const activeSkillGroup = ref('Todas')
const visibleSkills = computed(() => activeSkillGroup.value === 'Todas'
  ? skills
  : skills.filter(skill => skill.group === activeSkillGroup.value))

const counters = [
  { value: 'CMS', label: 'Experiencia con gestores de contenido' },
  { value: 'SEO', label: 'Optimización y migraciones web' },
  { value: 'UX/UI', label: 'Enfoque en experiencia de usuario' }
]

function openProject(project) {
  selectedProject.value = project
  document.body.classList.add('modal-open')
}
function closeProject() {
  selectedProject.value = null
  document.body.classList.remove('modal-open')
}
function handleKeydown(event) {
  if (event.key === 'Escape') closeProject()
}
function toggleTheme() {
  darkTheme.value = !darkTheme.value
  document.documentElement.dataset.theme = darkTheme.value ? 'dark' : 'light'
}
function submitContact() {
  const { name, email, message } = contactForm.value
  const subject = encodeURIComponent(`Contacto desde portafolio: ${name}`)
  const body = encodeURIComponent(`Nombre: ${name}\nCorreo: ${email}\n\n${message}`)
  contactStatus.value = 'Se abrirá tu aplicación de correo para enviar el mensaje.'
  window.location.href = `mailto:chlauckz@poligran.edu.co?subject=${subject}&body=${body}`
}

onMounted(() => window.addEventListener('keydown', handleKeydown))
onBeforeUnmount(() => {
  window.removeEventListener('keydown', handleKeydown)
  document.body.classList.remove('modal-open')
})
</script>

<template>
  <div class="site-shell">
    <a class="skip-link" href="#inicio">Saltar al contenido principal</a>

    <header class="site-header">
      <div class="container header-inner">
        <a class="brand" href="#inicio" aria-label="Christian Lauck, inicio">Christian<span>Lauck</span><i>.</i></a>
        <div class="header-actions">
          <nav class="site-nav" :class="{ 'is-open': mobileMenuOpen }" aria-label="Navegación principal">
            <a href="#sobre-mi" @click="mobileMenuOpen = false">Sobre mí</a>
            <a href="#habilidades" @click="mobileMenuOpen = false">Habilidades</a>
            <a href="#proyectos" @click="mobileMenuOpen = false">Proyectos</a>
            <a href="#contacto" @click="mobileMenuOpen = false">Contacto</a>
          </nav>
          <button class="icon-button theme-toggle" type="button" :aria-label="darkTheme ? 'Activar tema claro' : 'Activar tema oscuro'" @click="toggleTheme">
            <span aria-hidden="true">{{ darkTheme ? '☀' : '☾' }}</span>
          </button>
          <button class="icon-button menu-toggle" type="button" :aria-expanded="mobileMenuOpen" aria-label="Abrir menú" @click="mobileMenuOpen = !mobileMenuOpen">
            <span></span><span></span><span></span>
          </button>
        </div>
      </div>
    </header>

    <main>
      <section id="inicio" class="hero section">
        <div class="container hero-grid">
          <div class="hero-copy">
            <p class="eyebrow"><span class="status-dot"></span> Webmaster · Desarrollo web · Colombia</p>
            <h1>Construyo experiencias web <span class="gradient-text">útiles, rápidas y sostenibles.</span></h1>
            <p class="hero-lead">Soy Christian Lauck Zuluaga, especialista Webmaster enfocado en plataformas digitales, CMS, SEO técnico, rendimiento y soluciones web centradas en las personas.</p>
            <div class="hero-actions">
              <a class="button button-primary" href="#proyectos">Explorar proyectos <span aria-hidden="true">↗</span></a>
              <a class="button button-secondary" href="#contacto">Hablemos</a>
            </div>
            <div class="hero-meta">
              <span><span class="meta-mark">⌘</span> Desarrollo y operación web</span>
              <span><span class="meta-mark">◎</span> Enfoque técnico y estratégico</span>
            </div>
          </div>
          <div class="hero-visual" aria-label="Ilustración abstracta de desarrollo web">
            <div class="visual-glow"></div>
            <div class="code-window">
              <div class="window-bar"><span></span><span></span><span></span><small>portfolio.vue</small></div>
              <div class="code-lines" aria-hidden="true">
                <p><span class="code-purple">const</span> developer = {</p>
                <p class="indent"><span class="code-blue">name</span>: <span class="code-green">'Christian Lauck'</span>,</p>
                <p class="indent"><span class="code-blue">focus</span>: [</p>
                <p class="indent2"><span class="code-green">'Web performance'</span>,</p>
                <p class="indent2"><span class="code-green">'Technical SEO'</span>,</p>
                <p class="indent2"><span class="code-green">'Vue.js'</span></p>
                <p class="indent">],</p>
                <p class="indent"><span class="code-blue">build</span>: <span class="code-purple">true</span></p>
                <p>}</p>
                <p class="code-comment">// Ideas convertidas en experiencias.</p>
              </div>
              <div class="window-footer"><span class="status-dot"></span> Ready to build <span>Vue 3 · UI</span></div>
            </div>
            <div class="floating-chip chip-one"><span>⚡</span> Performance</div>
            <div class="floating-chip chip-two"><span>⌕</span> Technical SEO</div>
            <div class="orbit orbit-one"></div><div class="orbit orbit-two"></div>
          </div>
        </div>
      </section>

      <section class="proof-strip" aria-label="Áreas de especialidad">
        <div class="container proof-inner">
          <span>CMS & plataformas</span><i></i><span>SEO técnico</span><i></i><span>Rendimiento web</span><i></i><span>Frontend moderno</span><i></i><span>Infraestructura</span>
        </div>
      </section>

      <section id="sobre-mi" class="section">
        <div class="container about-grid">
          <div class="section-intro">
            <p class="section-kicker">01 / SOBRE MÍ</p>
            <h2>Conecto la visión técnica con las necesidades del negocio.</h2>
          </div>
          <div class="about-content">
            <p class="large-copy">Mi trabajo combina administración de plataformas, desarrollo web y optimización técnica para que los sitios no solo se vean bien: también sean mantenibles, accesibles y fáciles de encontrar.</p>
            <p>Me interesa resolver problemas reales mediante una buena arquitectura de contenidos, procesos de publicación claros, rendimiento, seguridad y experiencias digitales consistentes. Actualmente amplío mi stack con herramientas modernas de frontend como Vue.js.</p>
            <div class="about-facts">
              <div><span class="fact-label">Perfil</span><strong>Webmaster / Desarrollo web</strong></div>
              <div><span class="fact-label">Especialidad</span><strong>CMS, SEO y rendimiento</strong></div>
              <div><span class="fact-label">Enfoque</span><strong>Calidad técnica y UX</strong></div>
            </div>
          </div>
        </div>
      </section>

      <section class="metrics-section">
        <div class="container metrics-grid">
          <article v-for="item in counters" :key="item.value" class="metric-card">
            <p class="metric-value">{{ item.value }}</p>
            <p class="metric-label">{{ item.label }}</p>
          </article>
        </div>
      </section>

      <section id="habilidades" class="section">
        <div class="container">
          <div class="section-heading">
            <p class="section-kicker">02 / HABILIDADES</p>
            <h2>Herramientas para construir y mantener la web.</h2>
            <p>Explora las áreas de trabajo y tecnologías de mi stack.</p>
          </div>
          <div class="filter-row skill-filters" aria-label="Filtrar habilidades">
            <button v-for="group in skillGroups" :key="group" class="filter-button" :class="{ active: activeSkillGroup === group }" @click="activeSkillGroup = group">{{ group }}</button>
          </div>
          <div class="skills-grid">
            <article v-for="skill in visibleSkills" :key="skill.name" class="skill-card">
              <div class="skill-card-top"><span class="skill-symbol">{{ skill.name === 'Vue.js' ? 'V' : skill.name === 'WordPress' ? 'W' : '✳' }}</span><span class="skill-group">{{ skill.group }}</span></div>
              <h3>{{ skill.name }}</h3>
              <p>{{ skill.description }}</p>
            </article>
          </div>
        </div>
      </section>

      <section id="proyectos" class="section projects-section">
        <div class="container">
          <div class="projects-heading">
            <div class="section-heading align-left">
              <p class="section-kicker">03 / PROYECTOS</p>
              <h2>Trabajo, experimentación y aprendizaje.</h2>
              <p>Una selección de proyectos y conceptos que muestran distintas áreas de mi trabajo.</p>
            </div>
            <div class="project-count"><strong>{{ filteredProjects.length.toString().padStart(2, '0') }}</strong><span>proyectos visibles</span></div>
          </div>
          <div class="project-tools">
            <div class="filter-row" aria-label="Filtrar proyectos">
              <button v-for="filter in filters" :key="filter" class="filter-button" :class="{ active: activeFilter === filter }" @click="activeFilter = filter">{{ filter }}</button>
            </div>
            <label class="search-box">
              <span aria-hidden="true">⌕</span>
              <input v-model="searchQuery" type="search" placeholder="Buscar proyecto o tecnología..." aria-label="Buscar proyectos" />
              <kbd>↵</kbd>
            </label>
          </div>
          <div v-if="filteredProjects.length" class="projects-grid">
            <article v-for="project in filteredProjects" :key="project.id" class="project-card">
              <div class="project-art" :class="`tone-${project.tone}`">
                <span class="project-art-index">PROJECT / {{ String(project.id).padStart(2, '0') }}</span>
                <span class="project-art-icon">{{ project.icon }}</span>
                <span class="project-art-label">{{ project.category }}</span>
                <div class="art-lines"></div>
              </div>
              <div class="project-content">
                <div class="project-category">{{ project.category }}</div>
                <h3>{{ project.title }}</h3>
                <p>{{ project.description }}</p>
                <div class="tag-list"><span v-for="tech in project.technologies" :key="tech">{{ tech }}</span></div>
                <button class="text-link" type="button" @click="openProject(project)">Explorar proyecto <span aria-hidden="true">↗</span></button>
              </div>
            </article>
          </div>
          <div v-else class="empty-state"><span>⌕</span><h3>No encontramos proyectos</h3><p>Prueba con otro término o selecciona una categoría diferente.</p><button class="button button-secondary" @click="searchQuery = ''; activeFilter = 'Todos'">Limpiar filtros</button></div>
          <p class="project-note">Nota: los proyectos identificados como conceptos o prototipos son demostraciones de portafolio, no auditorías ni resultados de producción.</p>
        </div>
      </section>

      <section class="cta-section">
        <div class="container cta-card">
          <div><p class="section-kicker">¿TIENES UN RETO DIGITAL?</p><h2>Hagamos que funcione mejor.</h2><p>Hablemos de desarrollo web, plataformas digitales, rendimiento u optimización técnica.</p></div>
          <a class="button button-light" href="#contacto">Contactar <span aria-hidden="true">↗</span></a>
          <div class="cta-decoration"></div>
        </div>
      </section>

      <section id="contacto" class="section contact-section">
        <div class="container contact-grid">
          <div class="section-intro">
            <p class="section-kicker">04 / CONTACTO</p>
            <h2>¿Conversamos sobre tu próximo proyecto?</h2>
            <p>Cuéntame brevemente qué necesitas y nos ponemos en contacto.</p>
            <a class="contact-email" href="mailto:chlauckz@poligran.edu.co">chlauckz@poligran.edu.co <span>↗</span></a>
          </div>
          <form class="contact-form" @submit.prevent="submitContact">
            <label>Nombre<input v-model.trim="contactForm.name" required autocomplete="name" placeholder="Tu nombre" /></label>
            <label>Correo electrónico<input v-model.trim="contactForm.email" type="email" required autocomplete="email" placeholder="tu@correo.com" /></label>
            <label>¿En qué podemos trabajar?<textarea v-model.trim="contactForm.message" required rows="4" placeholder="Cuéntame sobre tu proyecto..."></textarea></label>
            <button class="button button-primary" type="submit">Preparar correo <span aria-hidden="true">↗</span></button>
            <p v-if="contactStatus" class="form-status" role="status">{{ contactStatus }}</p>
            <p class="form-note">Este formulario prepara un mensaje en tu aplicación de correo; no almacena datos en un servidor.</p>
          </form>
        </div>
      </section>
    </main>

    <footer class="site-footer">
      <div class="container footer-inner">
        <a class="brand footer-brand" href="#inicio">Christian<span>Lauck</span><i>.</i></a>
        <p>Diseñado y desarrollado con Vue 3.</p>
        <a href="#inicio" class="back-top">Volver arriba ↑</a>
        <span class="copyright">© {{ new Date().getFullYear() }} Christian Lauck Zuluaga</span>
      </div>
    </footer>

    <Transition name="modal">
      <div v-if="selectedProject" class="modal-backdrop" @click.self="closeProject" role="presentation">
        <section class="project-modal" role="dialog" aria-modal="true" :aria-label="selectedProject.title">
          <button class="modal-close" type="button" aria-label="Cerrar detalles" @click="closeProject">×</button>
          <div class="modal-art" :class="`tone-${selectedProject.tone}`"><span class="project-art-icon">{{ selectedProject.icon }}</span><span class="project-art-label">{{ selectedProject.category }}</span></div>
          <div class="modal-body">
            <p class="section-kicker">CASO DE ESTUDIO / {{ String(selectedProject.id).padStart(2, '0') }}</p>
            <h2>{{ selectedProject.title }}</h2>
            <p class="modal-description">{{ selectedProject.description }}</p>
            <div class="case-block"><h3>El reto</h3><p>{{ selectedProject.challenge }}</p></div>
            <div class="case-block"><h3>Enfoque de solución</h3><p>{{ selectedProject.solution }}</p></div>
            <div class="tag-list"><span v-for="tech in selectedProject.technologies" :key="tech">{{ tech }}</span></div>
            <a v-if="selectedProject.demo" class="button button-primary modal-demo" :href="selectedProject.demo" target="_blank" rel="noopener noreferrer">Visitar proyecto <span aria-hidden="true">↗</span></a>
          </div>
        </section>
      </div>
    </Transition>
  </div>
</template>
