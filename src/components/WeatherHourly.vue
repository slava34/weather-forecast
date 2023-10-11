<template>
  <div class="max-w-screen-md w-full py-10">
    <div class="mx-8 text-white">
      <h2 class="mb-8">Почасовая</h2>
      <div class="flex gap-10 overflow-x-scroll">
        <div
            v-for="hourData in weatherData.hourly"
            :key="hourData.dt"
            class="flex flex-col gap-4 items-center"
        >
          <p class="whitespace-nowrap text-md">
            {{
              new Date(
                  hourData.dt * 1000
              ).toLocaleTimeString("ru", {
                hour: "2-digit",
                minute: "2-digit"
              })
            }}
          </p>
          <img
              class="w-auto h-[50px] object-cover"
              :src="
                `${weatherImageUrl}${hourData.weather[0].icon}@2x.png`
              "
          />
          <p class="text-xl">
            {{ Math.round(hourData.temp) }}&deg;
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "WeatherHourly",
  props: {
    city: Object,
    weatherData: Object,
    weatherImageUrl: String
  }
}
</script>

<style scoped>

</style>