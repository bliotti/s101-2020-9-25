<script>
  import Weather from '../components/Weather.svelte'
  import { fahrenheitToCelsius, celsiusToFahrenheit } from "temperature"
  import { getJSON } from '../lib/async.js'
  
  let temp = 21.7;
  let unit = "c";

  $: displayTemp = `${Math.floor(temp)} &deg; ${unit.toUpperCase()}`;

  function convertToF() {
    temp = celsiusToFahrenheit(temp);
    unit = "f";
  }
  function convertToC() {
    temp = fahrenheitToCelsius(temp);
    unit = "c";
  }

  function getWeather() {
    return getJSON("/api/weather?city=charleston,sc&country=us").then(
      (results) => ({
        temp: `${results.temp} &deg; C`,
        icon: results.weather.icon,
        description: results.weather.description,
        city: `${results.city_name} ${results.state_code}`,
      })
    );
  }
</script>
<nav>
  <div>
    <a on:click|preventDefault={convertToF}>F</a>
    |
    <a on:click|preventDefault={convertToC}>C</a>
  </div>
  <a href="">Favorites</a>
</nav>
<main>
  <p>{@html displayTemp}</p>
  {#await getWeather()}
    Loading...
  {:then weather}
    <Weather {...weather} />

  {/await}
</main>
<style>
  h3 {
    color: rebeccapurple;
  }
  nav {
    margin-bottom: 0;
    margin-left: 8px;
    margin-right: 8px;
  }
  main {
    display: flex;
    flex-direction: column;
    align-items: center;
  }


</style>


