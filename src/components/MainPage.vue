<template>
  <q-page class="">
    <div
     class="full-width q-pa-sm q-gutter-md fixed text-center "
      style="z-index: 200;"
    >
     <q-btn color="red"  label="Get Location" v-on:click="indexYourLocation"/>
</div>
    <div class="full-width">
      <div id="mapCanvas"></div>
    </div>

  </q-page>

</template>

<script>
import 'leaflet-routing-machine/dist/leaflet-routing-machine.js'
import 'leaflet-routing-machine/dist/leaflet-routing-machine.css'
export default {
  name: 'app',
  data () {
    return {
      drawer: true,
      miniState: true,
      map: '',
      text: '',
      geoJsonText: '',
      point: '',
      control:null,
      myLines: [],
      myLinesString: [],
      
      polygonCoords: [],
      polylineCoords: [],
      baseLayerGroup: new L.layerGroup(),
      layerGroupCentral: new L.layerGroup(),

      layerGroupType: new L.layerGroup(),
      createdGeoElements: "",
     
      defaultIcon: "",
      popUpContentMarker: '',
      popUpOptions: {
        autoPan: true,
        autoClose: true,
        'className': 'custom-popup'
      },
      
    }
  },
  components: {

  },

  computed: {
    localTiles5To6: function () {
      return 'https://maptrack.in/tiles1588/5To6Ind/{z}/{x}/{y}.png'
    },
    localTiles7To10: function () {
      return 'https://maptrack.in/tiles1588/7To10Ind/{z}/{x}/{y}.png'
    },
    localTiles11To15: function () {
      return 'https://maptrack.in/tiles1588/11To15Go/{z}/{x}/{y}.png'
    },
    localTiles16To17: function () {
      return 'https://maptrack.in/tiles1588/16To17Go/{z}/{x}/{y}.png'
    },
    localTiles18Panaji: function () {
      return 'https://maptrack.in/tiles1588/18Panaji/{z}/{x}/{y}.png'
    },
  },
  methods: {

    initMap () {
      var self = this

      var baseLayer = self.getBaseMap()

      self.map = L.map('mapCanvas', {
        center: [15.464213, 73.849571],
        zoom: 10,
        minZoom: 5,
        maxZoom: 25,
        layers: [baseLayer],
        attributionControl: false,
        worldCopyJump: true
      })
      L.control.scale({ metric: true, imperial: false }).addTo(self.map)
      L.control.attribution({
        prefix: '<a href="http://leafletjs.com" title="A JS library for interactive maps" target="_blank">Leaflet</a> | 2020 © <a href="https://freethink.co.in/" target="_blank">freeTHINK(India)</a> | © <a href="http://osm.org/copyright" title="OpenStreetMap" target="_blank">OpenStreetMap</a> |  <a href="https://www.flaticon.com/auth title="A JS library for interactive maps" target="_blank">Leaflet</a> | 2020 © <a href="https://freethink.co.in/" target="_blank">freeTHINK(India)</a> | © <a href="https://www.flaticon.com/authors/freepik" title="Freepik">Freepik</a> from <a href="https://www.flaticon.com/" title="Flaticon"> Icon </a>'
      }).addTo(self.map)


      this.createdGeoElements = new L.featureGroup(this.layerGroupCentral).addTo(this.map)
      //Creating a custom icon
      


      var colored = L.tileLayer(
        'https://cartodb-basemaps-{s}.global.ssl.fastly.net/rastertiles/voyager/{z}/{x}/{y}.png',
        {
          maxZoom: 18,
          attribution: '&copy; <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a>, &copy; <a href="https://carto.com/attribution">CARTO</a>',
        }
      );

      var baseMaps = {
        "BaseMap": baseLayer,
        "Colored": colored
      };

      L.control.layers(baseMaps, null, { position: 'topleft' }).addTo(this.map);

L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
    attribution: '© OpenStreetMap contributors'
}).addTo(this.map);



this.control=L.Routing.control({
    waypoints: [
   
    ],
    routeWhileDragging: false,
     geocoder: L.Control.Geocoder.nominatim()
})
this.control.addTo(this.map)
      //Render the geoJson data onto the map
      //this.addLayerToMap();
var self=this
this.map.on('click', function(e) {
    var container = L.DomUtil.create('div'),
        startBtn = self.createButton('Start from this location', container),
        destBtn = self.createButton('Go to this location', container);
L.DomEvent.on(startBtn, 'click', function() {
        self.control.spliceWaypoints(0, 1, e.latlng);
        self.map.closePopup();
    });
    L.DomEvent.on(destBtn, 'click', function() {
        control.spliceWaypoints(control.getWaypoints().length - 1, 1, e.latlng);
        self.map.closePopup();
    });
    L.popup()
        .setContent(container)
        .setLatLng(e.latlng)
        .openOn(self.map);
});
    },
    /***************************************************GeoJson-related Functions******************************************************/
    createButton(label, container) {
    var btn = L.DomUtil.create('button', '', container);
    btn.setAttribute('type', 'button');
    btn.innerHTML = label;
    return btn;
},
    getBaseMap: function () {
      var southWest5to6 = L.latLng(3.776559, 55.986328), northEast5to6 = L.latLng(36.456636, 104.501953), boundSet5to6 = L.latLngBounds(southWest5to6, northEast5to6)
      var southWest7to10 = L.latLng(4.653080, 67.763672), northEast7to10 = L.latLng(29.573457, 89.208984), boundSet7to10 = L.latLngBounds(southWest7to10, northEast7to10)
      var southWest11To15 = L.latLng(14.636739, 73.350220), northEast11To15 = L.latLng(15.911150, 74.437866), boundSet11To15 = L.latLngBounds(southWest11To15, northEast11To15)
      var southWest16To17 = L.latLng(14.881087, 73.666077), northEast16To17 = L.latLng(15.813396, 74.358215), boundSet16To17 = L.latLngBounds(southWest16To17, northEast16To17)

      var baseLayer = L.layerGroup([L.tileLayer(this.localTiles5To6, { maxZoom: 6, minZoom: 5, bounds: boundSet5to6 }),
      L.tileLayer(this.localTiles7To10, { maxZoom: 10, minZoom: 7, bounds: boundSet7to10 }),
      L.tileLayer(this.localTiles11To15, { maxZoom: 15, minZoom: 11, bounds: boundSet11To15 }),
      L.tileLayer(this.localTiles16To17, { maxZoom: 25, maxNativeZoom: 17, minZoom: 16, bounds: boundSet16To17 }),
      L.tileLayer(this.localTiles18Panaji, { maxZoom: 25, maxNativeZoom: 18, minZoom: 18 })])
      return baseLayer
    },
    getLocation(){
      this.map.locate({
        setView:true,
        enableHighAccuracy:true,
      })
},
  indexYourLocation(){
    var self=this
    this.getLocation()
    this.map.on('locationfound',function(e){
    self.map.stopLocate()
    
    self.control.spliceWaypoints(self.control.getWaypoints().length - 1, 1, e.latlng);
})
  },  
  
  },

  mounted () {
    this.initMap()
  }
}
</script>

