<template>
  <div>
    <div class="container" style="display: flex; flex-wrap: wrap">
      <v-card
        v-for="user in users.data"
        :key="user.name"
        style="border: 1px gray solid; margin: 10px; height; 100px; width: 275px"
      >
        <v-card-title primary-title>
          {{ user.name }}
        </v-card-title>
        <v-card-text>
          {{ 'Username: ' + user.username }}
        </v-card-text>
        <div style="margin: 10px">
          <v-card-text style="padding: 0px 5px"
            ><strong> Address </strong></v-card-text
          >
          <v-card-text
            v-for="(item, value, i) in user.address"
            :key="i"
            style="padding: 0px 10px"
          >
            <p
              v-if="!item.lat && !value.match(/__/)"
              style="padding: 0px; margin: 0px"
            >
              {{ item }}
            </p>
            <div v-else-if="item.lat" style="margin: 10px">
              <p style="padding: 0px; margin: 0px">
                {{ 'Latitude: ' + item.lat }}
              </p>
              <p style="padding: 0px; margin: 0px">
                {{ 'Longitude: ' + item.lng }}
              </p>
            </div>
          </v-card-text>
        </div>
        <v-card-text>
          <!-- Note that posts { meta { totalCount } } return null -->
          {{ 'Number of posts: ' + user.posts.data.length }}
        </v-card-text>
      </v-card>
    </div>
  </div>
</template>

<script>
/* eslint-disable no-console */
import usersQuery from '~/apollo/queries/users'

export default {
  name: 'Users',
  data: () => ({
    users: [],
  }),
  apollo: {
    users: {
      prefetch: true,
      query: usersQuery,
      variables(page, limit) {
        return {
          options: {
            paginate: {
              page,
              limit,
            },
          },
        }
      },
    },
  },
}
</script>
