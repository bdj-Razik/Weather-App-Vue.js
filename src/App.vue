<template>
  <div class="container my-5">
    <div class="row mx-4" id="app-container">
      <div class="col-12">
        <h4 class="mb-3 text-center">Weather Stats</h4>
        <div class="button button-dial">
          <span class="button-dial-spoke"></span>
          <span class="button-dial-spoke"></span>
          <span class="button-dial-spoke"></span>
          <span class="button-dial-spoke"></span>
          <span class="button-dial-spoke"></span>
          <span class="button-dial-spoke"></span>

          <div class="button-dial-top"></div>
          <div class="button-dial-label">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              xmlns:xlink="http://www.w3.org/1999/xlink"
              version="1.1"
              x="0px"
              y="0px"
              viewBox="0 0 100 125"
              enable-background="new 0 0 100 100"
              xml:space="preserve"
            >
              <g>
                <path
                  d="M60.333,68.349V11.111C60.333,4.984,55.349,0,49.222,0c-6.126,0-11.111,4.984-11.111,11.111v57.237   c-4.15,3.323-6.667,8.397-6.667,13.874c0,9.802,7.975,17.777,17.777,17.777C59.024,100,67,92.024,67,82.223   C67,76.746,64.482,71.672,60.333,68.349z M44.778,48.363h4.444v-4.444h-4.444V35.03h4.444v-4.444h-4.444v-8.889h4.444v-4.444   h-4.444v-6.142c0-2.455,1.99-4.444,4.444-4.444c2.455,0,4.445,1.989,4.445,4.444v41.111h-8.889V48.363z"
                />
              </g>
            </svg>
            {{ weather.temp }}&deg;C
          </div>
        </div>
      </div>

      <div class="row">
        <div class="col-6 col-md-3 d-flex flex-column align-items-center mb-5">
          <div class="mb-2">
            <font-awesome-icon icon="fa-solid fa-cloud" color="#00092C" size="xl" />
          </div>
          <div>
            <h5>{{ weather.cloud }}&deg;</h5>
          </div>
        </div>

        <div class="col-6 col-md-3 d-flex flex-column align-items-center mb-5">
          <div class="mb-2">
            <font-awesome-icon icon="fa-solid fa-water" color="#03C988" size="xl" />
          </div>
          <div>
            <h5>{{ weather.humidity }}&deg;</h5>
          </div>
        </div>

        <div class="col-6 col-md-3 d-flex flex-column align-items-center mb-5">
          <div class="mb-2">
            <font-awesome-icon icon="fa-solid fa-wind" color="#DC0000" size="xl" />
          </div>
          <div>
            <h5>{{ weather.vis }} KM</h5>
          </div>
        </div>

        <div class="col-6 col-md-3 d-flex flex-column align-items-center mb-5">
          <div class="mb-2">
            <font-awesome-icon
              icon="fa-solid fa-sun-plant-wilt"
              color="#FF731D"
              size="xl"
            />
          </div>
          <div>
            <h5>{{ weather.feelslike }}&deg;C</h5>
          </div>
        </div>
      </div>

      <div class="row">
        <div class="col-12 d-flex flex-column align-items-center mb-5">
          <div class="mb-2">
            <h6>{{ location.tz }} / {{ location.country }}</h6>
          </div>
          <div class="mb-2">
            <h6>lat : {{ location.lat }} / long : {{ location.lon }}</h6>
          </div>
          <div class="mb-2">
            <h6>{{ location.localtime }}</h6>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script setup>
import { ref, onBeforeMount } from "vue";
import axios from "axios";

const weather = ref({
  temp: 0,
  cloud: 0,
  humidity: 0,
  vis: 0,
  feelslike: 0,
});

const location = ref({
  country: "",
  lat: "",
  lon: "",
  localtime: "",
  tz: "",
});

const options = {
  method: "GET",
  url: "https://weatherapi-com.p.rapidapi.com/current.json",
  params: { q: "algeria" },
  headers: {
    "X-RapidAPI-Key": "your key api",
    "X-RapidAPI-Host": "weatherapi-com.p.rapidapi.com",
  },
};

const getWeeather = async () => {
  axios
    .request(options)
    .then(function (response) {
      const data = response.data.current;

      weather.value.temp = data.temp_c;
      weather.value.cloud = data.cloud;
      weather.value.humidity = data.humidity;
      weather.value.vis = data.vis_km;
      weather.value.feelslike = data.feelslike_c;

      const info = response.data.location;

      location.value.country = info.country;
      location.value.lat = info.lat;
      location.value.lon = info.lon;
      location.value.localtime = info.localtime;
      location.value.tz = info.tz_id;
    })
    .catch(function (error) {
      console.error(error);
    });
};

onBeforeMount(async () => {
  await getWeeather();
});
</script>
