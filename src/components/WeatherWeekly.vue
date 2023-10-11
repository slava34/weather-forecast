<template>
  <div class="max-w-screen-md w-full py-10">
    <div class="mx-8 text-white">
      <h2 class="mb-8 text-2xl text-center">Прогноз на 7 дней</h2>
      <div
          v-for="day in weatherData.daily"
          :key="day.dt"
          class="flex items-center"
      >
        <p class="flex flex-1 justify-start">
          {{
            new Date(day.dt * 1000).toLocaleDateString(
                "ru",
                {
                  weekday: "long",

                }
            ).toUpperCase()
          }}
        </p>
        <img
            class="w-[50px] h-[50px] object-cover"
            :src="
              `${weatherImageUrl}${day.weather[0].icon}@2x.png`
            "
            alt=""
        />
        <div class="flex gap-2 flex-1 justify-end">
          <p>{{ Math.round(day.temp.max) }}&deg;</p>
          <p>{{ Math.round(day.temp.min) }}&deg;</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "WeatherWeekly",
  props: {
    city: Object,
    weatherData: Object,
    weatherImageUrl: String
  }
}
</script>

<style scoped>

</style>