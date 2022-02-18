<template>
  <div class="container">
    <div class="row justify-content-center">
      <center class="mb-3">
        <h1>Aplikasi Pencatatan Titip Tabung</h1>
        <h2>Pangkalan Tabung Iffah Store</h2>
      </center>

      <div class="col-md-4">
        <form @submit.prevent="addTitip" class="bg-dark p-3 text-white rounded">
          <div class="form-group">
            <label for="nama" class="form-label">Nama</label>
            <input
              type="text"
              class="form-control"
              id="nama"
              v-model="nama"
              required
            />
          </div>
          <div class="form-group">
            <label for="hari" class="form-label">Tanggal</label>
            <input
              type="date"
              class="form-control"
              id="hari"
              v-model="hari"
              required
            />
          </div>
          <div class="form-group">
            <label for="j_tabung" class="form-label">Jumlah Tabung</label>
            <input
              type="number"
              class="form-control"
              id="j_tabung"
              v-model="j_tabung"
              required
            />
          </div>
          <div class="form-group">
            <label for="rupiah" class="form-label">Jumlah Uang</label>
            <input
              type="number"
              class="form-control"
              id="rupiah"
              v-model="j_uang"
              required
            />
          </div>
          <div class="form-group">
            <label for="l_tabung" class="form-label">Lunas Tabung</label>
            <select
              class="form-select"
              id="l_tabung"
              aria-label="Floating label select example"
              v-model="l_tabung"
              required
            >
              <option value="ya">ya</option>
              <option value="tidak">Tidak</option>
            </select>
          </div>
          <div class="form-group">
            <label for="l_uang" class="form-label">Lunas Uang</label>
            <select
              class="form-select"
              id="l_uang"
              aria-label="Floating label select example"
              v-model="l_uang"
              required
            >
              <option value="ya">ya</option>
              <option value="tidak">Tidak</option>
            </select>
          </div>
          <div v-if="edit == false">
            <button class="btn btn-primary mt-3" v-on:keyup.enter="addTitip">
              Submit
            </button>
          </div>
          <div v-if="edit == true">
            <button class="btn btn-success mt-3" @click="updateTitip()">
              Update
            </button>
            <button class="btn btn-danger mt-3 ms-4" @click="cancelTitip()">
              Cancel
            </button>
          </div>
        </form>
      </div>
      <div class="col-md-8">
        <div class="table-responsive">
          <table
            class="table table-striped table-light table-hover table-danger"
          >
            <thead>
              <tr>
                <th>No</th>
                <th>Nama</th>
                <th>Tanggal</th>
                <th>Jumlah Tabung</th>
                <th>Jumlah Uang</th>
                <th>Lunas Tabung</th>
                <th>Lunas Uang</th>
                <th>Aksi</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(row, index) in titips" :key="index">
                <td>{{ index + 1 }}</td>
                <td>{{ row.nama }}</td>
                <td>{{ row.hari }}</td>
                <td>{{ row.j_tabung }}</td>
                <td>{{ row.j_uang }}</td>
                <td>{{ row.l_tabung }}</td>
                <td>{{ row.l_uang }}</td>
                <td>
                  <button
                    class="badge bg-warning"
                    @click="editTitip(row, index)"
                  >
                    Edit
                  </button>
                  <button
                    class="badge bg-danger ms-2"
                    @click="deleteTitip(index)"
                  >
                    delete
                  </button>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { onMounted, ref } from "vue";

const nama = ref("");
const hari = ref("");
const j_tabung = ref();
const j_uang = ref();
const l_tabung = ref("");
const l_uang = ref("");
const titips = ref([]);
const number = ref(1);
const edit = ref(false);
const ind = ref(0);
function addTitip() {
  const newTitip = {
    id: Date.now,
    nama: nama.value,
    hari: hari.value,
    j_tabung: j_tabung.value,
    j_uang: j_uang.value,
    l_tabung: l_tabung.value,
    l_uang: l_uang.value,
  };
  titips.value.push(newTitip);
  if (nama.value !== "") {
    localStorage.titips = JSON.stringify(titips.value);
    nama.value = "";
    hari.value = "";
    j_tabung.value = "";
    j_uang.value = "";
    l_tabung.value = "";
    l_uang.value = "";
  }
}
onMounted(() => {
  const titipData = localStorage.titips;
  if (titipData) {
    titips.value = JSON.parse(titipData);
  }
});
function editTitip(t, i) {
  edit.value = !edit.value;
  nama.value = t.nama;
  hari.value = t.hari;
  j_tabung.value = t.j_tabung;
  j_uang.value = t.j_uang;
  l_tabung.value = t.l_tabung;
  l_uang.value = t.l_uang;
  ind.value = i;
}
function updateTitip() {
  edit.value = !edit.value;
  const titipdb = {
    id: Date.now,
    nama: nama.value,
    hari: hari.value,
    j_tabung: j_tabung.value,
    j_uang: j_uang.value,
    l_tabung: l_tabung.value,
    l_uang: l_uang.value,
  };
  titips.value[ind.value] = titipdb;
  localStorage.titips = JSON.stringify(titips.value);
  let titipData1 = localStorage.titips;
  let titipDB = JSON.parse(titipData1);
  titips.value = titipDB;
  nama.value = "";
  hari.value = "";
  j_tabung.value = "";
  j_uang.value = "";
  l_tabung.value = "";
  l_uang.value = "";
}
function cancelTitip() {
  edit.value = !edit.value;
  nama.value = "";
  hari.value = "";
  j_tabung.value = "";
  j_uang.value = "";
  l_tabung.value = "";
  l_uang.value = "";
}
function deleteTitip(index) {
  titips.value.splice(index, 1);
}
</script>

<style>
</style>