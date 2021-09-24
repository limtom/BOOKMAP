/* eslint-disable no-console */
<template>
  <client-only>
    <l-map
      ref="map"
      style="height: 90vh"
      :zoom="zoom"
      @ready="getLocation"
      @locationfound="locationFound($event)"
      @locationerror="locationError"
    >
      <l-control-layers
        position="topright"
        :collapsed="false"
      ></l-control-layers>
      <l-tile-layer
        v-for="tileProvider in tileProviders"
        :key="tileProvider.name"
        :name="tileProvider.name"
        :visible="tileProvider.visible"
        :url="tileProvider.url"
        :attribution="tileProvider.attribution"
        layer-type="base"
      />
      <l-marker v-if="locationReady" :lat-lng="userLocation"></l-marker>
      <v-locatecontrol></v-locatecontrol>
      <l-control position="topleft">
        <v-btn class="pa-1" color="" outlined max-width="35" max-height="35">
          <v-icon small color="#444" class="">mdi-pin</v-icon>
        </v-btn>
      </l-control>
    </l-map>
  </client-only>
</template>

<script>
export default {
  components: {},
  data() {
    return {
      url: 'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',
      zoom: 8,
      center: null,
      userLocation: [],
      tileProviders: [
        {
          name: 'OpenStreetMap',
          visible: true,
          attribution:
            '&copy; <a target="_blank" href="http://osm.org/copyright">OpenStreetMap</a> contributors',
          url: 'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',
        },
        {
          name: 'OpenTopoMap',
          visible: false,
          url: 'https://{s}.tile.opentopomap.org/{z}/{x}/{y}.png',
          attribution:
            'Map data: &copy; <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a>, <a href="http://viewfinderpanoramas.org">SRTM</a> | Map style: &copy; <a href="https://opentopomap.org">OpenTopoMap</a> (<a href="https://creativecommons.org/licenses/by-sa/3.0/">CC-BY-SA</a>)',
        },
        {
          name: 'Imagery',
          visible: false,
          url: 'http://server.arcgisonline.com/ArcGIS/rest/services/World_Imagery/MapServer/tile/{z}/{y}/{x}',
          attribution:
            'Map data: &copy;<a href="http://www.esri.com/">Esri</a>, i-cubed, USDA, USGS, AEX, GeoEye, Getmapping, Aerogrid, IGN, IGP, UPR-EGP, and the GIS User Community)',
        },
      ],
      locationReady: false,
    }
  },
  methods: {
    getLocation() {
      this.$nextTick(() => {
        this.$refs.map.mapObject.locate({
          setView: true,
          enableHighAccuracy: true,
        })
      })
    },

    locationFound(event) {
      this.$nextTick(() => {
        // Set the location status and extract the position information
        this.locationReady = true
        this.userLocation = Object.values(event.latlng)
        // eslint-disable-next-line no-console
        console.log(this.userLocation)

        // Emit an event to send to the parent
        this.$emit('locationReady', {
          locationStatus: true,
          userLocation: this.userLocation,
        })
      })
    },

    locationError(event) {
      //   console.log(event.message)
    },
  },
}
</script>
<style lang="css" scoped>
@import 'https://maxcdn.bootstrapcdn.com/font-awesome/4.7.0/css/font-awesome.min.css';

.v-btn {
  background-color: white;
  min-width: 35px !important;
  border: 2px solid rgb(185, 183, 183);
}
</style>
