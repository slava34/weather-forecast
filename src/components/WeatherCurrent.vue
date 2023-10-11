<template>
  <div class="flex flex-col items-center text-white py-12">
    <h1 v-if="city" class="text-4xl mb-2">{{city.text_ru}}</h1>
    <h1 v-else class="text-2xl mb-2">Ваше местоположение</h1>
    <p class="text-sm mb-12">
      {{
        new Date(weatherData.current.dt * 1000).toLocaleDateString(
            "ru",
            {
              weekday: "short",
              day: "2-digit",
              month: "long",
            }
        )
      }}
      {{
        new Date(weatherData.current.dt * 1000).toLocaleTimeString(
            "ru",
            {
              timeStyle: "short",
            }
        )
      }}
    </p>
    <p class="text-8xl mb-8">
      {{ Math.round(weatherData.current.temp) }}&deg;
    </p>
    <p>
      Ощущается как
      {{ Math.round(weatherData.current.feels_like) }} &deg;
    </p>
    <p class="capitalize">
      {{ weatherData.current.weather[0].description }}
    </p>
    <img
        class="w-[150px] h-auto"
        :src="
          `${weatherImageUrl}${weatherData.current.weather[0].icon}@2x.png`
        "
    />
  </div>
</template>

<script>
export default {
  name: "WeatherCurrent",
  props: {
    city: Object,
    weatherData: Object,
    weatherImageUrl: String
  }
}
</script>

<style scoped>

</style>