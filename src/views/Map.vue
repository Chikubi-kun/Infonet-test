<template>
    <h1>Display Marker</h1>
    <div id="map"></div>
</template>

<script>
import { onMounted } from "vue";
import mapboxgl from "mapbox-gl";
import "mapbox-gl/dist/mapbox-gl.css";

//lokasi yang diberikan
var markers = [
        { "lat": -6.960848, "lng": 107.966894 },
        { "lat": 82.587825, "lng": 21.895907 }
    ]

export default {
    setup() {
        onMounted(() => {
            mapboxgl.accessToken = "pk.eyJ1IjoicmFpc2V5b3VydGFpbCIsImEiOiJja3VmbWttNngxY25yMnZvZ3VjNDZ5amcwIn0.GI7j1PaSlUuD1_sz9eBBUQ";

            const map = new mapboxgl.Map({
                container: 'map',
                style: 'mapbox://styles/mapbox/light-v10',
            });
 
            map.on('load', () => {
                // load markers
                markers.forEach(marker => {
                    new mapboxgl.Marker({color: "red"})
                        .setLngLat([marker.lng, marker.lat])
                        .addTo(map);
                })
                
                //zoom out agar kedua marker kelihatan
                map.setZoom(1);
            });
        });
        return {};
    },
};
</script>

<style scoped>
#map {
    height: 600px;
}
</style>