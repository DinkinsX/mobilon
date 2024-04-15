<template>
  <div class="sort-bar">
    <v-tooltip text="Сортировка по возрастанию рейтинга">
      <template v-slot:activator="{ props }">
        <v-btn
            v-bind="props"
            icon="mdi-sort-numeric-descending"
            density="compact"
            variant="tonal"
            class="sort-btn"
            color="blue"
            @click="sortListDecreasing" />
      </template>
    </v-tooltip>

    <v-tooltip text="Сортировка по убыванию рейтингу">
      <template v-slot:activator="{ props }">
        <v-btn
            v-bind="props"
            icon="mdi-sort-numeric-ascending"
            density="compact"
            variant="tonal"
            class="sort-btn"
            color="blue"
            @click="sortListIncrease" />
      </template>
    </v-tooltip>

    <v-tooltip text="Сортировка по умолчанию">
      <template v-slot:activator="{ props }">
        <v-btn
            v-bind="props"
            icon="mdi-sort"
            density="compact"
            variant="tonal"
            class="sort-btn"
            color="blue"
            @click="sortListDefault" />
      </template>
    </v-tooltip>
  </div>
</template>

<script setup>
import {inject, ref, watch} from "vue";

  const firstList = inject('firstList');
  const secondList = inject('secondList');
  const lastList = inject('lastList');
  let cards = ref([]);

  const props = defineProps({
    options: {},
  });

  let defaultCards = []

  function getLocalCards() {
    switch (props.options.id) {
      case 1:
        cards = firstList;
        break;
      case 2:
        cards = secondList;
        break;
      case 3:
        cards = lastList;
        break;
    }
  }

  function getGlobalCards() {
    switch (props.options.id) {
      case 1:
        return firstList;
      case 2:
        return secondList;
      case 3:
        return lastList;
    }
  }

  function sortListDecreasing() {
    getLocalCards();
    cards.value = cards.value.sort((a, b) => b.rating.rate - a.rating.rate);
  }

  function sortListIncrease() {
    getLocalCards();
    cards.value = cards.value.sort((a, b) => a.rating.rate - b.rating.rate);
  }

  function sortListDefault() {
    getLocalCards();
    cards.value = cards.value.sort((a, b) => defaultCards.indexOf(a.id) - defaultCards.indexOf(b.id));
  }

  watch(
      () => getGlobalCards().value,
      (newArr, oldArr) => {
        if(newArr.length !== oldArr.length) defaultCards = newArr.map(item => item.id)
      }, { deep: true }
  )
</script>

<style lang="scss" scoped>
  .sort-bar {
    display: flex;
    justify-content: space-around;
    width: 100%;
    background-color: white;
    border-radius: 10px;
    padding: 10px;
    margin-bottom: 10px;
  }
</style>
