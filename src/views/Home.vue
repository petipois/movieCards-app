<template>
  <ion-page>
    <ion-content>
    <card-game></card-game>
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import CardGame from '../components/CardGame.vue';
export default defineComponent({
  name: "Home",
  components: {
CardGame
  },
});
</script>