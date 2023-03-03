<template>
  <main>
    <Alert :status="status" :type="type" :message="message" />
    <template v-if="mode === 'menu'">
      <ListMenu />
    </template>
    <template v-if="mode === 'form'">
      <Form @back="onMenu" />
    </template>
    <template v-if="mode === 'otp_verification'">
      <OtpPage @back="onMenu" />
    </template>
    <template v-if="mode === 'dob_verification'">
      <DobPage @back="onMenu" />
    </template>
    <template v-if="mode === 'camera_capture'">
      <CameraPage />
    </template>
  </main>
</template>

<script setup>
import Form from "~~/components/Landing/Form.vue";
import ListMenu from "~~/components/Landing/Menu/ListMenu.vue";
import OtpPage from "~~/components/Landing/OTP/OtpPage.vue";
import DobPage from "~~/components/Landing/DOB/DobPage.vue";
import CameraPage from "~~/components/Landing/Camera/CameraPage.vue";
import Alert from "~~/components/Alert.vue";

import { storeToRefs } from "pinia";
import { useModeStore } from "~~/stores/mode";
// import { useViewStore } from "~~/stores/view";
import { useAlertStore } from "~~/stores/alert";

const store = useModeStore();
const { mode } = storeToRefs(store);
store.setMode("menu");

function onMenu() {
  store.setMode("menu");
}
// const storeView = useViewStore();
// const pathArray = window.location.pathname.split("/");
// const path = pathArray[1];
// storeView.setView(path);

const alertStore = useAlertStore();
const { status, type, message } = storeToRefs(alertStore);
watch(status, (newStatus) => {
  if (newStatus) {
    setTimeout(() => {
      alertStore.setAlert({
        status: false,
        type: "",
        message: "",
      });
    }, 1000);
  }
});
</script>
