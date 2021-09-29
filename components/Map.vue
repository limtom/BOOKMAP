/* eslint-disable no-console */ /* eslint-disable no-console */
<template>
  <client-only>
    <l-map
      ref="map"
      style="height: 90vh"
      :zoom="zoom"
      @ready="getLocation"
      @locationfound="locationFound($event)"
      @locationerror="locationError"
      @popupopen="getPopup"
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
      <v-locatecontrol></v-locatecontrol>
      <l-control position="topleft">
        <v-btn
          class="pa-1"
          color=""
          outlined
          max-width="35"
          max-height="35"
          @click="attachMapEvent"
        >
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
      zoom: 12,
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
      addBookmarkStatus: false,
      temp: `<form id="comment-form" style="
        width: 300px;" class="d-flex flex-column">
        <label for="fname" class="text-subtitle-2 mb-1">Title</label>
        <input type="text" placeholder="input first name" name="fname" style="height: 40px;
        font-size: 16px;
        padding-left: 12px;
        padding-right: 12px;
        margin-bottom: 5px;
        border:1px solid;" class="rounded text-caption">
        <label for="description" class="text-subtitle-2 mb-0 mt-2">Description</label>
        <textarea name="description" placeholder="Enter place description" id="" cols="30" rows="3" style="height: 100px;
        font-size: 16px;
        padding-left: 12px;
        padding-top: 4px;
        padding-bottom: 4px;
        padding-right: 12px;
        margin-bottom: 20px;
        border:1px solid;" class="rounded text-caption"></textarea>
        <button type="submit" class="v-btn success pa-1 font-weight-bold" style="width:100px; height:35px; background-color:white; border:1px solid green;">Submit</button> 
    </form>`,
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

    attachMapEvent() {
      // Attach an event listener on the map it self
      this.$refs.map.mapObject.addEventListener('click', (event) => {
        // Create a marker at the point of click
        const marker = this.$L.marker(Object.values(event.latlng))
        // Add the marker to the map
        this.$refs.map.mapObject.addLayer(marker)
        marker.bindPopup(this.temp)
        this.$refs.map.mapObject.removeEventListener('click')
      })
    },

    getPopup(event) {
      // Get the pop up element
      const popUpElement = event.popup.getElement()
      // Get the id of the form
      const form = popUpElement.querySelector('#comment-form')
      // Attach an event to the form
      form.addEventListener('submit', (e) => {
        // Get form data using form data
        const data = new FormData(form)
        const value = Object.fromEntries(data.entries())
        event.popup.removeFrom(this.$refs.map.mapObject)
        console.log(value)
        e.preventDefault()
      })
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

.leaflet-popup-content-wrapper {
  min-width: 500 !important;
}

#text-field {
  height: 40px !important;
}
form {
  padding: 12px;
  width: 400px;
  display: flex;
  flex-direction: column;
}
label {
  font-size: 16px;
}
input {
  height: 30px;
  font-size: 16px;
  padding-left: 4px;
  padding-right: 4px;
  margin-top: 4px;
  margin-bottom: 20px;
}

textarea {
  height: 100px;
  font-size: 16px;
  padding-left: 4px;
  padding-right: 4px;
  margin-top: 4px;
  margin-bottom: 10px;
}

button {
  height: 35px;
  font-size: 16px;
  width: 100px;
}
</style>
