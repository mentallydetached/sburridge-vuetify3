<template>
  <v-container>
    <div class="uploadsongform">
      <form @submit.prevent="uploadSong()">
        <h3>Upload Song</h3>
        <v-alert type="success" v-if="showSuccess">Track submitted</v-alert>
        <v-alert type="error" v-if="showFail">Error submitting track</v-alert>

        <div>
          <v-text-field
            type="text"
            label="Song Name"
            v-model.trim="songName"
          ></v-text-field>
          <v-text-field
            type="text"
            label="Artist Name"
            v-model.trim="artistName"
          ></v-text-field>

          <v-combobox
            v-model="songGenre"
            :items="items"
            :search-input.sync="search"
            label="Genre"
            hint="Maximum of 5 tags"
            multiple
            chips
          >
            <template v-slot:selection="data">
              <v-chip
                :key="JSON.stringify(data.item)"
                v-bind="data.attrs"
                :input-value="data.selected"
                :disabled="data.disabled"
                @click:close="data.parent.selectItem(data.item)"
              >
                <v-avatar
                  class="accent white--text"
                  left
                  v-text="data.item.slice(0, 1).toUpperCase()"
                ></v-avatar>
                {{ data.item }}
              </v-chip>
            </template>

            <template v-slot:no-data>
              <v-list-item>
                <v-list-item-content>
                  <v-list-item-title>
                    No results matching "<strong>{{ search }}</strong
                    >". Press <kbd>enter</kbd> to create a new one
                  </v-list-item-title>
                </v-list-item-content>
              </v-list-item>
            </template>
          </v-combobox>

          <v-file-input
            :rules="rules"
            v-model="file"
            accept="audio/mpeg"
            prepend-icon="mdi-music-note"
            label="MP3 File"
          ></v-file-input>

          <v-btn
            outlined
            class="button primary white--text"
            @click="uploadSong()"
            >Upload Track</v-btn
          >
        </div>
      </form>
    </div>
  </v-container>
</template>

<script>
// @ is an alias to /src

export default {
  name: "uploadsongform",
  components: {},
  data: () => ({
    showSuccess: false,
    showFail: false,
    songName: "",
    artistName: "",
    items: ["EDM", "Rock", "Rap", "R&B"],
    songGenre: [],
    file: null,

    search: null,
    rules: [
      (value) => (
        !value ||
          value.size < 10000000 ||
          "Profile file size should be less than 10 MB."),
      (value) => (
          !value ||
          ["audio/mpeg"].includes(value.type) ||
          "Only mp3 files are supported at this time."
      ),
    ],
  }),
  methods: {
    uploadSong() {
      this.showFail = false;
      if (!this.isDisabled) {
        this.$store.dispatch("uploadSong", {
          songName: this.songName,
          artistName: this.artistName,
          songGenre: this.songGenre,
          songFile: this.file,
        });
        this.showSuccess = true;
        this.songName = "";
        this.artistName = "";
        this.songGenre = [];
        this.file = null;

        setTimeout(() => {
          this.showSuccess = false;
        }, 2000);
      } else {
        this.showFail = true;
      }
    },
  },
  watch: {
    songGenre(val) {
      if (val.length > 5) {
        this.$nextTick(() => this.songGenre.pop());
      }
    },
  },
};
</script>