<template>
  <main class="container">
    <h2>My App</h2>
    <Search @searching-user="searching" />
    <div class="chip">
      <Chip :label="highlightCount.toString()" icon="pi pi-user" class="chipHighlight" />
    </div>
    <ul>
      <li
        v-for="user in filteredUsers"
        :key="user.id"
        :class="{
          highlight:
            highlight.length > 0 &&
            user.firstName.toLowerCase().includes(highlight.toLowerCase()),
        }"
      >
        {{ user.firstName }} {{ user.lastName }}
      </li>
    </ul>
  </main>
</template>

<script setup lang="ts">
import { reactive, onMounted, ref } from "vue";
import Search from "@/components/Search.vue";
import Chip from "primevue/chip";
import fakeData from "@/assets/json/MOCK_DATA.json";
import User from "@/interfaces/User";
import SearchData from "@/interfaces/SearchData";

const users: User[] = reactive([]);
const highlight = ref("");
const filteredUsers = ref(users);
const highlightCount = ref(0);

onMounted(() => {
  users.push(...fakeData);
});

const searching = (searched: SearchData) => {
  if (searched.type === "search") {
    highlight.value = searched.searching;
    filteredUsers.value = performFilter(searched.searching);
  } else {
    highlight.value = "";
    filteredUsers.value = users;
  }
  updateHighlightCount();
};

const performFilter = (searchTerm: string): User[] => {
  const lowerCaseSearchTerm = searchTerm.toLowerCase();
  return users.filter((user) =>
    user.firstName.toLowerCase().includes(lowerCaseSearchTerm)
  );
};

const updateHighlightCount = () => {
  const count = filteredUsers.value.reduce((total, user) => {
    if (highlight.value && user.firstName.toLowerCase().includes(highlight.value.toLowerCase())) {
      return total + 1;
    } else {
      return total;
    }
  }, 0);
  highlightCount.value = count;
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.container {
  max-width: 900px;
  margin: 0 auto;
  padding: 10px;
  text-align: center;
}

ul {
  list-style: none;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
}

ul li {
  background-color: aliceblue;
  color: black;
  padding: 5px;
  margin-right: 2px;
  margin-bottom: 5px;
  border: solid 1px #332;
}

.highlight {
  background-color: blueviolet;
  color: cornsilk;
}

.chip {
  margin-bottom: 10px;
}

.chip .chipHighlight {
  background-color: blue;
  color: white;
}
</style>