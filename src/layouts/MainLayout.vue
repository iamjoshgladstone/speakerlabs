<template>
  <q-layout view="lHh Lpr lFf">
    <q-header bordered class="bg-dark">
      <q-toolbar class="flex-center">
        <img
          class="sl-logo"
          src="https://www.speakerlabs.ca/wp-content/uploads/2017/01/logo_menu.png"
          alt="Speaker Labs"
          data-no-retina=""
        />
      </q-toolbar>
    </q-header>

    <q-page-container>
      <router-view />
    </q-page-container>

    <q-footer bordered class="bg-dark text-primary">
      <q-tabs
        no-caps
        active-color="primary"
        indicator-color="transparent"
        class="text-white"
      >
        <q-route-tab to="/" name="Home" label="Home" icon="home"></q-route-tab>
        <q-route-tab
          to="/curveballs"
          name="Curveballs"
          label="Curveballs"
          icon="sports_baseball"
        ></q-route-tab>
      </q-tabs>
    </q-footer>
  </q-layout>
</template>

<style>
.sl-logo {
  height: auto;
  width: auto;
}
</style>
