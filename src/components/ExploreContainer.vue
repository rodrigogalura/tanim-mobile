<template>
  <div id="container">
    <ion-list>
      <ion-item>
        <ion-input
          label="email"
          type="email"
          placeholder="Enter Email"
          v-model="email"
        ></ion-input>
      </ion-item>
      <ion-item>
        <ion-input
          label="password"
          type="password"
          placeholder="Enter Password"
          v-model="password"
        >
          <ion-input-password-toggle slot="end"></ion-input-password-toggle>
        </ion-input>
      </ion-item>
    </ion-list>

    <ion-button expand="full" @click="login">Login</ion-button>

    <ion-toast
      :is-open="isOpen"
      :message="toastMessage"
      :duration="3000"
      @didDismiss="closeToast"
    >
    </ion-toast>
  </div>
</template>

<script setup lang="ts">
import {
  IonList,
  IonItem,
  IonInput,
  IonInputPasswordToggle,
  IonButton,
  IonToast,
} from "@ionic/vue";

import { ref } from "vue";
import axios from "axios";

// axios.defaults.withCredentials = true;
// axios.defaults.withXSRFToken = true;

const email = ref();
const password = ref();

const isOpen = ref(false);
const toastMessage = ref();

const openToast = (message: string) => {
  isOpen.value = true;
  toastMessage.value = message;
};

const closeToast = () => {
  isOpen.value = false;
};

function login() {
  openToast("Logging in...");

  axios
    .post(`${import.meta.env.VITE_API_URL}/api/sanctum/token`, {
      email: email.value,
      password: password.value,
      device_name: navigator.platform || "Device Name",
    }, {
      headers: {
        'Accept': 'application/json'
      }
    })
    .then(function (response) {
      openToast("Successfully Logged In");
      console.log(response);
    })
    .catch(function (error) {
      openToast("Invalid Credentials");
      console.log(error);
    });
}
</script>

<style scoped>
#container {
  text-align: center;
  position: absolute;
  left: 0;
  right: 0;
  top: 50%;
  transform: translateY(-50%);
}

#container strong {
  font-size: 20px;
  line-height: 26px;
}

#container p {
  font-size: 16px;
  line-height: 22px;
  color: #8c8c8c;
  margin: 0;
}

#container a {
  text-decoration: none;
}
</style>
