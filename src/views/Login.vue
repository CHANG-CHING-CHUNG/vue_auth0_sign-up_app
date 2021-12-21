<template>
  <div class="h-full d-flex center">
    <b-card>
      <span class="title"> Welcome </span>
      <img class="heart-img" :src="images.heart" />
      <b-form class="px-15" @submit="onSubmit" @reset="onReset" v-if="show">
        <b-card-text> Log in to John's secret area to continue </b-card-text>
        <b-form-group id="input-group-1" label-for="input-1">
          <b-input-group class="mb-2">
            <b-input-group-prepend is-text>
              <b-icon icon="person-fill"></b-icon>
            </b-input-group-prepend>
            <b-form-input
              id="input-1"
              v-model="form.email"
              type="email"
              placeholder="Enter email"
              required
            ></b-form-input>
          </b-input-group>
        </b-form-group>

        <b-form-group id="input-group-2" label-for="input-2">
          <b-input-group class="mb-2">
            <b-input-group-prepend is-text>
              <b-icon icon="lock"></b-icon>
            </b-input-group-prepend>
            <b-form-input
              id="input-2"
              type="password"
              v-model="form.password"
              placeholder="Enter password"
              required
            ></b-form-input>
          </b-input-group>
        </b-form-group>

        <b-button type="submit" variant="primary">Log in</b-button>
        <div class="grey py-20">
          Don't have an account?
          <router-link to="/sign-up"><span>Sign up</span></router-link>
        </div>
      </b-form>
    </b-card>
  </div>
</template>

<script>
export default {
  data() {
    return {
      form: {
        email: "",
        name: "",
        food: null,
        checked: [],
      },
      foods: [
        { text: "Select One", value: null },
        "Carrots",
        "Beans",
        "Tomatoes",
        "Corn",
      ],
      show: true,
      images: {
        heart: require("../assets/images/heart.webp"),
      },
    };
  },
  methods: {
    onSubmit(event) {
      event.preventDefault();
      alert(JSON.stringify(this.form));
    },
    onReset(event) {
      event.preventDefault();
      // Reset our form values
      this.form.email = "";
      this.form.name = "";
      this.form.food = null;
      this.form.checked = [];
      // Trick to reset/clear native browser form validation state
      this.show = false;
      this.$nextTick(() => {
        this.show = true;
      });
    },
  },
};
</script>

<style scoped>
.grey {
  color: rgb(121, 127, 139);
}
.px-15 {
  padding-left: 15px;
  padding-right: 15px;
}
.py-20 {
  padding-top: 15px;
  padding-bottom: 15px;
}
.h-full {
  height: 100vh;
}
.center {
  justify-content: center;
  align-items: center;
}
.heart-img {
  width: 100px;
}
.title {
  display: block;
  font-size: 1.5rem;
}
</style>
