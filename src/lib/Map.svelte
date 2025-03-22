<script>
    import { onMount } from "svelte";
    import mapboxgl from "mapbox-gl";
    import "mapbox-gl/dist/mapbox-gl.css"; // Import Mapbox CSS

    let map;
    let mapContainer;

    const locations = [
        {
            name: "Rooms, Parker Street",
            address: "Parker Street",
            coordinates: { lat: 53.405530942522134, lng: -2.981729263632423 },
            datesActive: "December 1825 - August 1826",
            image: "Parker Street",
            path: "parker.PNG",
        },
        {
            name: "Chapel, St Thomas's Buildings",
            address: "St Thomas Street",
            coordinates: { lat: 53.408282077930316, lng: -2.9866895554547144 },
            datesActive: "June 1825 - c. September 1827",
            image: "Chapel, St Thomas's Buildings",
            path: "thomas.jpg",
        },
        {
            name: "Union Newsroom",
            address: "105 Duke Street",
            coordinates: { lat: 53.40165706808926, lng: -2.9807853906357495 },
            datesActive: "September 1827 - September 1837",
            image: "Union News Room",
            path: "union.PNG",
        },
        {
            name: "Apprentices' Library",
            address: "School Lane",
            coordinates: { lat: 53.40444, lng: -2.98338 },
            datesActive: "September 1826 - September 1827",
            image: "Apprentices' Library",
            path: "lib.PNG",
        },
        {
            name: "Liverpool Mechanics' Institution",
            address: "Mount Street",
            coordinates: { lat: 53.3999254230283, lng: -2.9719076483952622 },
            datesActive: "1837 - 1905",
            image: "Mechanics' Institution",
            path: "mech.jpg",
        },
        {
            name: "Land, Russell Street",
            address: "Russell Street",
            coordinates: { lat: 53.406613795538455, lng: -2.973283351672745 },
            datesActive: "[bought] September 1826 - December 1829 [sold]",
            image: "John Sinclair_Liverpool Street_Russell",
            path: "russell.jpeg",
        },
    ];

    const MAPBOX_ACCESS_TOKEN =
        "pk.eyJ1IjoiaXNhYmVscm9iaW5zb24iLCJhIjoiY204OTR5aDA1MTB1czJqcjNwa2c0dmEyMCJ9.tpU8bZIN7_rHmPaeBPHE3w";

    onMount(() => {
        mapboxgl.accessToken = MAPBOX_ACCESS_TOKEN;

        map = new mapboxgl.Map({
            container: mapContainer, // Now properly assigned
            style: "mapbox://styles/mapbox/streets-v11",
            center: [-2.9779, 53.4084], // Liverpool, UK
            zoom: 12,
        });

        map.on("load", () => {
            // HISTORICAL LAYER (Ensure the ID is valid)
            map.addSource("portland", {
                type: "raster",
                url: "mapbox://isabelrobinson.0cepgiyt",
            });

            map.addLayer({
                id: "portland",
                source: "portland",
                type: "raster",
            });

            // ADD MARKERS FOR LOCATIONS
            locations.forEach((location) => {
                new mapboxgl.Marker()
                    .setLngLat([
                        location.coordinates.lng,
                        location.coordinates.lat,
                    ])
                    .setPopup(
                        new mapboxgl.Popup({ offset: 25 }) // add popups
                            .setHTML(`
                  <h3>${location.name}</h3>
                  <p>${location.address}</p>
                  <p>${location.datesActive}</p>
                  <img src="${location.path}" alt="${location.image}">
                `),
                    )
                    .addTo(map);
            });
        });

        return () => map.remove();
    });
</script>

<div bind:this={mapContainer} class="map-container"></div>

<style>
    :global(body, html) {
        margin: 0;
        padding: 0;
        width: 100%;
        height: 100%;
        overflow: hidden;
    }

    .map-container {
        position: absolute;
        top: 0;
        left: 0;
        width: 100vw;
        height: 100vh;
    }

    :global(button) {
        color: black;
        font-size: 20px;
        padding: 5px 10px;
    }

    :global(.mapboxgl-popup-content) {
        width: 300px; /* Set a maximum width for the popup content */
        color: black;
        text-align: left;
    }

    :global(.mapboxgl-popup-content img) {
        width: 100%; /* Ensure the image fits within the popup */
        height: auto; /* Maintain the aspect ratio */
    }
</style>
