<template>
  <view class="container">
    <MapView class="container"
            v-if="loaded"
            :initialRegion="userLocation"
            :region="getSearchedLocation" >
      <Marker v-for="marker in markers"
              v-bind:key="marker.id"
              :coordinate="marker.coordinates"
              :title="marker.title"
              :description="marker.description" />
    </MapView>
    <text-input v-model="searchedAddress"
                class="searchedAddressInput"
                placeholder="Search here"
                :onSubmitEditing="searchLocation" />
  </view>
</template>

<script>
import MapView from "react-native-maps";
import { Marker } from 'react-native-maps';
import * as Location from "expo-location";
import * as Permissions from 'expo-permissions';

GOOGLE_MAPS_API_KEY="secret"


export default {
  beforeMount: function() {
    this.getLocation();
  },
  data: function() {
    return {
      loaded: false,
      markers: [
        { id: 1, title: "Ubicación 1", description: "Esta es una descripción", coordinates: {latitude: -34.724609, longitude: -58.260281} },
        { id: 2, title: "Ubicación 2", description: "Esta es una descripción", coordinates: {latitude: -34.824609, longitude: -58.360281} },
        { id: 3, title: "Ubicación 3", description: "Esta es una descripción", coordinates: {latitude: -34.924609, longitude: -58.460281} },
        { id: 4, title: "Ubicación 4", description: "Esta es una descripción", coordinates: {latitude: -34.100609, longitude: -58.560281} },
      ],
      userLocation: {
        latitude: null,
        longitude: null,
        latitudeDelta: 0.0922,
        longitudeDelta: 0.0421,
      },
      searchedAddress: '',
      searchedCoords: {},
    };
  },
  computed: {
    getSearchedLocation: function() {
      const result = {
        latitudeDelta: this.userLocation.latitudeDelta,
        longitudeDelta: this.userLocation.longitudeDelta,
        latitude: this.searchedCoords.lat,
        longitude: this.searchedCoords.lng,
      };

      return this.searchedCoords.lat ? result : null;
    }
  },
  components: {
    MapView,
    Marker,
  },
  methods: {
    getLocation: function() {
      Permissions.getAsync(Permissions.LOCATION).then(userPermission => {
        if (userPermission.status !== "granted") { return; }

        Location.getCurrentPositionAsync({}).then(location => {
          this.userLocation.latitude = location.coords.latitude;
          this.userLocation.longitude = location.coords.longitude;
          this.loaded = true;
        });
      }).catch((err)=>{
        console.log(err);
     });
    },
    searchLocation: function() {
      fetch(`https://maps.googleapis.com/maps/api/geocode/json?&address=${this.searchedAddress}&key=${GOOGLE_MAPS_API_KEY}`)
        .then((response) => response.json()).then(data => {
          this.searchedCoords = data['results'][0]['geometry']['location']
        })
        .catch((error) => console.log(error))
    }
  }
};
</script>

<style>
.container {
  flex: 1;
}
.searchedAddressInput {
  padding-left: 5px;
  border-radius: 5px;
  border-color: #cecece;
  border-style: solid;
  border-width: 1px;
  width: 80%;
  position: absolute;
  top: 10%;
  left: 10%;
  z-index: 9999;
  background-color: white;
}
</style>