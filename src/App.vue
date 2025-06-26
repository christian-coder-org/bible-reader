<template>
  <ion-app>
    <ion-router-outlet />
  </ion-app>
</template>

<script setup lang="ts">
import { IonApp, IonRouterOutlet } from "@ionic/vue";
//import { SplashScreen } from "@capacitor/splash-screen";

// Show the splash for two seconds and then automatically hide it:

// Hide the splash (you should do this on app launch)
//await SplashScreen.hide();

// Show the splash for an indefinite amount of time:
/*await SplashScreen.show({
  autoHide: false,
});*/

/*await SplashScreen.show({
  showDuration: 2000,
  autoHide: true,
});*/
</script>

<style lang="css">
/* This style can be used globally as class definitions, class="global-cursor-hover" */
.global-cursor-hover {
  cursor: pointer;
}
</style>
