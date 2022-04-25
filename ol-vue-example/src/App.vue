<template>
  <div id="app">
    <div class="cell cell-map">
      <MapContainer :geojson="geojson" v-on:select="selected = $event"></MapContainer>
    </div>
    <div class="cell cell-edit">
      <Edit :geojson="geojson" v-on:change="geojson = $event"></Edit>
    </div>
    <div class="cell cell-inspect">
      <Inspect :feature="selected"></Inspect>
    </div>
  </div>
</template>

<script>
import MapContainer from './components/MapContainer'
import Edit from './components/Edit'
import Inspect from './components/Inspect'
// import { Icon } from "ol/style";
import * as style from "ol/style";
import * as ol from "ol";
import * as geom from "ol/geom";
import * as proj from "ol/proj";

import iconPika from '@/assets/pika.png'

export default {
  name: 'App',
  components: {
    Inspect,
    Edit,
    MapContainer
  },
  data() {
    return {
      selected: undefined,
      geojson: [{
        // type: 'Feature',
        // properties: {
        //   name: 'LANGBAC',
        //   quality: 'top'
        // },
        // geometry: {
        //   type: 'Point',
        //   coordinates: this.getXY(21.036747, 105.8347)
        // },
        feature: this.getIconFeature(21.036747, 105.8347)
      },
        {
          // type: 'Feature',
          // properties: {
          //   name: 'HOTAY',
          //   quality: 'top'
          // },
          // geometry: {
          //   type: 'Point',
          //   coordinates: this.getXY(21.055141, 105.819464),
          //   style: new style.Style({
          //     image: new Icon({
          //       anchor: [0.5, 46],
          //       anchorXUnits: "fraction",
          //       anchorYUnits: "pixels",
          //       src: "https://upload.wikimedia.org/wikipedia/commons/e/ec/RedDot.svg"
          //     })
          //   })
          // },
          feature: this.getIconFeature(21.055141, 105.819464)
        },
        {
          // type: 'Feature',
          // properties: {
          //   name: 'HOHOANKIEM',
          //   quality: 'top'
          // },
          // geometry: {
          //   type: 'Point',
          //   coordinates: this.getXY(21.030696, 105.852405)
          // },
          feature: this.getIconFeature(21.030696, 105.852405)
        }]
    }
  },
  methods: {
    getXY(x, y) {
      let result = []
      result.push(y)
      result.push(x)
      return result
    },
    getIconFeature(x, y) {
      let feature = new ol.Feature({
        geometry: new geom.Point(proj.fromLonLat(this.getXY(x, y))),
        name: 'Null Island',
        population: 4000,
        rainfall: 500,
      })

      // const gifUrl = 'https://c.tenor.com/SfTNX06iX-QAAAAM/alert-warning.gif';
      // // eslint-disable-next-line no-undef
      // const gif = gifler(gifUrl);
      // gif.frames(
      //     document.createElement('canvas'),
      //     function (ctx, frame) {
      //         feature.setStyle(
      //             new style.Style({
      //               image: new style.Icon({
      //                 img: ctx.canvas,
      //                 imgSize: [frame.width/2, frame.height/2],
      //                 opacity: 0.8,
      //               }),
      //             })
      //         );
      //       ctx.clearRect(0, 0, frame.width, frame.height);
      //       ctx.drawImage(frame.buffer, frame.x, frame.y);
      //     },
      //     true
      // );

      feature.setStyle(new style.Style({
        // image: new style.Circle({
        //   radius: 10,
        //   rotateWithView: 190,
        //   stroke: new style.Stroke({
        //     color: '#b30734',
        //     width: 8,
        //     rotation :9
        //   }),
        //   fill: new style.Fill({
        //     color: 'rgb(14,49,225)',
        //     background : "https://c.tenor.com/SfTNX06iX-QAAAAM/alert-warning.gif"
        //   })
        // })
        image: new style.Icon({
          anchor: [0.5, 46],
          anchorXUnits: "fraction",
          anchorYUnits: "pixels",
          width:2,
          src: iconPika
        })
      }))
      return feature
    }
  }

}
</script>

<style>
html, body {
  height: 100%;
  margin: 0;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  height: 100%;
  display: grid;
  grid-template-columns: 100vh;
  grid-auto-rows: 1fr;
  grid-gap: 1rem;
  padding: 1rem;
  box-sizing: border-box;
}

.cell {
  border-radius: 4px;
  background-color: lightgrey;
}

.cell-map {
  grid-column-start: 1;
  grid-column-end: 3;
  grid-row-start: 1;
  grid-row-end: 3;
}

.cell-edit {
  grid-column: 3;
  grid-row: 1;
}

.cell-inspect {
  grid-column: 3;
  grid-row: 2;
}

.line:hover {
  stroke: red;
}

.line2 {
  stroke: black;
  stroke-width: 4;
  fill: red;
}

.line3 {
  stroke: purple;
  stroke-width: 6;
  stroke-dasharray: 8, 12;
}

.line4 {
  stroke: black;
  stroke-width: 6;
  stroke-dasharray: 8, 2;
  fill: yellow;
}

.line5 {
  stroke: gold;
  stroke-width: 2;
  fill: yellow;
}
</style>
