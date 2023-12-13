<template>
  <div>
    <!-- <div v-for="post in posts" :key="post.id" style="{ border: '1px dashed', marginBottom: '5px' }">
      {{ post.title }}
    </div> -->
    {{ value }}
    <br />
    Date: {{ date.toString() }}
    <br />
    Day: {{ getDay() }}
    <br />
    Month: {{ getMonth() }}
    <br />
    <input type="text" v-model="input" @input="handleChange" />
    <button @click="changeTheWorld">Change The World</button>
  </div>
</template>

<script>
import { ref, onMounted, onBeforeUnmount } from "vue";
import useHelloWorld from "./components/HelloWorld.vue";

const useDate = () => {
  const date = new Date();

  const getDay = () => date.getDate();
  const getMonth = () => date.getMonth() + 1;

  const addDay = (numberOfDays) => {
    const newDate = new Date(date);
    newDate.setDate(newDate.getDate() + numberOfDays);
    return newDate;
  };

  const addMonth = (numberOfMonths) => {
    const newDate = new Date(date);
    newDate.setMonth(newDate.getMonth() + numberOfMonths);
    return newDate;
  };

  return { date, getDay, getMonth, addDay, addMonth };
};

export default {
  setup() {
    const posts = ref([]);
    const input = ref("");
    const { value, setValue } = useHelloWorld();
    const { date, getDay, getMonth } = useDate();

    onMounted(async () => {
      try {
        const response = await fetch(
          "https://jsonplaceholder.typicode.com/todos?limit=5"
        );
        const data = await response.json();
        console.log(data);
        posts.value = data;
      } catch (error) {
        console.log(error);
      }
    });

    onBeforeUnmount(() => {
      console.log("I am unmounting...");
    });

    const handleChange = (event) => {
      input.value = event.target.value;
    };

    const changeTheWorld = () => {
      setValue(input.value);
    };

    return {
      posts,
      input,
      value,
      date,
      getDay,
      getMonth,
      handleChange,
      changeTheWorld,
    };
  },
};
</script>

<style scoped>
/* Add your styles here */
</style>
