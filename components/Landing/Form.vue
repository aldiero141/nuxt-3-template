<template>
  <div class="flex flex-col justify-center items-center min-h-[80vh]">
    <h1 class="mb-4 font-extrabold text-2xl">Form</h1>
    <div
      class="flex flex-col align-left shadow-xl rounded-lg w-full min-h-[40vh] max-w-[20rem] p-4 border-2 border-gray-300 border-opacity-25"
    >
      <div class="flex flex-col mb-4">
        <label class="mb-2">Name</label>
        <input
          v-model="form.name"
          class="border-2 border-black focus:border-blue-500 rounded px-2"
          type="text"
        />
        <div
          v-if="v$.name.$dirty && v$.name.$invalid"
          class="text-sm text-red-500 capitalize"
        >
          Name Can't be Empty
        </div>
      </div>
      <div class="flex flex-col mb-4">
        <label class="mb-2">Phone</label>
        <input
          v-model="form.phone"
          class="border-2 border-black focus:border-blue-500 rounded px-2"
          type="text"
        />
        <div
          v-if="v$.name.$dirty && v$.phone.$invalid"
          class="text-sm text-red-500 capitalize"
        >
          Phone Can't be Empty
        </div>
      </div>
      <div class="flex flex-col mb-4">
        <label class="mb-2">Address</label>
        <input
          v-model="form.address"
          class="border-2 border-black focus:border-blue-500 rounded px-2"
          type="text"
        />
        <div
          v-if="v$.name.$dirty && v$.address.$invalid"
          class="text-sm text-red-500 capitalize"
        >
          Address Can't be Empty
        </div>
      </div>
      <div class="flex flex-col mb-4">
        <label class="mb-2">Age</label>
        <input
          v-model="form.age"
          class="border-2 border-black focus:border-blue-500 rounded px-2"
          type="text"
        />
        <div
          v-if="v$.name.$dirty && v$.age.$invalid"
          class="text-sm text-red-500 capitalize"
        >
          Age Can't be Empty
        </div>
      </div>

      <Button class="my-4" :text="'Submit'" @click="onSubmit" />
      <Button
        class="mb-4"
        :text="'Back'"
        :type="'danger'"
        @click="$emit('back')"
      />
    </div>
  </div>
</template>

<script setup>
import { reactive, computed } from "vue";
import { useVuelidate } from "@vuelidate/core";
import { required } from "@vuelidate/validators";
import { useAlertStore } from "~~/stores/alert";
import Button from "./Button.vue";

const store = useAlertStore();
const emit = defineEmits(["back"]);
const form = reactive({
  name: "",
  phone: "",
  address: "",
  age: "",
});
const rules = computed(() => {
  return {
    name: { required },
    phone: { required },
    address: { required },
    age: { required },
  };
});
const v$ = useVuelidate(rules, form);

async function onSubmit() {
  const isFormCorrect = await v$.value.$validate();
  if (isFormCorrect) {
    const alert = {
      status: true,
      type: "success",
      message: `Form Submition Success `,
    };
    store.setAlert(alert);
  } else {
    const alert = {
      status: true,
      type: "error",
      message: `Form Submition Invalid`,
    };
    store.setAlert(alert);
  }
}
</script>
