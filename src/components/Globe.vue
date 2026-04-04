<template>
    <div class="navbar">
  <div class="logo">ZM</div>

  <div class="nav-links">
    <a href="#">About</a>
    <a href="#">Projects</a>
    <a href="#">Contact</a>
    <a href="#">CV</a>
  </div>
</div>

  <div ref="globeContainer" class="globe"></div>
  <div v-if="selectedCountry" class="panel">
  
    <div class="panel-header">
        <h2>{{ selectedCountry.name }}</h2>
        <button @click="resetView" class="close-btn">✕</button>
    </div>

    <div class="panel-content horizontal">

        <div 
            v-for="exp in selectedCountry.experiences" 
            :key="exp.company"
            class="exp-card-horizontal"
        >
            <div class="exp-header">
            <h3>{{ exp.company }}</h3>
            <span class="role">{{ exp.role }}</span>
            </div>

            <p class="period">{{ exp.period }}</p>

            <p class="context">{{ exp.story.context }}</p>

            <p class="challenge">
            <strong>Challenge:</strong> {{ exp.story.challenge }}
            </p>

            <ul class="actions">
            <li v-for="a in exp.story.actions">{{ a }}</li>
            </ul>

            <p class="impact">{{ exp.story.impact }}</p>
        </div>

    </div>
  </div>
</template>

<style>
.globe {
  position: absolute;
  inset: 0;
}

.panel {
  position: absolute;
  bottom: 40px;
  left: 50%;
  transform: translateX(-50%);
  
  width: 90%;          /* plus flexible que 420px */
  max-width: 1200px;   /* limite la largeur */
  max-height: 60vh;

  background: rgba(15, 15, 25, 0.95);
  border-radius: 20px;
  backdrop-filter: blur(20px);
  color: white;
  overflow: hidden;

  box-shadow: 0 30px 80px rgba(0,0,0,0.8);
  z-index: 30;
}

.panel-content.horizontal {
  display: flex;
  gap: 24px;
  padding: 25px;

  overflow-x: auto;
  overflow-y: hidden;
}

/* Scroll smooth + invisible scrollbar */
.panel-content.horizontal::-webkit-scrollbar {
  display: none;
}

.exp-card-horizontal {
  min-width: 380px;
  max-width: 420px;

  flex-shrink: 0;

  padding: 20px;
  border-radius: 18px;

  background: rgba(255,255,255,0.05);

  transition: all 0.3s ease;
}

.exp-card-horizontal:hover {
  transform: scale(1.03);
  border: 1px solid rgba(0,255,200,0.4);

  box-shadow: 0 0 30px rgba(0,255,200,0.2);
  transform: scale(1.05);
  z-index: 10;
}

.exp-card-horizontal:active {
  transform: scale(0.98);
}

.panel::before,
.panel::after {
  content: "";
  position: absolute;
  top: 0;
  width: 40px;
  height: 100%;
  pointer-events: none;
}

.panel::before {
  left: 0;
  background: linear-gradient(to right, rgba(0,0,0,0.6), transparent);
}

.panel::after {
  right: 0;
  background: linear-gradient(to left, rgba(0,0,0,0.6), transparent);
}

.panel-header {
  display: flex;
  justify-content: space-between;
  align-items: center;

  padding: 15px 20px;

  border-bottom: 1px solid rgba(255,255,255,0.08);
}

.panel-content {
  padding: 15px;
  overflow-y: auto;
}

.exp-card {
  padding: 14px;
  border-radius: 14px;
  margin-bottom: 12px;

  background: rgba(255,255,255,0.03);

  border: 1px solid rgba(255,255,255,0.05);

  transition: all 0.3s ease;
}

.exp-card:hover {
  transform: translateY(-3px);
  background: rgba(0,255,200,0.08);

  box-shadow: 0 0 20px rgba(0,255,200,0.2);
}

.exp-block {
  margin-bottom: 20px;
  padding: 16px;

  border-radius: 16px;

  background: rgba(255,255,255,0.03);
  border: 1px solid rgba(255,255,255,0.05);

  transition: 0.3s;
}

.exp-block:hover {
  transform: translateY(-4px);
  border: 1px solid rgba(0,255,200,0.3);

  box-shadow: 0 0 25px rgba(0,255,200,0.2);
}

.exp-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.role {
  font-size: 12px;
  opacity: 0.6;
}

.context {
  margin-top: 8px;
  opacity: 0.8;
}

.challenge {
  margin-top: 8px;
  color: #ffaa00;
}

.actions {
  margin-top: 8px;
}

.actions li {
  margin-bottom: 5px;
  padding-left: 10px;
  position: relative;
}

.actions li::before {
  content: "";
  position: absolute;
  left: 0;
  top: 6px;
  width: 4px;
  height: 4px;
  background: #00ffc8;
  border-radius: 50%;
}

.impact {
  margin-top: 10px;
  color: #00ffc8;
  font-weight: 500;
}

