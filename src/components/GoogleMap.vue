<template>
  <div class="map-wrapper">
    <div id="google-map" ref="mapRef"></div>
  </div>
</template>

<script>
import { Loader } from "@googlemaps/js-api-loader";

export default {
  name: "GoogleMap",
  props: {
    potholes: {
      type: Array,
      default: () => [
        {
          id: "A",
          coordinate: "123,23",
          severity: "LOW",
          location: "Near Flamingo",
        },
        {
          id: "B",
          coordinate: "123,23",
          severity: "LOW",
          location: "Nandankanan Rd",
        },
        {
          id: "C",
          coordinate: "123,23",
          severity: "MEDIUM",
          location: "Red Town",
        },
      ],
    },
  },
  data() {
    return {
      map: null,
      markers: [],
      center: { lat: 20.360236, lng: 85.8269268 },
    };
  },
  mounted() {
    this.initializeMap();
  },
  methods: {
    async initializeMap() {
      try {
        const loader = new Loader({
          apiKey: "AIzaSyDkJ_56WwyYTu-F-WxBHeyq6PHzi2bSzjE",
          version: "weekly",
          libraries: ["places"],
        });

        const google = await loader.load();

        this.map = new google.maps.Map(this.$refs.mapRef, {
          center: this.center,
          zoom: 15,
          styles: [
            {
              featureType: "poi",
              elementType: "labels",
              stylers: [{ visibility: "off" }],
            },
          ],
        });


        this.addMarkers();

        this.addSearchBox(google);
      } catch (error) {
        console.error("Error loading Google Maps:", error);
      }
    },

    addMarkers() {
      const google = window.google;

      this.markers.forEach((marker) => marker.setMap(null));
      this.markers = [];
      this.potholes.forEach((pothole) => {
        const marker = new google.maps.Marker({
          position: this.center,
          map: this.map,
          title: `Pothole ${pothole.id}`,
          icon: {
            path: google.maps.SymbolPath.CIRCLE,
            scale: 8,
            fillColor: this.getSeverityColor(pothole.severity),
            fillOpacity: 0.8,
            strokeWeight: 1,
            strokeColor: "#ffffff",
          },
        });

        const infoWindow = new google.maps.InfoWindow({
          content: `
            <div class="info-window">
              <h3>Pothole ${pothole.id}</h3>
              <p>Location: ${pothole.location}</p>
              <p>Severity: ${pothole.severity}</p>
            </div>
          `,
        });

        marker.addListener("click", () => {
          infoWindow.open(this.map, marker);
        });

        this.markers.push(marker);
      });
    },

    addSearchBox(google) {
      const input = document.createElement("input");
      input.className = "map-search-box";
      input.placeholder = "Search location...";

      this.map.controls[google.maps.ControlPosition.TOP_LEFT].push(input);

      const searchBox = new google.maps.places.SearchBox(input);

      searchBox.addListener("places_changed", () => {
        const places = searchBox.getPlaces();

        if (places.length === 0) return;

        const bounds = new google.maps.LatLngBounds();
        places.forEach((place) => {
          if (!place.geometry || !place.geometry.location) return;

          if (place.geometry.viewport) {
            bounds.union(place.geometry.viewport);
          } else {
            bounds.extend(place.geometry.location);
          }
        });

        this.map.fitBounds(bounds);
      });
    },

    getSeverityColor(severity) {
      const colors = {
        LOW: "#FFC107",
        MEDIUM: "#FF9800",
        HIGH: "#F44336",
      };
      return colors[severity] || "#FFC107";
    },
  },
};
</script>

<style scoped>
.map-wrapper {
  width: 100%;
  height: 100%;
  position: relative;
}

#google-map {
  width: 100%;
  height: 100%;
  border-radius: 8px;
}

:deep(.map-search-box) {
  margin: 10px;
  padding: 10px;
  width: 300px;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.3);
  font-size: 14px;
  outline: none;
}

:deep(.info-window) {
  padding: 5px;
}

:deep(.info-window h3) {
  margin: 0 0 5px 0;
  color: #333;
}

:deep(.info-window p) {
  margin: 2px 0;
  color: #666;
}
</style>
