<template>
  <ion-header>
    <ion-toolbar>
      <ion-buttons slot="end">
        <ion-button @click="cancel">Close</ion-button>
      </ion-buttons>
      <ion-title>Select chapter</ion-title>
    </ion-toolbar>
  </ion-header>

  <ion-content>
    <br />
    <ion-list v-if="verses.length > 0">
      <ion-item @click="confirm(n)" v-for="n in props.chaptersCnt" :key="n">
        <ion-icon
          style="color: grey; vertical-align: top-text"
          :icon="chevronBack"
          slot="start"
        ></ion-icon>
        <div>
          <div class="chapter">{{ book }} - {{ n }}</div>
          <div class="text-block">{{ verses[n - 1] }}...</div>
        </div>
      </ion-item>
    </ion-list>
  </ion-content>
</template>

<script lang="ts" setup>
import {
  IonContent,
  IonHeader,
  IonTitle,
  IonToolbar,
  IonButtons,
  IonButton,
  IonLabel,
  IonList,
  IonItem,
  IonIcon,
  modalController,
} from "@ionic/vue";
import { chevronBack } from "ionicons/icons";
import { ref } from "vue";

const props = defineProps(["identifier", "book", "chaptersCnt"]);
// @ts-ignore
const verses: string[] = ref([]);

// FETCH chapter verses when the chapter number changes
async function getChapterFirstVerse(chapter: number) {
  const path = `/chapters/${props.identifier}/${props.book}/${chapter}.json`;
  try {
    const response = await fetch(path);
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }
    const data = await response.json();
    // @ts-ignore
    verses.value.push(data[1].substr(0, 80));
  } catch (error) {
    console.error("Fetching error:", error);
  }
}

async function getVerses() {
  for (let i = 1; i < props.chaptersCnt + 1; i++) {
    getChapterFirstVerse(i);
  }
}

getVerses();

const cancel = () => modalController.dismiss(null, "cancel");
const confirm = (chapter: number) =>
  modalController.dismiss(chapter, "confirm");
</script>
<style lang="css">
.chapter {
  font-size: large;
}
.text-block {
  margin-left: 30px;
  padding-bottom: 20px;
  font-size: x-large;
}
</style>
