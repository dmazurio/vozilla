<template>
    <div class="mapContainer">
        
        <gmap-map
        :center="changingCenter"
        :zoom="mapZoom"
        id="carMap"
        map-type-id="terrain"
        :style="mapStyle"
        >
        <gmap-info-window :options="popup.options" :position="popup.position" :opened="popup.open" @closeclick="popup.open=false">
        <span v-html="popup.content"></span>
        </gmap-info-window>

        <gmap-cluster>
            <gmap-marker
            :key="index"
            v-for="(m, index) in list"
            :position="{lat:parseFloat(m.location.latitude),lng: parseFloat(m.location.longitude)}"
            :clickable="true"
            :title="'Hello'"
            :draggable="true"            
            :icon="getMarkerIcon(m)"
            @click="markerAction(m)"
            ></gmap-marker>
        </gmap-cluster>
        </gmap-map>  
    </div>
</template>

<script>
export default {
  name: 'CarMap',
  props: ['list'],
  data () {    
    return {
        mapStyle: {
            width: '70vw',
            height: '560px',
            fullWidth: '98vw',
            normalWidth: '70vw',
            maxWidth: '1000px',
            fullBreakpoint: 900
        },
        markerIcon: {
            path: 'M480 1088q0-66-47-113t-113-47-113 47-47 113 47 113 113 47 113-47 47-113zm36-320h1016l-89-357q-2-8-14-17.5t-21-9.5h-768q-9 0-21 9.5t-14 17.5zm1372 320q0-66-47-113t-113-47-113 47-47 113 47 113 113 47 113-47 47-113zm160-96v384q0 14-9 23t-23 9h-96v128q0 80-56 136t-136 56-136-56-56-136v-128h-1024v128q0 80-56 136t-136 56-136-56-56-136v-128h-96q-14 0-23-9t-9-23v-384q0-93 65.5-158.5t158.5-65.5h28l105-419q23-94 104-157.5t179-63.5h768q98 0 179 63.5t104 157.5l105 419h28q93 0 158.5 65.5t65.5 158.5z',
            scale: 0.02,
            strokeWeight: 0.2,
            strokeColor: 'black',
            strokeOpacity: 0,
            fillColor: '#f8ae5f',
            fillOpacity: 1,
        },
        changingCenter: {
            lat:0, lng:0
        },
        mapZoom: 10,
        popup: {
            open: false,
            content: null,
            title: null,
            options: {
                pixelOffset: {
                        width:20,
                        height: 0
                }                
            },
            position: {},            
        }
    }
  },
  methods: {
      getMarkerIcon(m) {
          
          let icon = Object.assign({},this.markerIcon);

          if(m.status == 'AVAILABLE') {
              icon.fillColor = '#53b267';
          }
          else {              
              icon.fillColor = '#d9232a'              
          }

          return icon ;
      },
      markerAction(m) {
          this.popup.position = {lat: parseFloat(m.location.latitude), lng: parseFloat(m.location.longitude)}
          this.popup.content = `<div>
            <h2>${m.name}</h2>
            Rejestracja: ${m.platesNumber}<br/>
            Kolor: ${m.color}
          </div>`;
          this.popup.open = true;
          this.$emit('map-car-selected', m);
    },
    updateMap(m) {
        this.changingCenter.lat = parseFloat(m.location.latitude);
        this.changingCenter.lng = parseFloat(m.location.longitude);
        this.mapZoom = 14;
    }    
  },
  created: function() {
    this.$parent.$on('updateMap', this.updateMap);

    if(window.innerWidth <= this.mapStyle.fullBreakpoint) {
        this.mapStyle.width = this.mapStyle.fullWidth;
    }
   
    window.addEventListener('resize',()=> {
        if(window.innerWidth <= this.mapStyle.fullBreakpoint) {
            this.mapStyle.width = this.mapStyle.fullWidth;
        }
        else {
            this.mapStyle.width = this.mapStyle.normalWidth;
        }
    });

  }
}
</script>

<style scoped>
.mapContainer {
    display: flex;
}


</style>
