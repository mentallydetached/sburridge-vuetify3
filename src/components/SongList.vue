 <template>
  <div class="querysongs">
    <v-row>
      <v-col
        v-for="song in songsList"
        :key="song.songURL"
        class="col-12 col-sm-6 col-md-4"
      >
        <v-card
          class="mx-auto text-left"
          outlined
          max-width="400"
          @click="
            updateMusicPlayer(song.songURL, song.artistName, song.songName)
          "
        >
          <v-list-item two-line>
            <v-list-item-avatar
              class="headline white--text"
              style="background-size: cover; text-shadow: 0px 0px 7px #000"
              :style="{
                backgroundImage:
                  'url(' + require('@/assets/blurrybackgroundsquare.png') + ')',
              }"
              >{{ songTitleFirstLetter(song.songName) }}</v-list-item-avatar
            >

            <v-list-item-content>
              <v-list-item-subtitle>{{ song.artistName }}</v-list-item-subtitle>
              <v-list-item-title class="headline">{{
                song.songName
              }}</v-list-item-title>
            </v-list-item-content>
          </v-list-item>
          <v-row class="mb-1">
            <v-col class="py-0 mb-1 mx-4">
              <v-rating background-color="primary" small></v-rating>
            </v-col>
            <v-col class="py-0 mb-1 mx-4 align-self-center">
              <div
                v-for="genre in song.genre"
                :key="genre.id"
                class="caption text-right text--disabled"
              >
                {{ genre }}
              </div>
            </v-col>
          </v-row>
        </v-card>
      </v-col>
    </v-row>
  </div>
</template>

<script>
import { mapState } from "vuex";

export default {
  name: "querysongs",
  components: {},
  data: () => ({
    searchTerm: "",
  }),
  mounted() {
    this.getSongList("artistNameLower", "Mentally Detached");
  },
  computed: {
    ...mapState(["songsList"]),
  },
  methods: {
    getSongList(searchtype, searchterm) {
      this.$store.dispatch("getSongs", {
        field: searchtype,
        value: searchterm,
      });
    },
    songTitleFirstLetter(name) {
      return name.charAt(0);
    },
    updateMusicPlayer(link, artist, song) {
      this.$store.dispatch("setPlayerData", {
        link: link,
        artist: artist,
        song: song,
      });
    },
  },
};
</script>
