<template>
  <div>
    <p class="md-title">Your shortened links</p>
    <p v-if="!this.stats.length">
      I can see... a big sea of nothing!
      Try creating some shortlinks over
      <router-link to="/admin">here!</router-link>
    </p>
    <div v-for="link in links" v-bind:key="link">
      <p>
        Long URL:
        <a :href="link.longURL">{{link.longURL}}</a>
      </p>
      <p>
        Short URL:
        <a :href="link.shortURL">{{link.shortURL}}</a>
      </p>
      <p>Statistics: {{link.stats}}</p>
      <hr />
    </div>
    <router-link to="/admin">Go back!</router-link>
  </div>
</template>

<script>
import { api } from "../../config.json";

export default {
  name: "links",
  data() {
    return {
      stats: [],
    };
  },
  beforeMount() {
    if (!this.$cookies.isKey("token")) return this.$router.replace("/login");

    fetch(api + "/me/links", {
      headers: {
        Authorization: `Bearer ${this.$cookies.get("token")}`,
      },
    })
      .then((res) => res.json())
      .then((res) => {
        this.stats = res.data;
      });
  },
};
</script>