<style>
#mapCanvas {
  z-index: 100;
  height: 100vh;
  position: relative;
}

textarea {
  background-color: #fff;
}
body {
  background-color: rgb(158, 158, 158);
}
.customButtonStyle {
  color: #fff;
  background-color: #720505;
}
.customText {
  color: rgb(104, 23, 9);
}
.customButton {
  color: #fff;
  background-color: #720505;
}
.customPaid {
  color: #fff;
  background-color: red;
}
.customUnpaid {
  color: #fff;
  background-color: green;
}
.carParking {
  background-image: url("https://image.flaticon.com/icons/png/512/51/51778.png");
  background-size: cover;
  width: 40px;
  height: 40px;
}
.busParking {
  background-image: url("https://cdn.iconscout.com/icon/premium/png-256-thumb/bus-1734816-1471755.png");
  background-size: cover;
  width: 40px;
  height: 40px;
}
.truckParking {
  background-image: url("https://www.iconfinder.com/data/icons/eldorado-transport/40/truck_1-512.png");
  background-size: cover;
  width: 40px;
  height: 40px;
}
.taxiParking {
  background-image: url("https://www.iconfinder.com/data/icons/car-11/100/taxi3-512.png");
  background-size: cover;
  width: 40px;
  height: 40px;
}
.custom-popup .leaflet-popup-content-wrapper {
  background: whitesmoke;
  color: #fff;
  font-size: 16px;
  /* line-height: 24px; */
}
.custom-popup .leaflet-popup-content-wrapper a {
  color: whitesmoke;
}

.leaflet-control-zoom {
  transform: translateY(-50%);
  top: 320px;
  bottom: 0;
}

.leaflet-touch .leaflet-bar {
  border: none;
  background-clip: padding-box;
}
.leaflet-bottom {
  bottom: 25px;
}
.leaflet-div-icon {
  background: radial-gradient(
    circle,
    rgb(238, 229, 233) 0%,
    rgba(80, 78, 78, 0.778) 81%
  );
  border: none;
  border-radius: 10px;
}

.sidebar-list q-btn {
  width: 100%;
}
</style>

<style lang="stylus" scoped>
.legend {
  position: absolute;
  left: 0px;
  margin-top: 35vh;
  width: 160px;
}

.legendlist {
  background: rgba(210, 146, 133, 0.7);
}

.icon {
  height: 20px;
  width: 20px;
}

.customButtonStyle {
  background-color: $accent;
}

.customStyleCard {
  top: 20px;
  height: 100px;
  background: rgba(210, 146, 133, 0.7);
}

.closeButton {
  float: center;
}

.center-contents {
  text-align: center;
}

.space {
  margin: 10px 15px;
}

.card_bg {
  background: rgba(210, 146, 133, 0.5);
}
</style>