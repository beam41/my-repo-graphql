<template>
  <div v-if="data" class="main-col columns is-multiline">
    <div class="column is-full">
      <h1 class="title is-1 has-text-centered">Meehoi Repo</h1>
    </div>
    <div
      v-for="edge in data.user.repositories.edges"
      :key="edge.node.id"
      class="column is-one-quarter"
    >
      <div class="card">
        <div class="card-content">
          <div>
            <div class="media">
              <div class="media-content">
                <h5 class="title is-6">
                  <a :href="edge.node.url">{{ edge.node.nameWithOwner }}</a>
                </h5>
              </div>
            </div>
            <div class="content desc">
              <p>{{ edge.node.description || 'No description' }}</p>
            </div>
          </div>
          <div class="content time">
            <p class="is-size-7">
              Created: <timeago :datetime="edge.node.createdAt"></timeago>
            </p>
            <p class="is-size-7">
              Updated:
              <timeago :datetime="edge.node.updatedAt"></timeago>
            </p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import gql from 'graphql-tag'
export default {
  data: () => ({
    data: null,
  }),
  mounted() {
    const query = gql`
      query($login: String!, $length: Int!) {
        user(login: $login) {
          repositories(first: $length) {
            edges {
              node {
                nameWithOwner
                url
                id
                description
                createdAt
                updatedAt
              }
            }
          }
        }
      }
    `
    const variables = {
      login: 'beam41',
      length: 100,
    }
    this.$apollo.query({ query, variables }).then(({ data }) => {
      this.data = data
    })
  },
}
</script>

<style lang="scss" scoped>
.main-col {
  max-width: 1024px;
  margin: auto;

  .card {
    height: 100%;
    .card-content {
      display: flex;
      flex-direction: column;
      justify-content: space-between;
      height: 100%;
      .content {
        &.desc {
          margin-bottom: 1em;
        }
        &.time {
          p:first-child {
            margin: 0;
          }
        }
      }
    }
  }
}
</style>
