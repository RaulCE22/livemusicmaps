<template>
  <div>
    <l-map ref="map" :zoom=16 :center="center" style="height: 300px">>
        <l-tile-layer :url="url" :attribution="attribution"/>
        <div v-for="item in markers" >
        <l-marker 
        :key="item.key" 
        :lat-lng="item.position" 
        :visible="true" 
        v-on:l-click="onMarkerClick(item)"
        >
        </l-marker>
        <l-circle
        :lat-lng="item.position"
        :radius="item.circle"></l-circle>
        </div>
      
  </l-map>
  </div>
</template>

<script>
// @ is an alias to /src
import { LMap, LTileLayer, LMarker, LCircle} from "vue2-leaflet";

export default {
  name: "home",
  data: () => {
    return {
      map: null,
      center: [40,-1],
      url: "http://{s}.tile.osm.org/{z}/{x}/{y}.png",
      attribution:
        '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors',
      markers: []

    };
  },
  components: {
    LMap,
    LTileLayer,
    LMarker,
    LCircle
  },
  methods: {
    addCenter:  function(positions) {
      this.center = [positions.coords.latitude, positions.coords.longitude];
      this.markers = [{key: 1, position: this.center, circle: positions.coords.accuracy}]
      console.log(this.center, positions);
    },
    getPosition: function() {
      navigator.geolocation.getCurrentPosition( data => this.addCenter(data), err => console.error(err));
    },
    onMarkerClick: function(item) {
      alert(item);
    }
  },
  created() {
    this.getPosition();
  }
};
</script>
