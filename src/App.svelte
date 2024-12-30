<script>
  import MapLibre from 'svelte-maplibre/MapLibre.svelte';
  import Popup from 'svelte-maplibre/Popup.svelte';
  import Marker from 'svelte-maplibre/Marker.svelte';
const markers=[
    {
      lngLat: [105.845411,21.051686],
      label: 'Nhà hàng SoftWater',
      name: 'Nhà hàng SoftWater, nơi tổ chức đám cưới Tố Nga và Quang Huy',
    }
  ];


</script>

<MapLibre
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
    <img src="https://i.natgeofe.com/n/4cebbf38-5df4-4ed0-864a-4ebeb64d33a4/NationalGeographic_1468962_3x4.jpg" width="100px"/>
      <Popup offset={[0, -10]}>
        <div class="text-lg font-bold">{name}</div>
      </Popup>
    </Marker>
      {/each}
</MapLibre>

<style>
	main {
		width: 100%;
		height: 100vh;
		position: relative;
	}
</style>