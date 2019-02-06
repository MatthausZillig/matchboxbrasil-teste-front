<template>
  <div id="app">
    <div class="container p-5">
      <!--Header-->
      <div class="row justify-content-md-center">
        <header class="col-4 mt-5">
          <b-img id="logo" class="mb-3 ml-3" src= "src/assets/logo_matchbox.png" fluid alt="Fluid image"/>
          <div id="card" class="card">
            <div class="card-body">
              <h5 class="text-body text-center text-monospace font-weight-bolder align-middle">ranking</h5>
          </div>
          </div>
        </header>
      </div>

      <!-- Ranking List -->
      <div class="row justify-content-md-center">
        <section class="mt-3 col-4">
          <b-list-group class="col-12">
            <b-list-group-item v-for="item in arrRankingList" v-bind:key="item" button><b-img left rounded="circle" width="75" class="mr-4" :src="item.picture" alt="left image"/>{{item.name}}<p>{{item.description}}</p></b-list-group-item>
          </b-list-group>
        </section>
      </div>

    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'app',
  data () {
    return {
      arrRankingList: []

    }
  },
  mounted () {
    axios
      .get('./src/matchboxbrasil.json')
      .then(response => {
        this.arrRankingList = response.data.data
        console.log(this.arrRankingList)
      })
      .catch(error => {
        console.log(error)
        this.errored = true
      })
      .finally(() => this.loading = false)
  }
}
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css?family=Montserrat');
@import './assets/styles/variables';
@import './assets/styles/bootstrap';
#card {
  width: 90% !important;
  margin-left: 15px !important;
}
#logo {

}
</style>
