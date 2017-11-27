<template>
  <div class="listContainer">
    
    <ul id="carList">
      <li :key="car.id" :class="{selected:(car == selectedCar)}" v-for="car in list">
        <a href="#" @click.prevent="selectCar(car)">{{car.name}}</a>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'CarList',
  props: ['list', 'selectedCar'],
  data () {    
    return {
     
    }
  },
  methods: {
    selectCar(car) {
      this.$emit('list-car-selected', car);
    },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>


.listContainer {
  display: flex;
  width: 28vw;
  min-width: 255px;
}

@media(max-width:900px) {
  .listContainer {
    order: 2;
    width: 100%;
  }

}

h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
  margin: 0;
  width:100%;
  text-align: left;
}

li {
  display: block;
  margin: 0px;
}

li.selected a {
  font-weight: bold;
  background: #5eb45f;
  color: #ffffff;
  border-left: .5rem solid #ffffff;
}

a {
  display: block;
  color: #396f57;
  text-decoration: none;
  background: #f3f5f4;
  padding: 20px;
  padding-left: 40px;
  text-transform: uppercase;
}

a:hover {
    background: #5eb45f;
    border-left: .5rem solid #ffffff;
    color: #ffffff;
    font-weight: bold;
  }

</style>
