<template lang="html">
	<div id="sightingsGrid">
		<sighting v-for="(sighting, index) in sightings" :key="index" :sighting="sighting"/>
	</div>
</template>

<script>
import Sighting from './Sighting';
import SightingService from '../services/SightingService.js';
import { eventBus } from '../main.js';
export default {
	name: "sightings-grid",
	data () {
    return {
      sightings: []
    }
  },
	components: {
		'sighting': Sighting
	},
	mounted(){
    this.fetchData();


		eventBus.$on('bird-added', (bird) => {
      this.sightings.push(bird)
    });

		eventBus.$on('bird-deleted', (id) => {
      let index = this.sightings.findIndex(bird => bird._id === id)
      this.sightings.splice(index, 1)
    })
  },
  methods: {
    fetchData(){
      SightingService.getSightings()
      .then(sightings => this.sightings = sightings);
    }
  }

}
</script>

<style lang="css" scoped>
#sightingsGrid {
	display: flex;
	flex-wrap: wrap;
	justify-content: space-evenly;
}

h2 {
	padding: 0;
	margin: 0;
}


</style>
