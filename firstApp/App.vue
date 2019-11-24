<template>
  <view  class="container">
    <MapView class="container" :initial-region="userLocation" v-if="loaded">
      <Marker v-for="marker in markers"
              v-bind:key="marker.id"
              :coordinate="marker.coordinates"
              :title="marker.title"
              :description="marker.description" />
    </MapView>
  </view>
</template>

<script>
import MapView from "react-native-maps";
import { Marker } from 'react-native-maps';
import * as Location from "expo-location";
import * as Permissions from 'expo-permissions';

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
        latitudeDelta: 0.0922,
        longitudeDelta: 0.0421,
      },
    };
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
    }
  }
};
</script>

<style>
.container {
  flex: 1;
}
</style>