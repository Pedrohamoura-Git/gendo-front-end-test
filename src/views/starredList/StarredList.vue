<template>
  <div class="starredList">
    <FilterName />
    <ul class="wrapper">
      <li v-for="repo in searchStarred" :key="repo.id">
        <div class="starredList__item" v-if="repo.stargazers_count">
          <a :href="repo.html_url" target="_blank">{{ repo.name }}</a>
          <p class="starredList__item__description">{{ repo.description }}</p>
          <section class="starredList__item__git-info">
            <span class="starredList__item__git-info__starredCont">
              <img
                src="data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIyNCIgaGVpZ2h0PSIyNCIgdmlld0JveD0iMCAwIDI0IDI0Ij48cGF0aCBkPSJNMTIgLjI4OGwyLjgzMyA4LjcxOGg5LjE2N2wtNy40MTcgNS4zODkgMi44MzMgOC43MTgtNy40MTYtNS4zODgtNy40MTcgNS4zODggMi44MzMtOC43MTgtNy40MTYtNS4zODloOS4xNjd6Ii8+PC9zdmc+"
              />
              <p>{{ repo.stargazers_count }}</p>
            </span>
            <span class="starredList__item__git-info__fork">
              <img
                src="data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIyNCIgaGVpZ2h0PSIyNCIgdmlld0JveD0iMCAwIDI0IDI0Ij48cGF0aCBkPSJNMjEgNGgtMnYyaC0ydi0yaC0ydi0yaDJ2LTJoMnYyaDJ2MnptLTQuMTMgNGMtLjU3NyAyLjMxNy0yLjQ0NSAyLjcyMy00LjgxNyAzLjIyMy0xLjcxLjM2LTMuNjQzLjc3NS01LjA1MyAyLjA4NXYtNy40OTJjMS4xNjItLjQxMyAyLTEuNTExIDItMi44MTYgMC0xLjY1Ny0xLjM0My0zLTMtM3MtMyAxLjM0My0zIDNjMCAxLjMwNS44MzggMi40MDMgMiAyLjgxNnYxMi4zNjdjLTEuMTYyLjQxNC0yIDEuNTEyLTIgMi44MTcgMCAxLjY1NyAxLjM0MyAzIDMgM3MzLTEuMzQzIDMtM2MwLTEuMjk1LS44MjQtMi4zODgtMS45NzMtMi44MDguMjctMy45MjIgMi41Ny00LjQwOCA1LjQzOC01LjAxMiAyLjYxMS0uNTUgNS43MzMtMS4yMyA2LjQzNS01LjE4aC0yLjAzem0tMTIuNjctNWMwLS45OTMuODA4LTEuOCAxLjgtMS44czEuOC44MDcgMS44IDEuOC0uODA4IDEuOC0xLjggMS44LTEuOC0uODA3LTEuOC0xLjh6bTMuNiAxOGMwIC45OTMtLjgwOCAxLjgtMS44IDEuOHMtMS44LS44MDctMS44LTEuOC44MDgtMS44IDEuOC0xLjggMS44LjgwNyAxLjggMS44eiIvPjwvc3ZnPg=="
              />
              <p>{{ repo.forks_count }}</p>
            </span>
          </section>
        </div>
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
    this.updateStarredCont()
  },
  computed: {
    userStarred() {
      return this.$store.state.userStarred
    },
    filterText() {
      return this.$store.state.filterText
    },
    searchStarred: function () {
      return this.userStarred.filter((repo) => {
        return repo.name.toLowerCase().match(this.filterText) || repo.owner.login.toLowerCase().match(this.filterText)
      })
    },
  },
  watch: {
    searchStarred: function () {
      if (this.searchStarred !== '') {
        this.updateStarredCont(this.searchStarred.length)
      } else {
        this.updateStarredCont(this.userStarred.length)
      }
    },
  },
  methods: {
    updateStarredCont() {
      let payload
      if (this.searchStarred !== '') {
        payload = this.searchStarred.length
      } else {
        payload = this.userStarred.length
      }
      this.$store.commit('mutateStarredCont', payload)
    },
  },
}
</script>

<style lang="scss" src="./scss/_starredList.scss" />
