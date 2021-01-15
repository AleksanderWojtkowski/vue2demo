<template>
  <v-container>
    <v-card
      style="padding:30px;margin:50px"
      max-width="800"
      class="mx-auto"
      color="blue"
      dark
    >
      <v-card-title>
        <v-img
          src="../assets/profile.jpg"
          max-height="50"
          max-width="50"
          class="imgWhite"
        ></v-img>
        <span class="title font-weight-light">{{ user.username }}</span>
      </v-card-title>
      <form @submit="addPost($event)">
        <v-textarea
          clearable
          clear-icon="mdi-close-circle"
          placeholder="Share your story:"
          v-model="newPost"
        />
        <p style="color:#F44336" v-if="validation">{{ validation }}</p>
        <v-btn type="submit" color="green">Share</v-btn>
      </form>
    </v-card>
  </v-container>
</template>

<script lang="ts">
interface User {
  name: string;
  username: string;
}
import { Component, Vue, Prop } from "vue-property-decorator";

@Component
export default class AddPost extends Vue {
  @Prop() user!: User;
  newPost = "";
  validation = "";

  addPost(e: any): void {
    e.preventDefault();

    if (this.newPost === "" || this.newPost.length > 150) {
      this.validation =
        "Story cannot be empty and max length of story is 150 characters";
      console.log(this.validation);
      return;
    }
    this.validation = "";
    this.$emit("addPost", this.newPost);
    this.newPost = "";
  }
}
</script>
<style>
.imgWhite {
  border-radius: 50%;
  border: 2px solid white;
  background-color: white;
  margin: 10px 5px;
}
</style>
