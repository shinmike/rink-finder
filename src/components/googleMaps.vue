<template>
  <div class="google-map fl w-60 pa2" v-bind:id="mapName"></div>
</template>
<script>
export default {
  name: "google-map",
  props: ["outdoorRinkData", "indoorRinkData", "name"],
  data: function() {
    var markerCoordinatesCompiled = [];

    if (this.outdoorRinkData) {
      this.outdoorRinkData.forEach(element => {
        markerCoordinatesCompiled.push({
          latitude: element.latitude,
          longitude: element.longitude,
          name: element.name,
          friendlyLocation: element.friendly_location,
        });
      });
    }

    // console.log()

    if (this.indoorRinkData) {
      this.indoorRinkData.forEach(element => {
        markerCoordinatesCompiled.push({
          latitude: element.latitude,
          longitude: element.longitude,
          name: element.name,
          address: element.address,
          friendlyLocation: element.friendly_location,
          phone: element.phone,
        });
      });
    }

    return {
      mapName: this.name + "-map",
      markerCoordinates: markerCoordinatesCompiled,
      map: null,
      bounds: null,
      markers: []
    };
  },
  mounted: function() {
    this.bounds = new google.maps.LatLngBounds();
    const element = document.getElementById(this.mapName);
    const mapCenter = this.markerCoordinates[0];
    const options = {
      center: new google.maps.LatLng(mapCenter.latitude, mapCenter.longitude)
    };
    this.map = new google.maps.Map(element, options);
    this.markerCoordinates.forEach(coord => {
      const position = new google.maps.LatLng(coord.latitude, coord.longitude);

      const infowindow = new google.maps.InfoWindow({
        content: 
        `<h4>${coord.name}</h4>
        <hr>
        <p>${coord.address}</p>
        <p>${coord.friendlyLocation}</p>
        <p>${coord.phone}</p>
        `
      });
      const marker = new google.maps.Marker({
        position,
        map: this.map
      });
      marker.addListener("click", function() {
        infowindow.open(this.map, marker);
      });
      this.markers.push(marker);
      this.map.fitBounds(this.bounds.extend(position));
    });
  }
};
</script>
<style scoped>
.google-map {
  background: gray;
}
</style>