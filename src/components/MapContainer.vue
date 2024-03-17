<template>
  <div ref="mapRoot"
       style="width: 100%; height: 100%">
  </div>
</template>

<script setup>
import View from 'ol/View'
import Map from 'ol/Map'
import TileLayer from 'ol/layer/Tile'
import OSM from 'ol/source/OSM'
import { ref } from 'vue'

import 'ol/ol.css'
import { onMounted } from 'vue'
import VectorLayer from 'ol/layer/Vector'
import VectorSource from 'ol/source/Vector'
import GeoJSON from 'ol/format/GeoJSON'

import Feature from 'ol/Feature';
import Overlay from 'ol/Overlay';
import Point from 'ol/geom/Point';
import {Icon, Style} from 'ol/style.js';
import {OGCMapTile} from 'ol/source.js';

const mapRoot = ref()

///////////////////
// const iconFeature = new Feature({
//   geometry: new Point([0, 0]),
//   name: 'Null Island',
//   population: 4000,
//   rainfall: 500,
// });

// const iconStyle = new Style({
//   image: new Icon({
//     anchor: [0.5, 46],
//     anchorXUnits: 'fraction',
//     anchorYUnits: 'pixels',
//     src: 'data/icon.png',
//   }),
// });

// iconFeature.setStyle(iconStyle);

// const vectorSource = new VectorSource({
//   features: [iconFeature],
// });

// const vectorLayer = new VectorLayer({
//   source: vectorSource,
// });

// const rasterLayer = new TileLayer({
//   source: new OGCMapTile({
//     url: 'https://maps.gnosis.earth/ogcapi/collections/NaturalEarth:raster:HYP_HR_SR_OB_DR/map/tiles/WebMercatorQuad',
//     crossOrigin: '',
//   }),
// });

// const map = new Map({
//   layers: [rasterLayer, vectorLayer],
//   target: mapRoot.value,
//   view: new View({
//     center: [0, 0],
//     zoom: 3,
//   }),
// });

// const element = document.getElementById('popup');

// const popup = new Overlay({
//   element: element,
//   positioning: 'bottom-center',
//   stopEvent: false,
// });
// map.addOverlay(popup);

// let popover;
// function disposePopover() {
//   if (popover) {
//     popover.dispose();
//     popover = undefined;
//   }
// }
// // display popup on click
// map.on('click', function (evt) {
//   const feature = map.forEachFeatureAtPixel(evt.pixel, function (feature) {
//     return feature;
//   });
//   disposePopover();
//   if (!feature) {
//     return;
//   }
//   popup.setPosition(evt.coordinate);
//   popover = new bootstrap.Popover(element, {
//     placement: 'top',
//     html: true,
//     content: feature.get('name'),
//   });
//   popover.show();
// });

// // change mouse cursor when over marker
// map.on('pointermove', function (e) {
//   const pixel = map.getEventPixel(e.originalEvent);
//   const hit = map.hasFeatureAtPixel(pixel);
//   map.getTarget().style.cursor = hit ? 'pointer' : '';
// });
// // Close the popup when the map is moved
// map.on('movestart', disposePopover);

//////////////////

const data = {
    type: 'Feature',
    properties: {},
    geometry: {
      type: 'Polygon',
      coordinates: [
        [
          [
            -27.0703125,
            43.58039085560784
          ],
          [
            -28.125,
            33.563987128451217
          ],
          [
            -10.8984375,
            30.84267363195431
          ],
          [
            -27.0703125,
            44.58039085560784
          ],
          [
            -39.0703125,
            44.58039085560784
          ],
        ]
      ]
    }
  };

onMounted(() => {
  const feature = new GeoJSON().readFeature(data, {
    featureProjection: 'EPSG:3857'
  });

  const vectorLayer = new VectorLayer({
    source: new VectorSource({
      features: [feature],
    }),
  })

  new Map({
    target: mapRoot.value,
    layers: [
      new TileLayer({
        source: new OSM(),
      }),
      vectorLayer 
    ],

    view: new View({
      zoom: 2,
      center: [10, -43],
      constrainResolution: true
    }),
  })
}) 
</script>