<template>
  <div>
    <input type="text" v-model="words" placeholder="Search" />
    <button v-on:click="getSearch">Search</button>
    <table>
      <tbody>
        <tr> 
            <td> ART </td>
            <td> ARTIST </td>
            <td> ALBUM </td>
            <td> PRICE </td>
        </tr>
        <tr v-for="val in values" :key="val.trackId">
          <td>
            <img :src="val.artworkUrl100" class="image" />
          </td>
          <td>{{ val.artistName }}</td>
          <td>{{ val.collectionName }}</td>
          <td>{{ val.collectionPrice }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      values: null,
      words: "",
    };
  },
  mounted() {
    this.getSearch();
  },
  methods: {
    getSearch() {
      axios
        .get(
          "https://itunes.apple.com/search?term=" + this.words + "&entity=album"
        )
        .then((response) => {
          this.values = response.data.results;
          console.log(response.data.results);
        })
        .catch((e) => console.log(e));
    },
  },
};
</script>

<style>
</style>