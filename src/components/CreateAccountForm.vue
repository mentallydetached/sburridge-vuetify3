<template>
  <v-container>
    <div class="createaccount">
      <form @submit.prevent="login()">
        <h3>Create new account</h3>
        <div>
          <v-text-field
            type="text"
            label="Name"
            v-model.trim="name"
          ></v-text-field>
          <v-text-field
            type="text"
            label="Email"
            v-model.trim="email"
            placeholder="you@email.com"
          ></v-text-field>

          <v-text-field
            :append-icon="showPass ? 'mdi-eye' : 'mdi-eye-off'"
            :type="showPass ? 'text' : 'password'"
            label="Password"
            placeholder="minimum 6 characters"
            v-model.trim="password"
            @click:append="showPass = !showPass"
          ></v-text-field>

          <v-file-input
            :rules="rules"
            v-model="file"
            accept="image/png, image/jpeg, image/bmp"
            prepend-icon="mdi-camera"
            label="Profile Image"
          ></v-file-input>

          <v-btn outlined class="button primary white--text" @click="signup()"
            >Sign Up</v-btn
          >
        </div>
      </form>
    </div>
  </v-container>
</template>

<script>
// @ is an alias to /src

export default {
  name: "createaccount",
  components: {},
  data: () => ({
    showPass: false,
    name: "",
    email: "",
    password: "",
    file: null,
    rules: [
      (value) =>
        !value ||
        value.size < 2000000 ||
        "Profile image size should be less than 2 MB.",
      (value) =>
        !value ||
        ["image/png", "image/jpeg", "image/bmp"].includes(value.type) ||
        "Please ensure you've selected an image format.",
    ],
  }),
  methods: {
    signup() {
      this.$store.dispatch("signup", {
        email: this.email,
        password: this.password,
        name: this.name,
        profileImage: this.file,
      });
    },
  },
};
</script>