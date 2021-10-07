<template>
    <h1>Display Marker</h1>
    <div id="map"></div>
</template>

<script>
import mapboxgl from "mapbox-gl";
import "mapbox-gl/dist/mapbox-gl.css";
import { onMounted } from "vue";

//lokasi yang diberikan
var markers = [
    { lat: -6.960848, lng: 107.966894 },
    { lat: 82.587825, lng: 21.895907 }
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
                //load markers
                new mapboxgl.Marker({color: "red"})
                    .setLngLat([markers[0].lng, markers[0].lat])
                    .addTo(map);
                new mapboxgl.Marker({color: "red"})
                    .setLngLat([markers[1].lng, markers[1].lat])
                    .addTo(map);
                
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