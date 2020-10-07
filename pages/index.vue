<template>
  <div>
    <div class="container" style="display: flex; flex-wrap: wrap">
      <v-card
        v-for="post in posts.data"
        :key="post.id"
        style="
          border: 1px white solid;
          margin: 10px;
          height: 450px;
          width: 30%;
          display: flex;
          flex-direction: column;
        "
      >
        <div style="display: flex; background-color: black">
          <v-card-text>
            {{ post.user.name }}
          </v-card-text>
          <v-card-text>
            {{ post.user.email }}
          </v-card-text>
          <v-card-text>
            {{ post.user.website }}
          </v-card-text>
          <v-card-text>
            {{ post.user.company.name }}
          </v-card-text>
        </div>
        <v-card-title primary-title>
          {{ post.title }}
        </v-card-title>
        <v-card-text>
          {{ post.body }}
        </v-card-text>
        <v-card-text
          style="border-top: 1px gray solid; background-color: black"
        >
          Comments...
        </v-card-text>
        <div style="flex: 1; overflow-y: auto; padding: 10px">
          <v-card-text
            v-for="(item, value, i) in post.comments.data"
            :key="i"
            style="padding: 0px 10px; margin: 5px 0px; border: 1px gray solid"
          >
            <p style="padding: 5px 0px; margin: 0px">
              <strong>{{ item.name }}</strong>
            </p>
            <p style="padding: 10px 0px; margin: 0px">
              {{ item.body }}
            </p>
            <p style="padding: 0px; margin: 0px">
              {{ item.email }}
            </p>
          </v-card-text>
        </div>
      </v-card>
    </div>
    <div style="display: flex; justify-content: center" width="100%">
      <v-btn style="margin: 10px" color="success" @click="prev">Forrige</v-btn>
      <p style="align-self: center; padding-top: 20px">{{ 'Side: ' + page }}</p>
      <v-btn style="margin: 10px" color="success" @click="next">Næste</v-btn>
    </div>
  </div>
</template>

<script>
/* eslint-disable no-console */
import postsQuery from '~/apollo/queries/posts'

export default {
  name: 'Home',
  data: () => ({
    posts: [],
    page: 1,
    limit: 9,
  }),
  computed: {},
  mounted() {
    this.$apollo
      .query({
        query: postsQuery,
        variables: {
          options: {
            paginate: {
              page: this.page,
              limit: this.limit,
            },
          },
        },
      })
      .then(({ data }) => {
        console.log('data', data)
        this.posts = data.posts
      })
  },
  methods: {
    prev() {
      if (this.page > 1) {
        this.page -= 1
        this.$apollo
          .query({
            query: postsQuery,
            variables: {
              options: {
                paginate: {
                  page: this.page,
                  limit: this.limit,
                },
              },
            },
          })
          .then(({ data }) => {
            console.log('data', data)
            this.posts = data.posts
          })
      }
    },
    next() {
      this.page += 1
      this.$apollo
        .query({
          query: postsQuery,
          variables: {
            options: {
              paginate: {
                page: this.page,
                limit: this.limit,
              },
            },
          },
        })
        .then(({ data }) => {
          console.log('data', data)
          this.posts = data.posts
        })
    },
  },
}
</script>
