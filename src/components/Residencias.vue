<template>
  <div class="row justify-content-center">
    <div class="col-12 col-lg-5">
      <div class="card text-center">
        <div class="card-body">
          <div class="row">
            <div class="col-12 col-lg-8">
              <h3 class="card-title">{{ title }}</h3>
            </div>
            <div class="col">
              <button type="button" class="btn btn-info btn-block" @click="medias">Average Price</button>
            </div>
          </div>
          <ul class="list-group mt-2">
            <template v-for="item in filtrar()">
              <list :key="item.id" :item="item" descricao="Price: $" :valor="item.price">
                <router-link :to="{ name: 'detalhes', params: { item }}">{{ item.name }}</router-link>
              </list>
            </template>
          </ul>
        </div>
      </div>
      <paginacao></paginacao>
    </div>
  </div>
</template>

<script>
import List from "@/components/List";
import Paginacao from "@/components/Paginacao";

export default {
  name: "residencias",
  props: {
    title: String,
  },
  components: {
    list: List,
    paginacao: Paginacao,
  },
  data: () => ({
    inicio: 0,
    fim: 10,
  }),
  computed: {
    residencias() {
      return this.$store.getters.getResidencias;
    },
  },
  created() {
    this.$eventBus.$on("paginacao", obj => this.paginacao(obj));
    this.filtrar();
  },
  mounted() {
    this.$store.dispatch("setResidencias");
  },
  methods: {
    paginacao(obj) {
      let pagina = obj.paginaAtual - 1;
      this.inicio = pagina * obj.limitePaginas;
      this.fim = this.inicio + obj.limitePaginas;
      this.filtrar();
    },
    filtrar() {
      return this.residencias.slice(this.inicio, this.fim);
    },
    medias() {
      this.$router.push({ name: "medias" });
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
a:hover {
  text-decoration: none;
}
li span:first-child {
  cursor: pointer;
}
li a {
  color:#17a2b8;
}
ul {
  height: 500px;
  overflow: auto;
}
</style>
