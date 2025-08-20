<template>
  <ion-page>
    <!-- Header -->
    <ion-header translucent>
      <ion-toolbar color="primary">
        <ion-title>📷 Capture</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content :fullscreen="true" class="elegant-bg ion-padding">

      <!-- Photo Grid -->
      <ion-grid>
        <ion-row>
          <ion-col size="6" :key="photo.filepath" v-for="photo in photos" class="photo-card">
            <div class="glass-card">
              <ion-img :src="photo.webviewPath" @click="showActionSheet(photo)"></ion-img>
            </div>
          </ion-col>
        </ion-row>
      </ion-grid>

      <!-- Floating Action Button -->
      <ion-fab vertical="bottom" horizontal="center" slot="fixed">
        <ion-fab-button color="tertiary" @click="takePhoto()">
          <ion-icon :icon="camera"></ion-icon>
        </ion-fab-button>
      </ion-fab>
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import { camera, trash, close } from 'ionicons/icons';
import {
  actionSheetController,
  IonPage,
  IonHeader,
  IonFab,
  IonFabButton,
  IonIcon,
  IonToolbar,
  IonTitle,
  IonContent,
  IonImg,
  IonGrid,
  IonRow,
  IonCol,
} from '@ionic/vue';

import { usePhotoGallery, UserPhoto } from '@/composables/usePhotoGallery';

const { takePhoto, photos, deletePhoto } = usePhotoGallery();

const showActionSheet = async (photo: UserPhoto) => {
  const actionSheet = await actionSheetController.create({
    header: 'Photos',
    buttons: [
      {
        text: 'Delete',
        role: 'destructive',
        icon: trash,
        handler: () => {
          deletePhoto(photo);
        },
      },
      {
        text: 'Cancel',
        icon: close,
        role: 'cancel',
      },
    ],
  });
  await actionSheet.present();
};
</script>

<style scoped>
/* Light Mode Elegant Background */
.elegant-bg {
  --background: linear-gradient(135deg, #007bff, #6610f2);
  color: white;
}

/* Dark Mode Background */
@media (prefers-color-scheme: dark) {
  .elegant-bg {
    --background: linear-gradient(135deg, #0d0d0d, #1a1a40);
    color: #e0e0e0;
  }
}

/* Glassmorphism Card */
.glass-card {
  backdrop-filter: blur(14px);
  background: rgba(255, 255, 255, 0.15) !important;
  border-radius: 20px;
  padding: 10px;
  transition: transform 0.25s ease, background 0.25s ease;
  box-shadow: 0 6px 18px rgba(0, 0, 0, 0.25) !important;
  cursor: pointer;
}

.glass-card:hover {
  transform: scale(1.05);
  background: rgba(255, 255, 255, 0.25) !important;
}

/* Dark Mode Card */
@media (prefers-color-scheme: dark) {
  .glass-card {
    background: rgba(255, 255, 255, 0.08) !important;
    box-shadow: 0 6px 16px rgba(0, 0, 0, 0.7) !important;
  }

  .glass-card:hover {
    background: rgba(255, 255, 255, 0.15);
  }
}

/* Photo Styling */
.photo-card ion-img {
  border-radius: 16px;
  object-fit: cover;
  width: 100%;
  height: 160px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.25);
}

/* Dark Mode Photo Shadow */
@media (prefers-color-scheme: dark) {
  .photo-card ion-img {
    box-shadow: 0 4px 16px rgba(0, 0, 0, 0.8);
  }
}

/* FAB */
ion-fab-button {
  box-shadow: 0 6px 18px rgba(0, 0, 0, 0.3);
  border-radius: 50%;
  transform: scale(1);
  transition: transform 0.2s ease;
}

ion-fab-button:active {
  transform: scale(0.9);
}
</style>
