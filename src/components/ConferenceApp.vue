<script setup lang="ts">
import { ref, reactive, computed } from 'vue'

const currentSection = ref('inicio')
const showConfigMenu = ref(false)
const paperSearch = ref('')
const sessionSearch = ref('')
const showMobileMenu = ref(false)

const navSections = [
  { id: 'inicio', name: 'Inicio' },
  { id: 'trabajos', name: 'Trabajos' },
  { id: 'sesiones', name: 'Sesiones' }
]

const newAttendee = reactive({
  name: '',
  surname: '',
  institution: '',
  email: '',
  phone: ''
})

const loginData = reactive({
  email: '',
  password: ''
})

const toggleConfigMenu = () => {
  showConfigMenu.value = !showConfigMenu.value
}

const toggleMobileMenu = () => {
  showMobileMenu.value = !showMobileMenu.value
}

const registerAttendee = () => {
  console.log('Registrando asistente:', newAttendee)
  Object.keys(newAttendee).forEach((key) => (newAttendee)[key] = '');
  showConfigMenu.value = false
}

const login = () => {
  console.log('Iniciando sesión:', loginData)
  loginData.email = ''
  loginData.password = ''
  showConfigMenu.value = false
}

const conferenceData = {
  papers: [
    {
      id: 1,
      title: "Avances en Inteligencia Artificial",
      abstract: "Este trabajo presenta los últimos avances en el campo de la Inteligencia Artificial, incluyendo nuevos algoritmos de aprendizaje profundo y sus aplicaciones en diversos sectores como la salud, la industria y la educación.",
      authors: ["Dr. Juan Pérez", "Dra. María García"],
      image: "/public/img/inteligencia-artificial.jpg"
    },
    {
      id: 2,
      title: "Nuevas Técnicas en Aprendizaje Profundo",
      abstract: "Exploramos las técnicas más recientes en el campo del aprendizaje profundo, con un enfoque especial en las redes neuronales convolucionales y su aplicación en el procesamiento de imágenes médicas para la detección temprana de enfermedades.",
      authors: ["Dr. Carlos Rodríguez"],
      image: "/public/img/deep-learning.jpeg"
    },
    {
      id: 3,
      title: "Aplicaciones de la Computación Cuántica",
      abstract: "Este trabajo examina las aplicaciones emergentes de la computación cuántica en diversos campos, desde la criptografía hasta la simulación de sistemas moleculares complejos.",
      authors: ["Dra. Laura Martínez", "Dr. Alejandro Gómez"],
      image: "/public/img/computacion-cuantica.jpg"
    }
  ],
  sessions: [
    {
      id: 1,
      room: "Auditorio A",
      time: "09:00 - 11:00",
      chairman: "Dr. Roberto Sánchez",
      presentations: [
        {
          id: 1,
          title: "Avances en Inteligencia Artificial",
          presenter: "Dr. Juan Pérez",
          presenterImage: "/public/img/peterNorving.png"
        },
        {
          id: 2,
          title: "Aplicaciones de la IA en la Medicina",
          presenter: "Dra. Ana Martínez",
          presenterImage: "/public/img/breazeal.jpg"
        }
      ]
    },
    {
      id: 2,
      room: "Sala B",
      time: "11:30 - 13:30",
      chairman: "Dra. Laura Gómez",
      presentations: [
        {
          id: 3,
          title: "Nuevas Técnicas en Aprendizaje Profundo",
          presenter: "Dr. Ramón Lopez",
          presenterImage: "/public/img/ramon-lopez.jpg"
        }
      ]
    },
    {
      id: 3,
      room: "Auditorio B",
      time: "14:00 - 16:00",
      chairman: "Dr. Miguel Fernández",
      presentations: [
        {
          id: 4,
          title: "Aplicaciones de la Computación Cuántica",
          presenter: "Dra. Kate CrawFord",
          presenterImage: "/public/img/Kate-Crawford.jpg"
        }
      ]
    }
  ],
  speakers: [
    {
      id: 1,
      name: "Dr. Peter Norving",
      title: "Investigador Principal en IA",
      experience: "20+ años en investigación de Inteligencia Artificial",
      image: "/public/img/peterNorving.png"
    },
    {
      id: 2,
      name: "Dra. Cinthya Breazeal",
      title: "Experta en Aprendizaje Profundo",
      experience: "Pionera en aplicaciones de IA en medicina",
      image: "/public/img/breazeal.jpg"
    },
    {
      id: 3,
      name: "Dr. Ramón Lopez",
      title: "Científico de Datos Senior",
      experience: "Líder en proyectos de Big Data y Machine Learning",
      image: "/public/img/ramon-lopez.jpg"
    }
  ]
}

