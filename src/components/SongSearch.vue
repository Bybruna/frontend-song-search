<template>
  <div class="song-search">
    <h1>Search Songs by Band</h1>
    <input
      v-model="bandName"
      placeholder="Enter band name"
      @keyup.enter="fetchSongs"
    />
    <button @click="fetchSongs">Search</button>

    <table v-if="songs.length" class="song-table">
      <thead>
        <tr>
          <th>Song Name</th>
          <th>Album Name</th>
          <th>Preview URL</th>
          <th>Price</th>
          <th>Release Date</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="song in songs" :key="song.cancion_id">
          <td>{{ song.nombre_tema }}</td>
          <td>{{ song.nombre_album }}</td>
          <td>
            <a :href="song.preview_url" target="_blank">Preview</a>
          </td>
          <td>{{ song.precio.valor }} {{ song.precio.moneda }}</td>
          <td>{{ formatDate(song.fecha_lanzamiento) }}</td>
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
      bandName: "",
      songs: [],
    };
  },
  methods: {
    async fetchSongs() {
      if (!this.bandName) return;

      try {
        const response = await axios.get(
          `${process.env.VUE_APP_MUSIC_API}/tracks/searchTracks?`,
          {
            params: { name: this.bandName },
          }
        );
        this.songs = response.data.canciones;
      } catch (error) {
        console.error("Error fetching songs:", error);
        alert("Could not fetch songs. Please try again.");
      }
    },
    formatDate(dateString) {
      const options = { year: "numeric", month: "long", day: "numeric" };
      return new Date(dateString).toLocaleDateString(undefined, options);
    },
  },
};
</script>

<style scoped>
.song-search {
  text-align: center;
  margin-top: 20px;
}

.song-table {
  width: 100%;
  margin-top: 20px;
  border-collapse: collapse;
}

.song-table th,
.song-table td {
  padding: 8px;
  border: 1px solid #ddd;
  text-align: center;
}
</style>
