<template>
  <!-- PAGE header -->
  <ion-header>
    <ion-toolbar>
      <ion-buttons slot="end">
        <ion-button @click="cancel">Close</ion-button>
      </ion-buttons>
      <ion-title>Select Bible</ion-title>
    </ion-toolbar>
  </ion-header>

  <ion-content>
    <!-- LIST OF BIBLES -->
    <ion-list lines="inset">
      <div v-for="(value, key) in bibles" :key="key">
        <ion-item @click="confirm(key)" class="global-cursor-hover">
          <ion-icon
            v-if="identifier === key"
            style="color: grey"
            :icon="bookOutline"
            slot="start"
          ></ion-icon>
          <ion-icon
            v-else
            style="color: grey"
            :icon="chevronBack"
            slot="start"
          ></ion-icon>
          <ion-label style="font-size: x-large">
            {{ value.title }}
          </ion-label>
        </ion-item>
      </div>
    </ion-list>
  </ion-content>
</template>

<script setup lang="ts">
import { ref } from "vue";
import {
  IonList,
  IonItem,
  IonLabel,
  IonIcon,
  IonHeader,
  IonButtons,
  IonButton,
  IonToolbar,
  IonTitle,
  IonContent,
  modalController,
} from "@ionic/vue";
import { chevronBack, bookOutline } from "ionicons/icons";
import BIBLES from "../schema/bibles.json";

const props = defineProps(["identifier"]);
const identifier = ref(props.identifier);
const bibles = ref(BIBLES);

const cancel = () => modalController.dismiss(null, "cancel");
const confirm = (identifier: string) =>
  modalController.dismiss(identifier, "confirm");
</script>
