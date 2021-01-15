<template>
  <v-container>
    <v-card
      style="padding:30px;margin:50px"
      max-width="800"
      class="mx-auto"
      color="blue"
      dark
    >
      <v-form @submit="updateProfile($event)">
        <v-text-field v-model="newUser.newName" label="Name"></v-text-field>

        <v-text-field
          v-model="newUser.newUsername"
          label="Username"
        ></v-text-field>
        <p style="color:#F44336" v-if="validator">{{ validator }}</p>
        <v-btn type="submit" color="success" class="mr-4">
          Change
        </v-btn>
      </v-form>
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
export default class Settings extends Vue {
  @Prop() user!: User;
  validator = "";
  newUser = {
    newName: this.user.name,
    newUsername: this.user.username,
  };

  updateProfile(e: any): void {
    e.preventDefault();

    if (
      (!this.newUser.newName && !this.newUser.newUsername) ||
      this.newUser.newName.length > 10 ||
      this.newUser.newUsername.length > 20
    ) {
      this.validator = "Please submit proper name and username";
      return;
    }
    this.$emit("updateProfile", this.newUser);
  }
}
</script>