const filteredPapers = computed(() => {
  return conferenceData.papers.filter(paper =>
    paper.title.toLowerCase().includes(paperSearch.value.toLowerCase()) ||
    paper.abstract.toLowerCase().includes(paperSearch.value.toLowerCase()) ||
    paper.authors.some(author => author.toLowerCase().includes(paperSearch.value.toLowerCase()))
  )
})

const filteredSessions = computed(() => {
  return conferenceData.sessions.filter(session =>
    session.room.toLowerCase().includes(sessionSearch.value.toLowerCase()) ||
    session.chairman.toLowerCase().includes(sessionSearch.value.toLowerCase()) ||
    session.presentations.some(presentation =>
      presentation.title.toLowerCase().includes(sessionSearch.value.toLowerCase()) ||
      presentation.presenter.toLowerCase().includes(sessionSearch.value.toLowerCase())
    )
  )
})

const upcomingSessions = computed(() => {
  return conferenceData.sessions.slice(0, 3).map(session => ({
    id: session.id,
    title: session.presentations[0].title,
    time: session.time,
    room: session.room
  }))
})

const featuredPapers = computed(() => {
  return conferenceData.papers.slice(0, 3)
})

const featuredSpeakers = computed(() => {
  return conferenceData.speakers
})
</script>

<template>
  <div class="min-h-screen bg-gray-100">
    <nav class="bg-blue-600 text-white p-4 shadow-md fixed top-0 left-0 right-0 z-10">
      <div class="max-w-7xl mx-auto flex justify-between items-center">

        <div class="flex items-center space-x-6">
          <div class="bg-white p-2 rounded-lg">
            <img src="/public/img/atomo.png" alt="Logo de la conferencia"
              class="w-14 h-14 transition-transform duration-1000 hover:rotate-[359deg]">
          </div>
          <h2 class="text-2xl font-bold hidden md:block">Conferencia Científica 2024</h2>
        </div>

        <div class="hidden md:flex space-x-4">
          <button v-for="section in navSections" :key="section.id" @click="currentSection = section.id" :class="['px-3 py-2 rounded-md text-sm font-medium transition-colors duration-200',
            currentSection === section.id ? 'bg-blue-700' : 'hover:bg-blue-500']">
            {{ section.name }}
          </button>
          <div class="relative">
            <button @click="toggleConfigMenu"
              class="px-3 py-2 rounded-md text-sm font-medium hover:bg-blue-500 transition-colors duration-200">
              <img src="/public/img/stting.png" alt="Logo de la conferencia" class="w-12 h-12">
            </button>
            <div v-if="showConfigMenu" class="absolute right-0 mt-2 w-48 bg-white rounded-md shadow-lg z-10">
              <div class="py-1">
                <a href="#" @click.prevent="currentSection = 'registro'"
                  class="block px-4 py-2 text-sm text-gray-700 hover:bg-gray-100">Registro de Congresistas</a>
                <a href="#" @click.prevent="currentSection = 'login'"
                  class="block px-4 py-2 text-sm text-gray-700 hover:bg-gray-100">Iniciar Sesión</a>
              </div>
            </div>
          </div>
        </div>
        <button @click="toggleMobileMenu" class="md:hidden z-50 relative">
          <img src="/public/img/menu.png" alt="Logo de la conferencia" class="w-12 h-14">
        </button>
      </div>
      <div v-if="showMobileMenu"
        class="fixed inset-y-0 left-0 w-64 bg-blue-600 text-white p-4 transform transition-transform duration-300 ease-in-out z-50"
        :class="showMobileMenu ? 'translate-x-0' : '-translate-x-full'">
        <button @click="toggleMobileMenu" class="absolute top-4 right-4">
          <X class="h-6 w-6" />
        </button>
        <div class="mt-12 flex flex-col space-y-4">
          <button v-for="section in navSections" :key="section.id"
            @click="currentSection = section.id; showMobileMenu = false" :class="['px-3 py-2 rounded-md text-sm font-medium transition-colors duration-200',
              currentSection === section.id ? 'bg-blue-700' : 'hover:bg-blue-500']">
            {{ section.name }}
          </button>
          <button @click="currentSection = 'registro'; showMobileMenu = false"
            class="px-3 py-2 rounded-md text-sm font-medium hover:bg-blue-500 transition-colors duration-200">
            Registro de Congresistas
          </button>
          <button @click="currentSection = 'login'; showMobileMenu = false"
            class="px-3 py-2 rounded-md text-sm font-medium hover:bg-blue-500 transition-colors duration-200">
            Iniciar Sesión
          </button>
        </div>
      </div>
      <div v-if="showMobileMenu" class="fixed inset-0 bg-black bg-opacity-50 z-40" @click="toggleMobileMenu"></div>
    </nav>

    <div class="max-w-7xl mx-auto py-8 px-8 mt-24">
      <div v-if="currentSection === 'inicio'" class="space-y-8">
        <div class="flex flex-col items-center md:flex-row">
          <div
            class="bg-gradient-to-r from-blue-500 to-purple-600 text-white rounded-lg shadow-xl p-10 max-w-sm md:max-w-xl md:mr-16">
            <h2 class="text-4xl font-bold mb-4">Innovación y descubrimiento</h2>
            <p class="text-xl">La conferencia es un espacio de encuentro para expertos y entusiastas de la ciencia y la
              tecnología. Únete a nosotros para explorar los últimos avances, descubrir nuevas oportunidades y innovar
              en tu campo.
            </p>
          </div>
          <img src="/public/img/estudiante-ciencia.png" alt="Cientifico"
            class="w-8/12 md:w-5/12 md:order-2 slidedown-science">
        </div>

        <!-- Expositores destacados -->
        <div class="space-y-6 flex flex-col items-center">
          <h3 class="text-2xl font-semibold text-gray-800 text-center">Expositores Destacados</h3>
          <div v-for="speaker in featuredSpeakers" :key="speaker.id"
            class="flex flex-col items-center p-4 bg-gray-50 rounded-lg w-full max-w-2xl hover:scale-105 transition-transform duration-200">
            <img :src="speaker.image" :alt="speaker.name" class="w-24 h-24 rounded-full object-cover mb-4">
            <div class="text-center">
              <h4 class="text-xl font-semibold text-blue-600">{{ speaker.name }}</h4>
              <p class="text-gray-600">{{ speaker.title }}</p>
              <p class="text-sm text-gray-500 mt-1">{{ speaker.experience }}</p>
            </div>
          </div>
        </div>

        <!-- Trabajos destacados y próximas sesiones -->
        <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
          <div class="bg-white shadow-md rounded-lg p-6">
            <h3 class="text-2xl font-semibold mb-4 text-blue-600">Trabajos Destacados</h3>
            <ul class="space-y-4">
              <li v-for="paper in featuredPapers" :key="paper.id" class="border-b pb-4 last:border-b-0">
                <h4 class="text-lg font-medium text-gray-800">{{ paper.title }}</h4>
                <p class="text-sm text-gray-600">{{ paper.authors.join(', ') }}</p>
              </li>
            </ul>
          </div>

          <div class="bg-white shadow-md rounded-lg p-6">
            <h3 class="text-2xl font-semibold mb-4 text-green-600">Próximas Sesiones</h3>
            <ul class="space-y-4">
              <li v-for="session in upcomingSessions" :key="session.id"
                class="flex justify-between items-center border-b pb-4 last:border-b-0">
                <div>
                  <h4 class="text-lg font-medium text-gray-800">{{ session.title }}</h4>
                  <p class="text-sm text-gray-600">{{ session.room }}</p>
                </div>
                <span class="text-sm font-medium text-gray-500">{{ session.time }}</span>
              </li>
            </ul>
          </div>
        </div>
      </div>

      <!-- Registro de Congresistas -->
      <div v-if="currentSection === 'registro'" class="bg-white shadow-md rounded-2xl p-10 sm:w-5/12 mx-auto mt-8">
        <h2 class="text-2xl font-semibold mb-4 text-gray-800 text-center">Registro de Congresistas</h2>
        <form @submit.prevent="registerAttendee" class="space-y-4">
          <div>
            <label for="name" class="block text-sm font-medium text-gray-700">Nombre</label>
            <input v-model="newAttendee.name" type="text" id="name" required
              class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-blue-300 focus:ring focus:ring-blue-200 focus:ring-opacity-50">
          </div>
          <div>
            <label for="surname" class="block text-sm font-medium text-gray-700">Apellido</label>
            <input v-model="newAttendee.surname" type="text" id="surname" required
              class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-blue-300 focus:ring focus:ring-blue-200 focus:ring-opacity-50">
          </div>
          <div>
            <label for="institution" class="block text-sm font-medium text-gray-700">Institución</label>
            <input v-model="newAttendee.institution" type="text" id="institution" required
              class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-blue-300 focus:ring focus:ring-blue-200 focus:ring-opacity-50">
          </div>
          <div>
            <label for="email" class="block text-sm font-medium text-gray-700">Correo Electrónico</label>
            <input v-model="newAttendee.email" type="email" id="email" required
              class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-blue-300 focus:ring focus:ring-blue-200 focus:ring-opacity-50">
          </div>
          <div>
            <label for="phone" class="block text-sm font-medium text-gray-700">Teléfono Móvil (opcional)</label>
            <input v-model="newAttendee.phone" type="tel" id="phone"
              class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-blue-300 focus:ring focus:ring-blue-200 focus:ring-opacity-50">
          </div>
          <button type="submit"
            class="w-full bg-blue-500 text-white py-2 px-4 rounded-md hover:bg-blue-600 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-opacity-50">
            Registrarse
          </button>
        </form>
      </div>

      <!-- Login -->
      <div v-if="currentSection === 'login'" class="bg-white shadow-md rounded-2xl p-10 sm:w-5/12 mx-auto mt-8">
        <h2 class="text-2xl font-semibold mb-4 text-gray-800">Iniciar Sesión</h2>
        <form @submit.prevent="login" class="space-y-4">
          <div>
            <label for="login-email" class="block text-sm font-medium text-gray-700">Correo Electrónico</label>
            <input v-model="loginData.email" type="email" id="login-email" required
              class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-blue-300 focus:ring focus:ring-blue-200 focus:ring-opacity-50">
          </div>
          <div>
            <label for="login-password" class="block text-sm font-medium text-gray-700">Contraseña</label>
            <input v-model="loginData.password" type="password" id="login-password" required
              class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-blue-300 focus:ring focus:ring-blue-200 focus:ring-opacity-50">
          </div>
          <button type="submit"
            class="w-full bg-blue-500 text-white py-2 px-4 rounded-md hover:bg-blue-600 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-opacity-50">
            Iniciar Sesión
          </button>
        </form>
      </div>

      <!-- Trabajos Presentados -->
      <div v-if="currentSection === 'trabajos'" class="bg-white shadow-md rounded-lg p-6 mt-8">
        <h2 class="text-2xl font-semibold mb-4 text-gray-800">Trabajos Presentados</h2>
        <div class="mb-4">
          <input v-model="paperSearch" type="text" placeholder="Buscar trabajos..."
            class="w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-blue-500 focus:border-blue-500">
        </div>
        <div v-for="paper in filteredPapers" :key="paper.id" class="mb-6 p-4 border border-gray-200 rounded-lg">
          <h3 class="text-xl font-semibold text-blue-600">{{ paper.title }}</h3>
          <p class="text-gray-600 mt-2">{{ paper.abstract }}</p>
          <div class="mt-4">
            <h4 class="text-lg font-medium text-gray-700">Autores:</h4>
            <ul class="list-disc list-inside">
              <li v-for="author in paper.authors" :key="author" class="text-gray-600">{{ author }}</li>
            </ul>
          </div>
          <img :src="paper.image" :alt="paper.title" class="mt-4 w-full h-60 object-cover rounded-md">
        </div>
      </div>

      <!-- Sesiones del Congreso -->
      <div v-if="currentSection === 'sesiones'" class="bg-white shadow-md rounded-lg p-6 mt-8">
        <h2 class="text-2xl font-semibold mb-4 text-gray-800">Sesiones del Congreso</h2>
        <div class="mb-4">
          <input v-model="sessionSearch" type="text" placeholder="Buscar sesiones..."
            class="w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-blue-500 focus:border-blue-500">
        </div>
        <div v-for="session in filteredSessions" :key="session.id" class="mb-6 p-4 border border-gray-200 rounded-lg">
          <div class="flex justify-between items-start">
            <div>
              <h3 class="text-xl font-semibold text-blue-600">Sesión {{ session.id }}</h3>
              <p class="text-gray-600">Sala: {{ session.room }}</p>
              <p class="text-gray-600">Hora: {{ session.time }}</p>
            </div>
            <div class="text-right">
              <p class="text-gray-700 font-medium">Chairman:</p>
              <p class="text-gray-600">{{ session.chairman }}</p>
            </div>
          </div>
          <div class="mt-4">
            <h4 class="text-lg font-medium text-gray-700">Presentaciones:</h4>
            <ul class="space-y-4 mt-2">
              <li v-for="presentation in session.presentations" :key="presentation.id"
                class="flex items-center space-x-4">
                <img :src="presentation.presenterImage" :alt="presentation.presenter"
                  class="w-12 h-12 rounded-full object-cover">
                <div>
                  <p class="font-medium text-gray-800">{{ presentation.title }}</p>
                  <p class="text-gray-600">Presentador: {{ presentation.presenter }}</p>
                </div>
              </li>
            </ul>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
@keyframes slidedown-science {
  0% {
    transform: translateY(0);
  }

  50% {
    transform: translateY(20px);
  }

  100% {
    transform: translateY(0);
  }
}

.slidedown-science {
  animation: slidedown-science;
  animation-duration: 3s;
  animation-iteration-count: infinite;
}
</style>
