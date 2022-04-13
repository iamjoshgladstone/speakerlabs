<template>
  <q-page class="flex-center">
    <h2>Pick your game</h2>
    <q-btn rounded to="/curveballs" color="primary" text-color="black">
      <q-icon left size="3em" name="sports_baseball" />
      <div>Curveballs</div>
    </q-btn>
  </q-page>
</template>

<script>
import { defineComponent } from "vue";

export default defineComponent({
  name: "PageIndex",
});
</script>

<style></style>
