
<template>
  <div class="container">
    <div class="row">
      <div class="col-md">
        <!-- <HelloWorld /> -->
        Hallo
      </div>
      <div class="row">
        <div class="col-md">
          <form action="">
            <div class="form-group">
              <label for="nama" class="form-label">Nama</label>
              <input
                type="text"
                class="form-control"
                id="nama"
                v-model="nama"
              />
            </div>
            <div class="form-group">
              <label for="hari" class="form-label">Tanggal</label>
              <input
                type="date"
                class="form-control"
                id="hari"
                v-model="hari"
              />
            </div>
            <div class="form-group">
              <label for="j_tabung" class="form-label">Jumlah Tabung</label>
              <input
                type="number"
                class="form-control"
                id="j_tabung"
                v-model="j_tabung"
              />
            </div>
            <div class="form-group">
              <label for="j_uang" class="form-label">Jumlah Uang</label>
              <input
                type="number"
                class="form-control"
                id="j_uang"
                v-model="j_uang"
              />
            </div>
            <div class="form-group">
              <label for="l_tabung" class="form-label">Lunas Tabung</label>
              <input
                type="text"
                class="form-control"
                id="l_tabung"
                v-model="l_tabung"
              />
            </div>
            <div class="form-group">
              <label for="l_uang" class="form-label">Lunas Uang</label>
              <input
                type="number"
                class="form-control"
                id="l_uang"
                v-model="l_uang"
              />
            </div>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>
<script setup>
import { ref } from "vue";
// import HelloWorld from "./components/HelloWorld.vue";
const nama = ref("");
const Hari = ref("");
const j_tabung = re(0);
const j_uang = ref(0);
const l_tabung = ref("");
const l_uang = ref("");
const titips = ref([]);
const number = ref(1);
const edit = ref(false);
const ind = ref(0);
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
