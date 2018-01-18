<template>
  <div class="google-map fl w-70 pa2" id="example-map"></div>
</template>
<script>
export default {
  props: ["selected"],
  data() {
    return {
      map: null,
      bounds: null,
      markers: [],
      mapData: []
    };
  },
  methods: {
    compileMarkerCoordinates() {
      let url =
        `https://api-us.hockey-community.com/v1/explore/locations?category=${this.selected}&sw=49.1407,-123.0899&ne=49.3261,-122.9310&origin=49.2335,-123.0104`;
      this.$http.get(url).then(
        function(response) {
          console.log("SUCCESS GOOGLEMAPS", response);
          this.mapData = response.data;

          let markerCoordinatesCompiled = [];

          console.log(this.outdoorRinkData);
          console.log(this.indoorRinkData);

          console.log("SELECTEDDDDD", this.selected);

          if (this.selected === "outdoor_rinks") {
            this.outdoorRinkData.forEach(element => {
              markerCoordinatesCompiled.push({
                latitude: element.latitude,
                longitude: element.longitude,
                name: element.name,
                friendlyLocation: element.friendly_location
              });
            });
          }

          if (this.selected === "indoor_rinks") {
            this.mapData.forEach(element => {
              markerCoordinatesCompiled.push({
                latitude: element.latitude,
                longitude: element.longitude,
                name: element.name,
                address: element.address,
                friendlyLocation: element.friendly_location,
                phone: element.phone
              });
            });
          }

          this.bounds = new google.maps.LatLngBounds();
          const element = document.getElementById("example-map");
          const mapCenter = markerCoordinatesCompiled[0];
          const options = {
            center: new google.maps.LatLng(
              mapCenter.latitude,
              mapCenter.longitude
            )
          };
          this.map = new google.maps.Map(element, options);
          markerCoordinatesCompiled.forEach(coord => {
            const position = new google.maps.LatLng(
              coord.latitude,
              coord.longitude
            );

            const infowindow = new google.maps.InfoWindow({
              content: `<h4>${coord.name}</h4>
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
        },
        function(error) {
          console.log("ERROR", error);
        }
      );
    }
  },
  mounted: function() {
    this.compileMarkerCoordinates();
  },
  updated: function(){
    this.compileMarkerCoordinates();
  }
};
</script>
<style scoped>
.google-map {
  background: gray;
}
</style>