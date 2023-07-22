<template>
  <div class="field">
    <form @submit.prevent="searching">
      <InputText
        placeholder="Search..."
        v-model="searched"
        class="p-inputtext-lg"
        @keyup="highlighting"
      />
      <Button
        label="Search"
        type="submit"
        icon="pi pi-search"
        class="p-button-secondary p-button-lg button-search"
      />
    </form>
  </div>
</template>

<script lang="ts">
import InputText from "primevue/inputtext";
import Button from "primevue/button";

import { defineComponent, ref } from "vue";

export default defineComponent({
  name: "Search",
  components: { InputText, Button },
  emits: ["searching-user"],
  setup(_, { emit }) {
    const searched = ref("");

    function highlighting(event: { key: string; type: string }) {
      if (event.key !== "Enter" && event.type !== "submit") {
        emit("searching-user", {
          type: "highlight",
          searching: searched.value,
        });
      }
    }

    function searching(event: { key: string; type: string }) {
      if (event.key === "Enter" || event.type === "submit") {
        emit("searching-user", { type: "search", searching: searched.value });
      }
    }

    return {
      searched,
      searching,
      highlighting,
    };
  },
});
</script>

<style scoped>
.field {
  margin: 10px 0px;
}
.button-search {
  margin-left: 5px;
}
</style>