<template>
  <div class="reposList">
    <FilterName />
    <ul class="wrapper">
      <li v-for="repo in searchRepos" :key="repo.id" class="reposList__item">
        <a :href="repo.html_url" target="_blank">{{ repo.name }}</a>
        <p class="reposList__item__description">{{ repo.description }}</p>

        <section class="reposList__item__git-info">
          <span class="reposList__item__git-info__lang">
            <img
              src="data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIyNCIgaGVpZ2h0PSIyNCIgdmlld0JveD0iMCAwIDI0IDI0Ij48cGF0aCBkPSJNMjQgMTAuOTM1djIuMTMxbC0xMCA0LjkzNHYtMi4yM2w3LjY0LTMuNzctNy42NC0zLjc3OXYtMi4yMjFsMTAgNC45MzV6bS0yNCAwdjIuMTMxbDEwIDQuOTM0di0yLjIzbC03LjY0LTMuNzcgNy42NC0zLjc3OXYtMi4yMjFsLTEwIDQuOTM1eiIvPjwvc3ZnPg=="
            />
            <p>{{ repo.language }}</p>
          </span>

          <span class="reposList__item__git-info__fork">
            <img
              src="data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIyNCIgaGVpZ2h0PSIyNCIgdmlld0JveD0iMCAwIDI0IDI0Ij48cGF0aCBkPSJNMjEgNGgtMnYyaC0ydi0yaC0ydi0yaDJ2LTJoMnYyaDJ2MnptLTQuMTMgNGMtLjU3NyAyLjMxNy0yLjQ0NSAyLjcyMy00LjgxNyAzLjIyMy0xLjcxLjM2LTMuNjQzLjc3NS01LjA1MyAyLjA4NXYtNy40OTJjMS4xNjItLjQxMyAyLTEuNTExIDItMi44MTYgMC0xLjY1Ny0xLjM0My0zLTMtM3MtMyAxLjM0My0zIDNjMCAxLjMwNS44MzggMi40MDMgMiAyLjgxNnYxMi4zNjdjLTEuMTYyLjQxNC0yIDEuNTEyLTIgMi44MTcgMCAxLjY1NyAxLjM0MyAzIDMgM3MzLTEuMzQzIDMtM2MwLTEuMjk1LS44MjQtMi4zODgtMS45NzMtMi44MDguMjctMy45MjIgMi41Ny00LjQwOCA1LjQzOC01LjAxMiAyLjYxMS0uNTUgNS43MzMtMS4yMyA2LjQzNS01LjE4aC0yLjAzem0tMTIuNjctNWMwLS45OTMuODA4LTEuOCAxLjgtMS44czEuOC44MDcgMS44IDEuOC0uODA4IDEuOC0xLjggMS44LTEuOC0uODA3LTEuOC0xLjh6bTMuNiAxOGMwIC45OTMtLjgwOCAxLjgtMS44IDEuOHMtMS44LS44MDctMS44LTEuOC44MDgtMS44IDEuOC0xLjggMS44LjgwNyAxLjggMS44eiIvPjwvc3ZnPg=="
            />
            <p>{{ repo.forks_count }}</p>
          </span>
        </section>
      </li>
    </ul>
  </div>
</template>

<script>
import FilterName from '../../components/widgets/filterName/FilterName.vue'

export default {
  components: { FilterName },
  data() {
    return {}
  },
  created() {
    this.updateReposCont()
  },
  computed: {
    userRepos() {
      return this.$store.state.userRepos
    },
    filterText() {
      return this.$store.state.filterText
    },
    searchRepos: function () {
      return this.userRepos.filter((repo) => {
        return repo.name.toLowerCase().match(this.filterText) || repo.owner.login.toLowerCase().match(this.filterText)
      })
    },
  },
  watch: {
    searchRepos: function () {
      this.updateReposCont()
    },
  },
  methods: {
    updateReposCont() {
      let payload
      if (this.searchRepos !== '') {
        payload = this.searchRepos.length
      } else {
        payload = this.userRepos.length
      }
      this.$store.commit('mutateReposCont', payload)
    },
  },
}
</script>

<style lang="scss" src="./scss/_reposList.scss" />
