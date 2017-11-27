<template>
  <div id="app">
    <header>
     <h1>Vozilla</h1>
    </header>

    <CarList @list-car-selected="handleListSelection" :selected-car="selectedCar" :list="list.objects"/>
    <CarMap @map-car-selected="handleMapSelection" :list="list.objects"/>
  </div>
</template>

<script>
import CarList from './components/CarList'
import CarMap from './components/CarMap'


export default {
  name: 'app',
  components: {
    CarList,
    CarMap,
  },
  data() {
    return {
      list: [],
      selectedCar: null,
    }
  },
  mounted: function() {
    this.getCarList().then((list) => {
      this.list = list 
      this.selectedCar = this.list.objects[3];
      
      //change value intentionally to check unavailable marker
      this.list.objects[0].status = 'UNAVAILABLE';   
      this.$emit('updateMap', this.selectedCar); 
      console.log(this.list);
    });
  },
  methods: {
    async getCarList() {
      const API_URL = 'https://test.vozilla.pl/api-client-portal/';
      const API_CARLIST = 'map?objectType=VEHICLE&vehicleStatus=AVAILABLE';
      
      const res = await this.axios.get(API_URL+API_CARLIST)
        .then(response => (response.data))
        .catch(error => (error))

      return res;
    },
    handleListSelection(car) {
      this.selectedCar = car;
      this.$emit('updateMap', this.selectedCar);
    },
    handleMapSelection(car) {
      this.selectedCar = car;
    }
  },
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}
header {
  display: flex;
  width:100%;
  justify-content: center;
}
h1 {
  font-size:4em;
}
img {
  width: auto;
  margin-bottom: 50px;
}

</style>
