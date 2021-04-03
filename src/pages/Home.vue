<template>
    <div class="wrapper-content wrapper-content--fixed">
      <section>
        <div class="container">
          <!-- error-->
          <div class="error" v-if="error" style="margin-bottom: 20px;">
            <p> {{ error }} </p>
          </div>

          <search
            :value="search"
            placeholder="Type username..."
            @search="search = $event"
          />

          <!-- button -->
          <button v-if="!repos" class="btn btnPrimary" @click="getRepos">Search!</button>
          <button v-else class="btn btnPrimary" @click="getRepos">Search Again!</button>

          <!-- wrapper -->
          <div class="repos__wrapper" v-if="repos">
            <!-- item -->
            <div class="repos-item" v-for="repo in repos" :key="repo.id">
              <div class="repos-info">
                <a class="link" :href="repo.html_url" target="_blank">{{ repo.name }}</a>
                <span> {{ repo.stargazers_count }} ⭐</span>
              </div>
            </div>
          </div>

        </div>
      </section>
    </div>
</template>

<script>
  import search from '@/components/Search.vue'
  import axios from "axios";

  export default {
    components: { search, axios },
    data () {
      return {
        search: '',
        error: null,
        repos: null
      }
    },
    methods: {
      getRepos () {
        axios
            .get(`https://api.github.com/users/${this.search}/repos`)
            .then( res => {
              this.error = null
              this.repos = res.data
            })
            .catch( error => {
              this.repos = null
              this.error = 'Can`t find this user'
            })
      }
    }
  }
</script>

<style lang="scss" scoped>
  .container {
    display: flex;
    align-items: center;
    flex-direction: column;
  }

  button {
    margin-top: 40px;
  }

  .repos__wrapper {
    width: 400px;
    margin: 30px 0;
  }

  .repos-info {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 10px;
    padding: 10px;
    border-bottom: 1px solid #dbdbdb;
  }
</style>
