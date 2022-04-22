<template>
    <div id = "map">
    </div>
</template>

<script>
import { Map, View } from 'ol';
import { Tile as TileLayer } from 'ol/layer';
import { OSM } from 'ol/source';
import { transform } from "ol/proj";

export default {
    name: 'Map',
    mounted() {
        this.map = new Map({
            target: 'map',
            layers: [
                new TileLayer({
                    source: new OSM()
                })
            ],
            view: new View({
                // center: [21.028333, 105.853333],
                center: transform([105.8347, 21.036747], 'EPSG:4326', 'EPSG:3857'),
                zoom: 13
            })
        })
    }
}
</script>
