<template>
  <ion-page v-if="currentBible" id="main-content">
    <!-- PAGE header -->
    <ion-header :translucent="false">
      <!-- BIBLE toolbar -->
      <ion-toolbar>
        <ion-title>{{ currentBible.title }}</ion-title>
        <ion-buttons slot="end">
          <ion-button @click="openModal"
            ><ion-icon slot="icon-only" :icon="bookOutline" size="large"
          /></ion-button>
          <ion-button @click="openModal"
            ><ion-icon slot="icon-only" :icon="bookmarkOutline" size="large"
          /></ion-button>
        </ion-buttons>
      </ion-toolbar>
    </ion-header>
    <!-- CONTENT -->
    <ion-content :fullscreen="true" class="ion-padding">
      <!-- SELECT TESTAMENT -->
      <div>
        <ion-segment :value="testament" @ion-change="segmentChanged($event)">
          <ion-segment-button value="old">
            <ion-label style="font-size: large">Old Testament</ion-label>
          </ion-segment-button>
          <ion-segment-button value="new">
            <ion-label style="font-size: large">New Testament</ion-label>
          </ion-segment-button>
        </ion-segment>
      </div>
      <br />
      <!-- LIST OF CHAPTERS -->
      <ion-list lines="inset" style="margin: 0px 25px 0px 20px">
        <!-- @vue-ignore -->
        <div
          v-for="(value, key, i) in currentBible[testament]"
          :key="key"
          @click="goToReaderPage(key.toString())"
        >
          <ion-item class="global-cursor-hover">
            <ion-label>
              <span style="color: grey; font-size: small">
                <!-- @vue-ignore -->{{ i + 1 }}) </span
              >&nbsp;&nbsp;
              <span style="font-size: x-large">{{ key }}</span></ion-label
            >
            <ion-icon
              :icon="chevronForward"
              slot="end"
              style="color: grey"
            ></ion-icon>
          </ion-item>
        </div>
      </ion-list>
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import { ref, onMounted } from "vue";
import { useRouter } from "vue-router";
import {
  IonContent,
  IonHeader,
  IonPage,
  IonTitle,
  IonToolbar,
  IonButtons,
  IonButton,
  IonIcon,
  IonSegment,
  IonSegmentButton,
  IonLabel,
  IonList,
  IonItem,
  modalController,
} from "@ionic/vue";
import { bookOutline, bookmarkOutline, chevronForward } from "ionicons/icons";
import BIBLES from "../schema/bibles.json";
import { BiblesType } from "../interfaces";
import BibleModalPicker from "@/components/BibleModalPicker.vue";

//import packageJson from "../../package.json";
//const version = ref(packageJson.version);

const bibles: BiblesType = BIBLES;

// GET THE USER'S DEFAULT BIBLE IDENTIFIER FROM LOCALSTORAGE
const identifier = ref(localStorage.getItem("bibleIdentifier") || "bsb");
const currentBible = ref();

// Used to display either the old or new testament
const testament = ref("new");

// USER SELECTS A DIFFERENT TESTAMENT
function segmentChanged(ev: any) {
  if (ev.detail.value === "new") testament.value = "new";
  else testament.value = "old";
}

// OPENS THE BIBLE PICKER MODAL
const isOpen = ref(false);
//const setOpen = (open: boolean) => (isOpen.value = open);

// GO TO READER PAGE
const router = useRouter();
const goToReaderPage = (book: string) => {
  router.push({
    path: `/reader`,
    query: {
      identifier: identifier.value,
      book: book,
      testament: testament.value,
    },
  });
};

// BIBLE MODAL PICKER
const openModal = async () => {
  const modal = await modalController.create({
    component: BibleModalPicker,
    componentProps: {
      identifier: identifier.value,
    },
  });
  modal.present();
  const { data, role } = await modal.onWillDismiss();
  if (role === "confirm") {
    console.log(data);
    if (data) {
      identifier.value = data;
      localStorage.setItem("bibleIdentifier", identifier.value);
      currentBible.value = bibles[identifier.value];
    }
  }
};

/**
 * Tried using onIonViewWillEnter here but it would not fire.
 */
onMounted(() => {
  // Get the last know bible identifier or use bsb
  identifier.value = localStorage.getItem("bibleIdentifier") || "bsb";
  // Set the current bible
  currentBible.value = bibles[identifier.value];
});
</script>
