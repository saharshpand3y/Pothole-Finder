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
      default: () => [],
    },
  },
  data() {
    return {
      map: null,
      markers: [],
      center: { lat: 20.296, lng: 85.8246 },
    };
  },
  watch: {
    potholes: {
      handler() {
        if (this.map) {
          this.addMarkers();
        }
      },
      deep: true,
    },
  },
  mounted() {
    this.initializeMap();
  },
  methods: {
    async initializeMap() {
      try {
        const loader = new Loader({
          apiKey:
            import.meta.env.VITE_GOOGLE_MAPS_API_KEY ||
            process.env.VITE_GOOGLE_MAPS_API_KEY,
          version: "weekly",
          libraries: ["places"],
        });

        const google = await loader.load();

        this.map = new google.maps.Map(this.$refs.mapRef, {
          center: this.center,
          zoom: 12,
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
          position: {
            lat: parseFloat(pothole.latitude),
            lng: parseFloat(pothole.longitude),
          },
          map: this.map,
          title: `Pothole ${pothole.pothole_id}`,
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
          <h3>Pothole ${pothole.pothole_id}</h3>
          <p>Location: ${pothole.place}</p>
          <p>Severity: ${pothole.severity.toUpperCase()}</p>
          <p>Coordinates: ${pothole.latitude}, ${pothole.longitude}</p>
          <img src=${pothole.image_url}>
        </div>
      `,
        });

        marker.addListener("click", () => {
          infoWindow.open(this.map, marker);
        });

        this.markers.push(marker);
      });
    },

    getSeverityColor(severity) {
      const colors = {
        low: "#28a745",
        medium: "#ffc107",
        high: "#dc3545",
      };
      return colors[severity.toLowerCase()] || "#ffc107";
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
:deep(.info-window img) {
  /* margin-top: 10px; */
  margin: 10px auto 0 auto;
  color: #666;
  border-radius: 10px;
  max-width: 320px;
  max-height: 200px;
}
</style>
