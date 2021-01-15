<template>
  <v-app id="inspire">
    <v-navigation-drawer v-model="drawer" app>
      <v-list-item>
        <v-list-item-content>
          <v-list-item-title style="color:#2196F3;" class="title">
            Welcome {{ user.name }}
          </v-list-item-title>
          <v-list-item-subtitle>
            {{ user.username }}
          </v-list-item-subtitle>
          <v-img
            src="./assets/profile.jpg"
            max-height="50"
            max-width="50"
            class="img"
          ></v-img>
        </v-list-item-content>
      </v-list-item>

      <v-divider></v-divider>

      <v-list dense nav>
        <v-list-item v-for="item in items" :key="item.title" link>
          <v-list-item-icon>
            <v-icon color="blue">{{ item.icon }}</v-icon>
          </v-list-item-icon>

          <v-list-item-content @click="setCurrentPage(item.title)">
            <v-list-item-title style="color:#2196F3">{{
              item.title
            }}</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-app-bar style="background:white" app>
      <v-app-bar-nav-icon
        color="blue"
        @click="drawer = !drawer"
      ></v-app-bar-nav-icon>

      <v-toolbar-title style="color:#2196F3;margin-left:20px"
        >StoryTeller
      </v-toolbar-title>
      <v-icon style="margin-left:10px" medium color="blue">
        mdi-message-text
      </v-icon>
    </v-app-bar>

    <v-main>
      <div v-if="currentPage === 'Home' && dataLoaded && posts">
        <Home
          @addPost="addPost"
          @deletePost="deletePost"
          :user="user"
          :posts="posts"
        />
      </div>
      <div v-else-if="currentPage === 'Settings'">
        <Settings @updateProfile="updateProfile" :user="user" />
      </div>
      <div v-else class="text-center">
        <v-progress-circular
          :size="50"
          color="primary"
          indeterminate
        ></v-progress-circular>
      </div>
    </v-main>
  </v-app>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import Home from "./views/Home.vue";
import Settings from "./views/Settings.vue";

import axios from "axios";

@Component({
  components: {
    Home,
    Settings,
  },
})
export default class App extends Vue {
  drawer = null;
  currentPage = "Home";
  items = [
    { title: "Home", icon: "mdi-antenna" },
    { title: "Settings", icon: "mdi-pencil" },
  ];
  right = null;
  user = {
    username: "Alek_Wojtkowski",
    name: "Alek",
  };
  posts: object[] = [];
  dataLoaded = false;

  async getPosts(): Promise<void> {
    try {
      const res = await axios.get("https://jsonplaceholder.typicode.com/posts");
      if (res) {
        this.dataLoaded = true;
        this.posts = res.data;
      }
    } catch (error) {
      console.log(error);
    }
  }
  async addPost(newPost: string): Promise<void> {
    try {
      const res = await axios.post(
        `https://jsonplaceholder.typicode.com/posts`,
        {
          body: newPost,
          title: this.user.username,
        }
      );
      if (res) {
        this.posts = [res.data, ...this.posts];
      }
    } catch (error) {
      console.log(error);
    }
  }
  async deletePost(id: number): Promise<void> {
    try {
      const res = await axios.delete(
        `https://jsonplaceholder.typicode.com/posts/${id}`
      );
      if (res) {
        this.posts = this.posts.filter((post: any) => post.id !== id);
      }
    } catch (error) {
      console.log(error);
    }
  }
  updateProfile(newProfile: any): void {
    console.log(newProfile);
    this.user.name = newProfile.newName;
    this.user.username = newProfile.newUsername;
  }
  setCurrentPage(page: string) {
    this.currentPage = page;
    this.drawer = null;
  }
  mounted() {
    this.getPosts();
  }
}
</script>

<style lang="scss">
.img {
  border-radius: 50%;
  border: 2px solid #2196f3;
  background-color: white;
  margin: 10px 0;
}

.v-progress-circular {
  margin: 1rem;
}
</style>
