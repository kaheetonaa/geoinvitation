<script>
  import MapLibre from 'svelte-maplibre/MapLibre.svelte';
  import Popup from 'svelte-maplibre/Popup.svelte';
  import Marker from 'svelte-maplibre/Marker.svelte';
  import G1 from './lib/img/G1.jpg';
  import G2 from './lib/img/G2.jpg';
  import T1 from './lib/img/T1.jpg';
  import T2 from './lib/img/T2.jpg';
  import Logo from './lib/img/Logo.png';
  import Geolocation from './lib/img/Geolocation.png';
  import GreatCircle from 'great-circle';
  import Papa from "papaparse";

  const queryString = window.location.search;
const urlParams = new URLSearchParams(queryString);
let id = urlParams.get('id')
let inviter;

  Papa.parse('https://docs.google.com/spreadsheets/d/e/2PACX-1vQRyUg1BNt9EEpGsz8sCFGsuXVCgAiOHIuYQWLDEEOOFZCBWxSxObMvNoauAvPpf4mATc5unO-RTRJt/pub?gid=332641665&single=true&output=csv', {
    download: true,
    complete: results => {
        inviter=results['data'][id][1];
    }
})

  var currentPos={lng:0,lat:0};
  let dist=-1;
  let button="Ẩn🔻";
 

 
const markers=[
    {
      lngLat: [105.845411,21.051686],
      label: 'Nhà hàng SoftWater',
      name: 'Nhà hàng SoftWater, nơi tổ chức đám cưới Tố Nga và Quang Huy',
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


function movedown() {
  let element = document.getElementById("invitation");
  console.log(element)
  element.classList.toggle('movedown')
  if (button=="Ẩn🔻") {
    button = "Hiện🔺"
  } else {
    button = "Ẩn🔻"
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
        <div class="text-lg font-bold">{name}</div>
      </Popup>
    </Marker>
      {/each}
    <Marker lngLat={currentPos}>
    <img src={Geolocation} width="50px"/>
      <Popup offset={[0, -25]}>
        <div class="text-lg font-bold">Bạn đang ở đây</div>
      </Popup>
    </Marker>
</MapLibre>
{#if dist!=-1}
<div style="z-index:205;position:absolute;right:5vw;top:5vw;width:55%">Khoảng cách giữa bạn và đám cưới Tố Nga và Quang Huy chỉ là {dist}km</div>
{/if}

<div id='invitation'>
<button style="z-index:207;position:absolute;right:0px;top:-20px;" onclick={movedown}>{button}</button>
  <div class="flip-card-inner">
    <div class="flip-card-front">
      <img src="{G1}" alt="Avatar" style="height:100%">
    </div>
    <div class="flip-card-back">
     <div style="position:absolute;text-align:center;top:20%;z-index:210;color:red;width:100%">{inviter}</div>
     <img src="{G2}" alt="Avatar" style="height:100%">
    
    </div>
  </div>
</div>

<style>
/* The flip card container - set the width and height to whatever you want. We have added the border property to demonstrate that the flip itself goes out of the box on hover (remove perspective if you don't want the 3D effect */
#invitation {
  position:absolute;
  background-color: transparent;
  width: 30vh;
  height: 50vh;
  perspective: 1000px; /* Remove this if you don't want the 3D effect */
  bottom:5vh;
  left: 5%;
}

:global(.movedown){
  transform: translate(0px, 50vh);
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