.company {
  font-weight: 600;
  color: #00ffc8;
}

.period {
  font-size: 12px;
  opacity: 0.5;
}

.desc {
  font-size: 14px;
  margin-top: 6px;
  line-height: 1.4;
}

.close-btn {
  background: rgba(255,255,255,0.05);
  border: none;
  color: white;

  width: 28px;
  height: 28px;
  border-radius: 50%;

  cursor: pointer;
  transition: 0.3s;
}

.close-btn:hover {
  background: rgba(0,255,200,0.2);
}
.navbar {
  position: absolute;
  top: 20px;
  left: 0;
  width: 100%;

  display: flex;
  justify-content: space-between;
  align-items: center;

  padding: 0 40px;

  color: white;
  z-index: 10;
}

.nav-links {
  display: flex;
  gap: 20px;
}

.nav-links a {
  text-decoration: none;
  color: white;
  opacity: 0.7;
  transition: 0.3s;
}

.nav-links a:hover {
  opacity: 1;
}

.logo {
  font-weight: bold;
  font-size: 18px;
}

.nav-links {
  display: flex;
  gap: 25px;
}

.nav-links a {
  text-decoration: none;
  color: white;
  opacity: 0.7;
  transition: 0.3s;
}

.nav-links a:hover {
  opacity: 1;
}


.title {
  font-size: 20px;
  margin-bottom: 15px;
}

.card {
  margin-bottom: 15px;
  padding: 12px;
  background: rgba(255,255,255,0.05);
  border-radius: 12px;
}

.card-header {
  display: flex;
  align-items: center;
  gap: 8px;
  margin-bottom: 6px;
}

.icon {
  width: 18px;
  height: 18px;
}

.company {
  font-weight: bold;
}

.period {
  font-size: 12px;
  opacity: 0.7;
}



.back-btn {
  margin-top: 15px;
  display: flex;
  align-items: center;
  gap: 8px;
  background: rgba(255,255,255,0.1);
  padding: 10px;
  border-radius: 10px;
  cursor: pointer;
}

.globe {
  z-index: 0;
}

.navbar {
  z-index: 20;
}

</style>

<script setup>
import { onMounted, ref } from "vue"
import * as topojson from "topojson-client"
import { countriesData } from "../data/experiences"
import Globe from "globe.gl"
import { ArrowLeft, Trophy, Briefcase } from "lucide-vue-next"


const selectedCountry = ref(null)
const globeContainer = ref(null)
let world


const resetView = () => {
  selectedCountry.value = null

  /* world.controls().autoRotate = true

  world.pointOfView(
    { lat: 20, lng: 0, altitude: 2 },
    1000
  ) */
}

onMounted(async () => {
  world = Globe()(globeContainer.value)
    .globeImageUrl("//unpkg.com/three-globe/example/img/earth-dark.jpg")
    .backgroundImageUrl("//unpkg.com/three-globe/example/img/night-sky.png")

  world.pointOfView({ lat: 20, lng: 0, altitude: 2 })

  // 🔥 Charger les pays
  const res = await fetch("/data/countries.json")
  const data = await res.json()

  // Convertir en GeoJSON lisible
  const countries = topojson.feature(data, data.objects.countries).features

  const arcsData = [
    { startLat: 46, startLng: 2, endLat: 56, endLng: -106 }, // France → Canada
    { startLat: 56, startLng: -106, endLat: 7.5, endLng: -5.5 }, // Canada → CI
    ]
    
    world
    .arcsData(arcsData)
    .arcColor(() => "cyan")
    .arcDashLength(0.4)
    .arcDashGap(0.2)
    .arcDashAnimateTime(2000)
    .polygonsData(countries)
    .polygonCapColor((d) => {
        const country = countriesData.find(
            (c) => c.name === d.properties.name
        )

        if (country) {
            return "rgba(0, 255, 200, 0.8)" // 🔥 glow vert/bleu
        }

        return "rgba(80, 80, 80, 0.05)"
    })
    .polygonSideColor(() => "rgba(0, 100, 200, 0.1)")
    .polygonStrokeColor(() => "#111")
    .polygonAltitude((d) => {
        const isVisited = countriesData.find(
            (c) => c.name === d.properties.name
        )
        return isVisited ? 0.04 : 0.01
    })

    // Hover
    .onPolygonHover((hoverD) => {
        world
            .polygonAltitude((d) =>
            d === hoverD ? 0.05 : 0.01
        )
    })

    // Click
    .onPolygonClick((polygon) => {
        const countryName = polygon.properties.name

        const countryData = countriesData.find(
            (c) => c.name === countryName
        )

        if (!countryData) return

        // 🔥 STOP rotation auto
        world.controls().autoRotate = false

        selectedCountry.value = countryData

        world.pointOfView(
            {
            lat: countryData.lat,
            lng: countryData.lng,
            altitude: 0.6
            },
            1000
        )
    })

    
})
</script>