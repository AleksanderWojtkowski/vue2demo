<template>
  <div>
    <AddPost @addPost="addPost" :user="user" />
    <v-card max-width="1200" class="mx-auto">
      <v-container>
        <v-row dense>
          <v-card class="mx-auto titleContainer">
            <h1 style="text-align:center">Daily Stories</h1></v-card
          >

          <v-col v-for="(post, i) in properPosts" :key="i" cols="12">
            <v-card
              style="margin-top:50px"
              class="mx-auto "
              color="blue"
              dark
              max-width="800"
            >
              <v-card-title>
                <v-icon large left>
                  mdi-access-point
                </v-icon>
                <span class="title font-weight-light">Story of the day</span>
              </v-card-title>

              <v-card-text class="headline font-weight-bold">
                {{ post.text }}
              </v-card-text>

              <v-card-actions>
                <v-list-item class="grow">
                  <v-list-item-avatar
                    v-if="post.user === user.username"
                    color="grey darken-3"
                  >
                    <v-img
                      class="elevation-6"
                      alt=""
                      src="../assets/profile.jpg"
                    ></v-img>
                  </v-list-item-avatar>
                  <v-list-item-avatar v-else color="grey darken-3">
                    <v-img
                      class="elevation-6"
                      alt=""
                      src="https://avataaars.io/?avatarStyle=Transparent&topType=ShortHairShortCurly&accessoriesType=Prescription02&hairColor=Black&facialHairType=Blank&clotheType=Hoodie&clotheColor=White&eyeType=Default&eyebrowType=DefaultNatural&mouthType=Default&skinColor=Light"
                    ></v-img>
                  </v-list-item-avatar>

                  <v-list-item-content>
                    <v-list-item-title>{{ post.user }}</v-list-item-title>
                  </v-list-item-content>

                  <v-row align="center" justify="end">
                    <div v-if="post.liked">
                      <v-icon
                        @click="likePost(post.id)"
                        class="mr-1"
                        color="red"
                        >mdi-heart</v-icon
                      >
                    </div>
                    <div v-else>
                      <v-icon @click="likePost(post.id)" class="mr-1">
                        mdi-heart
                      </v-icon>
                    </div>
                    <v-icon
                      color="black"
                      @click="deletePost(post.id)"
                      class="ml-2"
                    >
                      mdi-cancel
                    </v-icon>
                  </v-row>
                </v-list-item>
              </v-card-actions>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-card>
  </div>
</template>

<script lang="ts">
interface User {
  name: string;
  username: string;
}

import { Component, Vue, Prop, Watch } from "vue-property-decorator";
import AddPost from "../components/AddPost.vue";

@Component({
  components: {
    AddPost,
  },
})
export default class Home extends Vue {
  @Prop() posts!: any;
  @Prop() user!: User;

  properPosts =
    this.posts &&
    this.posts.map((post: any) => ({
      id: post.id,
      text: post.body,
      user: post.title.substr(0, 10),
      liked: false,
    }));
  //Methods
  likePost(id: number): void {
    const post = this.properPosts.find((post: any) => post.id === id);
    post.liked = !post.liked;
  }
  deletePost(id: number): void {
    this.$emit("deletePost", id);
  }
  addPost(newPost: string) {
    this.$emit("addPost", newPost);
  }
  //prop change
  @Watch("posts")
  check() {
    this.properPosts =
      this.posts &&
      this.posts.map((post: any) => ({
        id: post.id,
        text: post.body,
        user: post.title.substr(0, 18),
        liked: false,
      }));
  }
}
</script>
<style>
.titleContainer {
  border: none !important;
  box-shadow: none !important;
  margin: 15px;
}
</style>
