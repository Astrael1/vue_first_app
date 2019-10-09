<template>
    <div class="wrapper">
      <h1>Search</h1>
      <div class="search">
        <input
          id="search"
          name="search"
          v-model="searchValue"
          @input="handleInput"
        />
      </div>
      <ul>
        <li v-for="item in results" :key="item.data[0].nasa_id">
          <p>{{ item.data[0].description }}</p>
        </li>
      </ul>
    </div>
</template>

<script>
import axios from 'axios';
import debounce from 'lodash.debounce';

const API = 'https://images-api.nasa.gov';
export default {
  name: 'Search',
  data() {
    return {
      searchValue: '',
      results: [],
    };
  },
  methods: {
    handleInput: debounce(function () {
      axios.get(`${API}/search?q=${this.searchValue}&media_type=image`)
        .then((response) => {
          this.results = response.data.collection.items;
        })
        .catch((error) => {
          console.log(error);
        });
    }, 1000),
  },
};
</script>

<style type="scss" scoped>
  .wrapper{
    display: flex;
    flex-direction: column;
    align-items: center;
    margin: 0;
    padding: 30px;
    width: 100%;
  }
  .search{
    width: 300px;
    display: flex;
    flex-direction: column;

    label{
      font-family: Calibri,serif;
    }
    input {
      height: 50px;
      border: 0;
      border-bottom: 1px solid black;
    }
  }
</style>
