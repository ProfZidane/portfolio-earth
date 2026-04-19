<template>
  <div class="app">
    <Navbar :current="currentView" @navigate="navigate" />

    <IntroOverlay @done="introSeen = true" />

    <!-- Globe view -->
    <template v-if="currentView === 'world'">
      <Globe :sidebar-open="!!selectedCountry" @country-click="onCountryClick" />
      <Sidebar :country="selectedCountry" @close="closeSidebar" />
    </template>

    <!-- Products view -->
    <Transition name="page">
      <ProductPage v-if="currentView === 'products'" />
    </Transition>

    <!-- About view -->
    <Transition name="page">
      <About v-if="currentView === 'about'" />
    </Transition>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import Navbar from './components/Navbar.vue'
import Globe from './components/Globe.vue'
import Sidebar from './components/Sidebar.vue'
import ProductPage from './components/ProductPage.vue'
import About from './components/About.vue'
import IntroOverlay from './components/IntroOverlay.vue'

const introSeen = ref(false)

const currentView = ref('world')
const selectedCountry = ref(null)

const navigate = (view) => {
  selectedCountry.value = null
  currentView.value = view
}

const onCountryClick = (country) => {
  selectedCountry.value = country
}

const closeSidebar = () => {
  selectedCountry.value = null
}
</script>

<style>
.app {
  width: 100%;
  height: 100%;
  position: relative;
}

.page-enter-active, .page-leave-active {
  transition: opacity 0.35s ease, transform 0.35s ease;
}
.page-enter-from {
  opacity: 0;
  transform: translateY(16px);
}
.page-leave-to {
  opacity: 0;
  transform: translateY(-16px);
}
</style>
