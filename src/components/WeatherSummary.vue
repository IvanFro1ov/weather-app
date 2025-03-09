<template>
  <div class="summary">
    <div
      class="pic-main"
      :style="{ backgroundImage: `url('${weatherImage}')` }"
    ></div>
    <div class="weather">
      <div class="temp">{{ Math.round(weatherInfo?.main?.temp) }} °C</div>
      <div class="weather-desc text-block">
        {{ capitalizeFirstLetter(weatherInfo?.weather[0].description) }}
      </div>
    </div>
    <div class="city text-block">
      {{ weatherInfo?.name }}, {{ weatherInfo?.sys?.country }}
    </div>
    <div class="date text-block">{{ today }}</div>
  </div>
</template>

<script setup>
import { capitalizeFirstLetter } from "../utils";
import { computed } from "vue";

const props = defineProps({
  weatherInfo: {
    type: Object,
    required: true,
  },
});

const today = new Date().toLocaleString('en-EN', { weekday: 'short', year: 'numeric', month: 'long', day: 'numeric' });

const images = require.context("@/assets/img/weather-main", false, /\.png$/);

const weatherImage = computed(() => {
  if (!props.weatherInfo?.weather) return "";

  const fileName = `./${props.weatherInfo.weather[0].description.toLowerCase()}.png`;

  return images.keys().includes(fileName)
    ? images(fileName).default || images(fileName)
    : images("./thunderstorm.png").default || images("./thunderstorm.png");
});
</script>

<style lang="scss" scoped>
@import "../assets/styles/main.scss";

.pic-main {
  width: 60px;
  height: 60px;
  margin: 20px 0 12px;
  background-repeat: no-repeat;
  background-position: 50% 50%;
  background-size: contain;
  background-image: url("@/assets/img/weather-main/thunderstorm.png");
}

/* Остальные стили остаются без изменений */
.city {
  font-size: 24px;
}

.weather {
  margin: 0 0 20px;
  padding: 20px 0;
  border-bottom: 1px solid rgba(255, 255, 255, 0.4);
}

.temp {
  padding-bottom: 8px;
  font-size: 32px;
}

.text-block {
  position: relative;
  padding-left: 24px;
  padding-bottom: 8px;
  font-size: 14px;

  &::before {
    content: "";
    position: absolute;
    top: 0;
    left: 0;
    width: 20px;
    height: 20px;
    margin-right: 6px;
    background-repeat: no-repeat;
    background-position: 50% 50%;
    background-size: contain;
  }
}

.weather-desc::before {
  background-image: url("/src/assets/img/weather.svg");
}

.city::before {
  background-image: url("/src/assets/img/location.svg");
}

.date::before {
  left: 2px;
  width: 15px;
  height: 15px;
  background-image: url("/src/assets/img/calendar.svg");
}
</style>
