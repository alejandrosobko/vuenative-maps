<template>
  <view  class="container">
    <MapView class="container" :initial-region="coordinates">
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


export default {
  data: function() {
    return {
      markers: [
        { id: 1, title: "Ubicación 1", description: "Esta es una descripción", coordinates: {latitude: -34.724609, longitude: -58.260281} },
        { id: 2, title: "Ubicación 2", description: "Esta es una descripción", coordinates: {latitude: -34.824609, longitude: -58.360281} },
        { id: 3, title: "Ubicación 3", description: "Esta es una descripción", coordinates: {latitude: -34.924609, longitude: -58.460281} },
        { id: 4, title: "Ubicación 4", description: "Esta es una descripción", coordinates: {latitude: -34.100609, longitude: -58.560281} },
      ],
      coordinates: {
        latitude: -34.724609,
        longitude: -58.260281,
        latitudeDelta: 0.0922,
        longitudeDelta: 0.0421
      },
    };
  },
  components: {
    MapView,
    Marker,
  }
};
</script>

<style>
.container {
  flex: 1;
}
</style>