<template>
  <ion-page id="main-content">
    <!-- PAGE header -->
    <ion-header :translucent="false">
      <!-- COMPARE toolbar -->
      <ion-toolbar>
        <ion-buttons slot="start">
          <ion-back-button></ion-back-button>
        </ion-buttons>
        <ion-title>Verse comparison </ion-title>
      </ion-toolbar>
    </ion-header>

    <!-- CONTENT -->
    <ion-content v-show="verses">
      <br />
      <ion-list lines="inset" style="margin: 0px 5px 0px 20px">
        <ion-item v-for="(value, key) in verses" :key="key">
          <span
            :style="identifier === value.identifier ? 'color: #0099FF;' : ''"
          >
            <div style="font-size: large; text-align: left">
              {{ value.bibleTitle }}
            </div>

            <div class="chapter">{{ book }} {{ chapter }}:{{ verse }}</div>
            <div class="text-block">{{ value.verse }}</div>
          </span>
        </ion-item>
      </ion-list>
    </ion-content>
  </ion-page>
</template>

<script setup>
import { ref } from "vue";
import {
  IonPage,
  IonHeader,
  IonToolbar,
  IonButtons,
  IonBackButton,
  IonContent,
  IonTitle,
  IonList,
  IonItem,
  onIonViewWillEnter,
} from "@ionic/vue";
import { useRoute } from "vue-router";
import BIBLES from "../schema/bibles.json";

// GET QUERY PARAMS, SET REFS
const route = useRoute();
//const router = useRouter();
const queryParams = route.query;

// SET REFS
const identifier = ref(queryParams.identifier);
const book = ref(queryParams.book);
const chapter = ref(queryParams.chapter);
const verse = ref(queryParams.verse);
const verses = ref([]);

// FETCH chapter verses when the chapter number changes
async function getChapterVerse(focusIdentifier, identifier, bibleTitle) {
  const path = `/chapters/${identifier}/${book.value}/${chapter.value}.json`;
  try {
    const response = await fetch(path);
    if (!response.ok) {
      throw new Error(`Fetch error status: ${response.status}`);
    }
    const data = await response.json();
    verses.value.push({
      bibleTitle: bibleTitle,
      identifier: identifier,
      verse: data[verse.value],
    });
  } catch (error) {
    console.error("Fetching error:", error);
  }
}

async function getVerses() {
  for (const key in BIBLES) {
    await getChapterVerse(identifier.value, key, BIBLES[key].title);
  }
  verses.value = verses.value.sort((a, b) => (a.title > b.title ? -1 : 1));
}

onIonViewWillEnter(() => {
  getVerses();
});
</script>
