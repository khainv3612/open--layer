<template>
  <div ref="map-root"
       style="width: 100%; height: 100%">
  </div>
</template>

<script>
import View from 'ol/View'
import Map from 'ol/Map'
import TileLayer from 'ol/layer/Tile'
import OSM from 'ol/source/OSM'
import VectorLayer from 'ol/layer/Vector'
import VectorSource from 'ol/source/Vector'
// import GeoJSON from 'ol/format/GeoJSON'

import 'ol/ol.css'
import { transform } from "ol/proj";
import * as ol from "ol";
import * as geom from "ol/geom";
import * as proj from "ol/proj";
import * as style from "ol/style";

export default {
  name: 'MapContainer',
  components: {},
  props: {
    geojson: []
  },
  data() {
    return {
      olMap: null,
      vectorLayer: null,
      selectedFeature: null,
      iconFeature: null,
    }
  },
  mounted() {
    this.iconFeature = new ol.Feature({
      geometry: new geom.Point(proj.fromLonLat([105.819464, 21.055141])),
      name: 'Null Island',
      population: 4000,
      rainfall: 500,
    }),
        this.iconFeature.setStyle(new style.Style({
          image: new style.Icon({
            anchor: [0.5, 46],
            anchorXUnits: 'fraction',
            anchorYUnits: 'pixels',
            src: "https://upload.wikimedia.org/wikipedia/commons/e/ec/RedDot.svg",
          })
        })),


        this.vectorLayer = new VectorLayer({
          source: new VectorSource({
            // features: [this.iconFeature, this.iconFeature1],
            features: [],
          })
        }),

        this.olMap = new Map({
          target: this.$refs['map-root'],
          layers: [
            new TileLayer({
              source: new OSM(),
            }),
            this.vectorLayer
          ],
          view: new View({
            zoom: -990,
            center: transform([105.8347, 21.036747], 'EPSG:4326', 'EPSG:3857'),
            constrainResolution: true
          }),
        })

    this.olMap.on('pointermove', (event) => {
      const hovered = this.olMap.forEachFeatureAtPixel(event.pixel, (feature) => feature)
      if (hovered !== this.selectedFeature) {
        this.$set(this, 'selectedFeature', hovered);
      }
    })

    this.updateSource(this.geojson)
  },
  watch: {
    geojson(value) {
      this.updateSource(value)
    },
    selectedFeature(value) {
      this.$emit('select', value)
    }
  },
  methods: {
    updateSource(geojson) {
      const view = this.olMap.getView();
      const source = this.vectorLayer.getSource();
      for (let i = 0; i < geojson.length; i++) {
        source.addFeature(geojson[i].feature);
        view.fit(source.getExtent())
      }

    }
  }
}
</script>
