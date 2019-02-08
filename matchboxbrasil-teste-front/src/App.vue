<template>
  <div id="app">
    <!-- Container -->
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
      <!-- end header -->

      <!-- Ranking List -->
      <div class="row justify-content-md-center">
        <section class="mt-3 col-4">
          <b-list-group class="col-12">
            <b-list-group-item v-b-popover.hover="item.positive" title="GOSTAM" id="popoverButton-sync" class="text-monospace" v-for="item in arrRankingList" v-bind:key="item.name" button><b-img left rounded="circle" width="75" class="mr-4" :src="item.picture" alt="left image"/><b-badge variant="primary">{{item.rank}}</b-badge>{{item.name}}<p id="p">{{item.description}}</p></b-list-group-item>
          </b-list-group>
        </section>
      </div>
      <!-- end ranking list -->

    </div>
    <!-- end container -->
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'app',
  data () {
    return {
      arrRankingList: [],
    }
  },
  mounted () {
    // GET para consumir o json
    axios
      .get('./src/matchboxbrasil.json')
      .then(response => {
        let arrayRetorno = response.data.data
        // forEach para calcular porcentagem
        arrayRetorno.forEach(pessoa => {
            let total = (pessoa.positive || 0) + (pessoa.negative || 0)
            // this.rankingNumber = rankingNumber
            pessoa.positive = ((pessoa.positive * 100) / total).toFixed(2) + "%"
            pessoa.negative = ((pessoa.negative * 100) / total).toFixed(2) + "%"

          })
        // sort pela porcentagem
        arrayRetorno.sort((a, b) => {
          if (a.positive > b.positive)
            return 0;
          if (a.positive < b.positive)
            return 1;
            return -1;
        });
        arrayRetorno.forEach((pessoa, indice) => {
            pessoa.rank = indice+1;
        })
        this.arrRankingList = arrayRetorno // encapsular o array de objetos do arquivo json no array criado em data()

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
// importação de uma fotne externa
@import url('https://fonts.googleapis.com/css?family=Montserrat');
// importação do styles para a SPA
@import './assets/styles/variables';
@import './assets/styles/bootstrap';

/*Sobrescrever algumas propriedades do bootstrap vue*/
#card {
  width: 90% !important;
  margin-left: 15px !important;
}
/*Id para determinar a font-size da tag p*/
#p {
  font-size: 12px !important;
}
/*Sobrescrever o componente badge do bootstrap vue*/
.badge {
  margin-right: 10px !important;
  border-radius: 0.55rem;
}
</style>
