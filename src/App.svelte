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
  var currentPos={lng:0,lat:0};
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
    console.log(GreatCircle.distance(currentPos.lat,currentPos.lng, 21.051686, 105.845411))
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
    <img src={Logo} width="150px"/>
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
<p id="desciption">Hello</p>

<style>
	main {
		width: 100%;
		height: 100vh;
		position: relative;
	}
  #map{
    z-index:-1;
  }
  #description{
    position:absolute;
    left:0;
    top:0;
    z-index:99;
  }
</style>