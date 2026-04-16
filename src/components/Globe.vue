<template>
  <div ref="globeContainer" class="globe-wrap"></div>
</template>

<script setup>
import { onMounted, ref, watch } from 'vue'
import * as topojson from 'topojson-client'
import { countriesData } from '../data/experiences'
import Globe from 'globe.gl'

const props = defineProps({ sidebarOpen: Boolean })
const emit = defineEmits(['country-click'])

const globeContainer = ref(null)
let world = null

watch(() => props.sidebarOpen, (open) => {
  if (!world) return
  world.controls().autoRotate = !open
})

onMounted(async () => {
  world = Globe()(globeContainer.value)
    .globeImageUrl('//unpkg.com/three-globe/example/img/earth-dark.jpg')
    .backgroundImageUrl('//unpkg.com/three-globe/example/img/night-sky.png')

  world.controls().autoRotate = true
  world.controls().autoRotateSpeed = 0.4
  world.pointOfView({ lat: 20, lng: 0, altitude: 2 })

  const res = await fetch('/data/countries.json')
  const data = await res.json()
  const countries = topojson.feature(data, data.objects.countries).features

  const arcsData = [
    { startLat: 46, startLng: 2, endLat: 56, endLng: -106 },
    { startLat: 56, startLng: -106, endLat: 7.5, endLng: -5.5 },
  ]

  world
    .arcsData(arcsData)
    .arcColor(() => 'rgba(0,255,200,0.6)')
    .arcDashLength(0.4)
    .arcDashGap(0.2)
    .arcDashAnimateTime(2000)
    .arcStroke(0.5)
    .polygonsData(countries)
    .polygonCapColor((d) => {
      const hit = countriesData.find(c => c.name === d.properties.name)
      return hit ? 'rgba(0, 255, 200, 0.75)' : 'rgba(80, 80, 80, 0.05)'
    })
    .polygonSideColor(() => 'rgba(0, 100, 200, 0.08)')
    .polygonStrokeColor(() => '#111')
    .polygonAltitude((d) => {
      const hit = countriesData.find(c => c.name === d.properties.name)
      return hit ? 0.04 : 0.01
    })
    .onPolygonHover((hoverD) => {
      world.polygonAltitude((d) => {
        const isVisited = countriesData.find(c => c.name === d.properties.name)
        if (d === hoverD && isVisited) return 0.07
        return isVisited ? 0.04 : 0.01
      })
      world.polygonCapColor((d) => {
        const isVisited = countriesData.find(c => c.name === d.properties.name)
        if (!isVisited) return 'rgba(80, 80, 80, 0.05)'
        return d === hoverD ? 'rgba(0, 255, 200, 1)' : 'rgba(0, 255, 200, 0.75)'
      })
    })
    .onPolygonClick((polygon) => {
      const countryData = countriesData.find(c => c.name === polygon.properties.name)
      if (!countryData) return

      world.controls().autoRotate = false
      world.pointOfView({ lat: countryData.lat, lng: countryData.lng, altitude: 0.6 }, 1000)
      emit('country-click', countryData)
    })
})
</script>

<style scoped>
.globe-wrap {
  position: fixed;
  inset: 0;
  z-index: 0;
}
</style>
