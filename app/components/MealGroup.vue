<script>
import { meals } from '@/src/util'
import { meal } from '@/src/api'

export default {
  name: 'MealGroup',

  data () {
    return {
      meal: {
        breakfast: '',
        lunch: '',
        dinner: '',
        snack: ''
      },
      pending: false
    }
  },

  computed: {
    meals: () => meals
  },

  async created () {
    this.meal = await meal.getTodayMeal()
  }
}
</script>

<template>
  <div class="meal-list">
    <dimi-meal-item
      v-for="(m, i) in meals"
      :key="i"
      :current="m.isActive()"
      :meal="m.name">
      {{ meal[m.key] | prettyMeal }}</dimi-meal-item>
  </div>
</template>

<style lang="scss">
@import '~styles/variables';

.meal-list {
  color: $gray;
}

</style>
