<template>
  <div>
    <input type="text" v-model="words" placeholder="Search" />
    <button v-on:click="getSearch">Search</button>
    <button v-on:click="getPrev">Prev</button>
    <button v-on:click="getNext">Next</button>
    <table>
      <tbody>
        <tr>
          <td>ART</td>
          <td>ARTIST</td>
          <td>ALBUM</td>
          <td>PRICE</td>
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
      totalValues: null,
      values: null,
      words: "",
      init: null,
      end: null,
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
          this.init = 0;
          this.end = 6;
          this.totalValues = response.data.results;
          this.values = this.totalValues.slice(this.init, this.end);
          console.log(response.data.results[0]);
        })
        .catch((e) => console.log(e));
    },
    getNext() {
      this.init = this.init + 6;
      this.end = this.end + 6;
      if (this.init < this.totalValues.length) {
        this.values = this.totalValues.slice(this.init, this.end);
      } else {
        this.init = this.init - 6;
        this.end = this.end - 6;
        this.values = this.totalValues.slice(this.init, this.end);
      }
    },
    getPrev() {
      this.init = this.init - 6;
      this.end = this.end - 6;
      if (this.init >= 0) {
        this.values = this.totalValues.slice(this.init, this.end);
      } else {
        this.init = this.init + 6;
        this.end = this.end + 6;
        this.values = this.totalValues.slice(this.init, this.end);
      }
    },
  },
};
</script>

<style>
table,
th,
td {
  border: 4px solid black;
  border-collapse: collapse;
  width: 1000px;
}
</style>