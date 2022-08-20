<script lang="ts">
  import { onMount } from 'svelte'
  import axios from 'axios'

  let loadAnimals = []

  let loadMoreData = 10

  onMount(async () => {
    try {
      const response = await axios.get(
        `https://zoo-animal-api.herokuapp.com/animals/rand/${loadMoreData}`,
      )
      loadAnimals = response.data
    } catch (error) {
      console.log('Error ', error)
    }
  })

  const loadMoreDataHandler = () => {
    window.location.reload()
  }
</script>

<style>
  .animal-container {
    align-items: center;
    margin-left: 30%;
  }

  .animal-list {
    color: #ff3e00;
    font-size: 2em;
    font-weight: 100;
  }

  @import url('https://fonts.googleapis.com/css?family=Open');

  html {
    height: 100%;
    width: 100%;
    background-image: linear-gradient(to right top, #8e44ad 0%, #3498db 100%);
  }

  nav {
    max-width: 660px;
    mask-image: linear-gradient(
      90deg,
      rgba(255, 255, 255, 0) 0%,
      #ffffff 25%,
      #ffffff 75%,
      rgba(255, 255, 255, 0) 100%
    );
    margin: 0 auto;
    padding: 0px 0;
  }

  nav ul {
    text-align: center;
    background: linear-gradient(
      90deg,
      rgba(255, 255, 255, 0) 0%,
      rgba(255, 255, 255, 0.2) 25%,
      rgba(255, 255, 255, 0.2) 75%,
      rgba(255, 255, 255, 0) 100%
    );
    box-shadow: 0 0 25px rgba(0, 0, 0, 0.1),
      inset 0 0 1px rgba(255, 255, 255, 0.6);
  }

  nav ul li {
    display: inline-block;
  }

  nav ul li a {
    padding: 18px;
    font-family: 'Open Sans';
    text-transform: uppercase;
    color: rgba(0, 35, 122, 0.5);
    font-size: 18px;
    text-decoration: none;
    display: block;
  }

  nav ul li a:hover {
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1),
      inset 0 0 1px rgba(255, 255, 255, 0.6);
    background: rgba(255, 255, 255, 0.1);
    color: rgba(0, 35, 122, 0.7);
  }

  @media (max-width: 768px) {
    .animal-container {
      margin-left: 0%;
    }

    .animal-card {
      width: 100px;
    }
    .glass-effect {
      width: 100px;
    }
  }

  .glass-effect {
    backdrop-filter: blur(5px);
    -webkit-backdrop-filter: blur(5px);
    box-shadow: 0 8px 32px rgb(31 38 135 / 37%);
    background-image: linear-gradient(
      200deg,
      rgba(255, 255, 255, 0.5),
      rgba(255, 255, 255, 0)
    );
    border-radius: 15px;
    border-top: 0px solid rgba(255, 255, 255, 1);
    border-left: 0px solid rgba(255, 255, 255, 1);
    margin-top: 4em;
  }
  .animal-card {
    width: 500px;
    height: 300px;
    display: flex;
    margin-top: 20;
    padding: 30px 50px 30px 40px;
  }

  .animal-pic img {
    border-radius: 50%;
    border: 7px solid #fff;
    width: 100px;
  }

  .animal-details {
    margin-left: 30px;
    padding-top: 10px;
    width: calc(100% - 130px);
  }

  .animal-details p {
    margin: 0 0 4px;
  }

  .animal-details .name {
    font-size: 22px;
  }

  .animal-details .mention {
    font-size: 13px;
  }

  .animal-details .about {
    margin-top: 20px;
    font-size: 14px;
    line-height: 1.5;
  }

  .animal-details .stats {
    margin-top: 20px;
    display: flex;
  }

  .animal-details .stat {
    padding-right: 30px;
  }

  .animal-details .stat + .stat {
    padding-left: 30px;
    border-left: 1px solid rgb(255, 255, 255, 0.2);
  }

  .stat-value {
    font-size: 17px;
  }
  .stat-description {
    font-size: 12px;
  }
  .loader {
    position: relative;
    width: 150px;
    height: 150px;
    border-radius: 50%;
    background: linear-gradient(45deg, transparent, transparent 40%, #e5f403);
    animation: animate 2s linear infinite;
  }

  .loader::before {
    content: '';
    position: absolute;
    top: 6px;
    left: 6px;
    bottom: 6px;
    right: 6px;
    background: transparent;
    border-radius: 50%;
    z-index: 1000;
  }

  .loader::after {
    content: '';
    position: absolute;
    top: 0px;
    left: 0px;
    bottom: 0px;
    right: 0px;
    background: linear-gradient(45deg, transparent, transparent 40%, #e5f403);
    border-radius: 50%;
    filter: blur(60px);
    z-index: 1;
  }

  .loader-container {
    display: flex;
    align-items: center;
    justify-content: center;
  }

  @keyframes animate {
    0% {
      transform: rotate(0deg);
      filter: hue-rotate(0deg);
    }
    100% {
      transform: rotate(360deg);
      filter: hue-rotate(360deg);
    }
  }

  body {
    align-items: center;
    justify-content: center;
  }
</style>

<body>

  <nav>
    <ul>
      <li>
        <a href="#">Animal Zoo</a>
      </li>
      <li>
        <a href="https://dantelentsoe.com">Reach Dev</a>
      </li>
      <li>
        <a href="#" on:click={loadMoreDataHandler}>Generate New List</a>

      </li>
    </ul>
  </nav>

  {#if loadAnimals.length <= 0}
    <div class="loader-container">
      <div class="loader" />
    </div>
  {/if}

  <div class="animal-container">
    {#each loadAnimals as animal}
      <div class="animal-card glass-effect">
        <div class="animal-pic">
          <img src={animal.image_link} alt={animal.name} />
        </div>
        <div class="animal-details">
          <h1 class="animal-list">{animal.name}</h1>
          <p class="mention">Diet : {animal.diet}</p>

          <p class="about">Location : {animal.geo_range}</p>
          <p class="about">Animal Type : {animal.animal_type}</p>

          <div class="stats">

            <div class="stat">
              <span class="stat-value">Length Max</span>
              <br />
              <span class="stat-description">{animal.length_max}m</span>
            </div>
            <div class="stat">
              <span class="stat-value">Length Min</span>
              <br />
              <span class="stat-description">{animal.length_min}m</span>
            </div>
          </div>
        </div>

      </div>
    {/each}
  </div>
</body>
