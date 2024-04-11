<template>
  <div class="map">
    <h3>Map</h3>
    <input type="text" v-model="apiKey" placeholder="Enter Google Maps API Key" />
    <button @click="initMap">Load Map</button>
    <div id="map"></div>
  </div>
</template>

<script>
/* global google */
export default {
  name: 'MapComponent',
  data() {
    return {
      apiKey: '',
      map: null,
      directionsRenderer: null,
    };
  },
  methods: {
    initMap() {
      if (this.apiKey) {
        const script = document.createElement('script');
        script.src = `https://maps.googleapis.com/maps/api/js?key=${this.apiKey}&libraries=places,directions`;
        script.onload = () => {
          const mapOptions = {
            zoom: 8,
            center: { lat: 39.9607, lng: -75.6055 }, // Default center
          };
          this.map = new google.maps.Map(document.getElementById('map'), mapOptions);
          this.directionsRenderer = new google.maps.DirectionsRenderer();
          this.directionsRenderer.setMap(this.map);
        };
        document.head.appendChild(script);
      } else {
        alert('Please enter a valid Google Maps API key.');
      }
    },
    renderDirections(directions) {
      this.directionsRenderer.setDirections(directions);
    },
  },
};
</script>

<style>
.placeholder {
  border: 2px solid #ccc;
  margin: 10px;
  padding: 20px;
  text-align: center;
}
</style>
