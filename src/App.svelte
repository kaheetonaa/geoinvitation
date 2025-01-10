<script>
  import MapLibre from 'svelte-maplibre/MapLibre.svelte';
  import Popup from 'svelte-maplibre/Popup.svelte';
  import Marker from 'svelte-maplibre/Marker.svelte';
  import G1 from './lib/img/G1.jpg';
  import G2 from './lib/img/G2.jpg';
  import E1 from './lib/img/E1.jpg';
  import E2 from './lib/img/E2.jpg';
  import Logo from './lib/img/Logo.png';
  import Geolocation from './lib/img/Geolocation.png';
  import GreatCircle from 'great-circle';
  import Papa from "papaparse";

const queryString = window.location.search;
const urlParams = new URLSearchParams(queryString);
let id = urlParams.get('id')
let locale='vi'
if (urlParams.get('locale')=='en'){locale=urlParams.get('locale');}
let inviter;
let here={'vi':'Bạn đang ở đây','en':'You are here'};
let dist_des={'vi':'Khoảng cách giữa bạn và đám cưới Tố Nga và Quang Huy chỉ là','en':"The distance between you and To Nga and Quang Huy's wedding is just"}
let front= {'vi':G1,'en':E1}
let back= {'vi':G2,'en':E2}
let text_height={'vi':'19.5%','en':'23%'}
let button_scale_up={'vi':'Phóng','en':'Maximize'}
let button_scale_down={'vi':'Thu','en':'Minimize'}

  Papa.parse('https://docs.google.com/spreadsheets/d/e/2PACX-1vQRyUg1BNt9EEpGsz8sCFGsuXVCgAiOHIuYQWLDEEOOFZCBWxSxObMvNoauAvPpf4mATc5unO-RTRJt/pub?gid=332641665&single=true&output=csv', {
    download: true,
    complete: results => {
        inviter=results['data'][id][1];
    }
})

  var currentPos={lng:0,lat:0};
  let dist=-1;
  let button=button_scale_up[locale]+" ⛶";
 

 
const markers=[
    {
      lngLat: [105.845411,21.051686],
      name: {'vi':'Nhà hàng SoftWater, nơi tổ chức đám cưới Tố Nga và Quang Huy','en':'SoftWater Restaurant, wedding venue To Nga and Quang Huy'}
    }
  ];
function checkGeo() {
  if (navigator.geolocation) {
    navigator.geolocation.getCurrentPosition(showPosition);
  } else {
  }
}


  function showPosition(position) {
    currentPos={lng:position.coords.longitude,lat:position.coords.latitude};
    dist=GreatCircle.distance(currentPos.lat,currentPos.lng, 21.051686, 105.845411);
}


function scale() {
  let element = document.getElementById("invitation");
  console.log(element)
  element.classList.toggle('scale')
  if (button==button_scale_up[locale]+" ⛶") {
    button = button_scale_down[locale]+" ⛶"
  } else {
    button = button_scale_up[locale]+" ⛶"
  }
}

setInterval(checkGeo, 5000);

</script>

<MapLibre id="map"
  style="https://api.maptiler.com/maps/streets-v2/style.json?key=fTquHLsOc7W9XJAo9yfK" 
  standardControls
  center={[105.845411,21.051686]}
  zoom={15.5}
  pitch={45}
  bearing={-17.6}
>
 {#each markers as { lngLat, name }}
    <!-- Unlike the custom marker example, default markers do not have mouse events,
    and popups only support the default openOn="click" behavior -->
    <Marker {lngLat}>
    <img src={Logo} width="100px"/>
      <Popup offset={[0, -75]}>
        <div class="text-lg font-bold">{name[locale]}</div>
      </Popup>
    </Marker>
      {/each}
    <Marker lngLat={currentPos}>
    <img src={Geolocation} width="50px"/>
      <Popup offset={[0, -25]}>
        <div class="text-lg font-bold">{here[locale]}</div>
      </Popup>
    </Marker>
</MapLibre>
{#if dist!=-1}
<div style="z-index:205;position:absolute;right:5vw;top:5vw;width:50vw;padding:10px;border-style: solid;border-radius: 5px;border-width:1px;border-color:white;background-color:#FFFFFFCC;color:#4A5D85;font-family: 'Playwrite US Modern', serif;"><p>{dist_des[locale]}{dist}km<p><a href=https://maps.app.goo.gl/wR6rfXBBN24NvBq6A>Địa điểm google</a><br><a href="https://osm.org/go/4dt1IG5Pa?m=&node=12463420847">Địa điểm OSM</a></div>
{/if}

<div id='invitation'>
<button style="z-index:207;position:absolute;right:0px;top:-40px;" onclick={scale}>{button}</button>
  <div class="flip-card-inner">
    <div class="flip-card-front">
      <img src="{front[locale]}" alt="Avatar" style="height:100%">
    </div>
    <div class="flip-card-back">
     <div style="position:absolute;text-align:center;font-size:.8em;top:{text_height[locale]};z-index:210;color:#4A5D85;width:120%">{inviter}</div>
     <img src="{back[locale]}" alt="Avatar" style="height:100%">
    
    </div>
  </div>
</div>

<style>
/* The flip card container - set the width and height to whatever you want. We have added the border property to demonstrate that the flip itself goes out of the box on hover (remove perspective if you don't want the 3D effect */
#invitation {
  font-family: 'Playwrite US Modern';
  position:absolute;
  background-color: transparent;
  width: 30vh;
  height: 50vh;
  perspective: 1000px; /* Remove this if you don't want the 3D effect */
  bottom:5vh;
  left: 5%;
  z-index:206
}

:global(.scale){
  transform: scale(1.5) translate(7.5vw,-10vh);
  transition-duration: 2s;
}

/* This container is needed to position the front and back side */
.flip-card-inner {
  position: relative;
  width: 100%;
  height: 100%;
  text-align: center;
  transition: transform 0.8s;
  transform-style: preserve-3d;
}

/* Do an horizontal flip when you move the mouse over the flip box container */
#invitation:hover .flip-card-inner {
  transform: rotateY(180deg);
}

/* Position the front and back side */
.flip-card-front, .flip-card-back {
  position: absolute;
  width: 100%;
  height: 100%;
  -webkit-backface-visibility: hidden; /* Safari */
  backface-visibility: hidden;
}

/* Style the front side (fallback if image is missing) */
.flip-card-front {
  background-color: #bbb;
  color: black;
}

/* Style the back side */
.flip-card-back {
  background-color: dodgerblue;
  color: white;
  transform: rotateY(180deg);
}
</style>
