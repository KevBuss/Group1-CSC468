<template>
  <div class="placeholder">
    <input type="text" id="start" placeholder="Enter your origin" />
    <input type="text" id="end" placeholder="Enter your destination" />
    <button type="button" @click="calculateAndDisplayRoute">Get Directions</button>
    <div id="directions-panel"></div>
  </div>
</template>

<script>
/* global google */
export default {
  name: 'RouteSuggestionsComponent',
  data() {
    return {
      directionsService: null,
    };
  },
  mounted() {
    this.directionsService = new google.maps.DirectionsService();
    const options = {
      types: ['(cities)'],
    };
    const inputStart = document.getElementById('start');
    if (inputStart) {
      new google.maps.places.Autocomplete(inputStart, options);
    }
    const inputEnd = document.getElementById('end');
    if (inputEnd) { 
      new google.maps.places.Autocomplete(inputEnd, options);
    }
  },
  methods: {
    renderTurnByTurnDirections(steps) {
      const directionsPanel = document.getElementById('directions-panel');
      directionsPanel.innerHTML = '';
      steps.forEach((step) => {
        const instruction = document.createElement('div');
        instruction.innerHTML = step.instructions;
        directionsPanel.appendChild(instruction);
        const distance = document.createElement('div');
        distance.innerHTML = step.distance.text;
        directionsPanel.appendChild(distance);
      });
    },
    calculateAndDisplayRoute() {
      const startInput = document.getElementById('start');
      const endInput = document.getElementById('end');
      const start = startInput ? startInput.value : '';
      const end = endInput ? endInput.value : '';
      if (!start || !end) {
        alert('Please enter both origin and destination!');
        return;
      }
      this.directionsService
        .route({
          origin: start,
          destination: end,
          travelMode: google.maps.TravelMode.DRIVING,
          unitSystem: google.maps.UnitSystem.IMPERIAL,
        })
        .then((response) => {
          this.$emit('directionsCalculated', response);
          this.renderTurnByTurnDirections(response.routes[0].legs[0].steps);
        })
        .catch((e) => {
          console.error('Directions request failed:', e);
          alert('Directions request failed. Please check the console log for more details.');
        });
    },
  },
};
</script>

<style>
#directions-panel {
  height: 400px;
  width: 35%;
  float: right;
  overflow-y: auto;
  border: 2px solid #ccc;
  margin: 10px;
  padding: 20px;
  text-align: center;
}
</style>
