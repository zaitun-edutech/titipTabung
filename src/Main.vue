<template>
  <div class="container">
    <div class="row justify-content-center">
      <div class="col-md-6 text-center">
        <div class="p-4 fw-bold">
          <!-- <img src="./assets/top.png" alt="" /> -->
          <h1>Aplikasi Pencatatan Titip Tabung</h1>
          <h2>Pangkalan Tabung Iffah Store</h2>
        </div>
      </div>
    </div>

    <div class="row justify-content-center">
      <div class="col-md-8">
        <div>
          <button class="btn btn-dark mb-3" v-if="post == true" @click="back()">
            &laquo;Back
          </button>
        </div>
        <div v-if="post == false">
          <button class="btn btn-primary mb-3" @click="postData()">
            + Tambah Data
          </button>
        </div>
        <div v-if="post == true">
          <div class="card p-3 mb-4 rounded-3 shadow-lg">
            <form @submit.prevent="addTitip" class="p-3 rounded-3">
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
                  <option selected value="">--pilih--</option>
                  <option value="ya">ya</option>
                  <option value="tidak">Tidak</option>
                </select>
              </div>
              <div class="">
                <label for="l_uang" class="form-label">Lunas Uang</label>
                <select
                  class="form-select"
                  id="l_uang"
                  aria-label="Floating label select example"
                  v-model="l_uang"
                  required
                >
                  <option selected value="">--pilih--</option>
                  <option value="ya">ya</option>
                  <option value="tidak">Tidak</option>
                </select>
              </div>
              <div class="">
                <label for="status" class="form-label"
                  >Status Pengambilan</label
                >
                <select
                  class="form-select"
                  id="status"
                  aria-label="Floating label select example"
                  v-model="status"
                  required
                >
                  <option selected value="">--pilih--</option>
                  <option value="belum">Belum</option>
                  <option value="sudah">Sudah</option>
                </select>
              </div>

              <div v-if="edit == false">
                <button
                  class="btn btn-primary mt-3"
                  v-on:keyup.enter="addTitip"
                >
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
        </div>

        <!-- <div class="col-md-8"> -->
        <div v-if="post == false" class="table-responsive shadow">
          <table id="example" class="table table-hover rounded align-middle">
            <thead class="align-middle">
              <tr class="text-center bg-primary text-white rounded">
                <th>No</th>
                <th>Nama</th>
                <th>Tanggal</th>
                <th>Jumlah Tabung</th>
                <th>Jumlah Uang</th>
                <th>Lunas Tabung</th>
                <th>Lunas Uang</th>
                <th>Status</th>
                <th>Aksi</th>
              </tr>
            </thead>
            <tbody>
              <tr
                v-for="(row, index) in titips"
                :key="index"
                class="fw-bold text-center"
              >
                <td>{{ index + 1 }}</td>
                <td>{{ row.nama }}</td>
                <td>{{ row.hari }}</td>
                <td>{{ row.j_tabung }}</td>
                <td>{{ row.j_uang }}</td>
                <td>{{ row.l_tabung }}</td>
                <td>{{ row.l_uang }}</td>
                <td>{{ row.status }}</td>
                <td class="text-center">
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
              <tr class="text-center">
                <td class="text-center">
                  <h4>Total</h4>
                </td>
                <td></td>
                <td></td>
                <td>
                  <h4>{{ totalTabung }}</h4>
                  <hr />
                </td>
                <td>
                  <h4>{{ totalUang }}</h4>
                  <hr />
                </td>
                <td></td>
                <td></td>
                <td></td>
                <td></td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed, onMounted, ref } from "vue";

const nama = ref("");
const hari = ref("");
const j_tabung = ref();
const j_uang = ref();
const l_tabung = ref("");
const l_uang = ref("");
const status = ref("");
const titips = ref([]);
const number = ref(1);
const edit = ref(false);
const ind = ref(0);
const post = ref(false);
function addTitip() {
  const newTitip = {
    id: Date.now,
    nama: nama.value,
    hari: hari.value,
    j_tabung: j_tabung.value,
    j_uang: j_uang.value,
    l_tabung: l_tabung.value,
    l_uang: l_uang.value,
    status: status.value,
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
    post.value = !post.value;
  }
}
onMounted(() => {
  const titipData = localStorage.titips;
  if (titipData) {
    titips.value = JSON.parse(titipData);
  }
});
function editTitip(t, i) {
  post.value = !post.value;
  edit.value = !edit.value;
  nama.value = t.nama;
  hari.value = t.hari;
  j_tabung.value = t.j_tabung;
  j_uang.value = t.j_uang;
  l_tabung.value = t.l_tabung;
  l_uang.value = t.l_uang;
  status.value = t.status;
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
    status: status.value,
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
  post.value = !post.value;
}
function cancelTitip() {
  edit.value = !edit.value;
  nama.value = "";
  hari.value = "";
  j_tabung.value = "";
  j_uang.value = "";
  l_tabung.value = "";
  l_uang.value = "";
  post.value = !post.value;
}
function deleteTitip(index) {
  if (confirm("Are you sure")) {
    titips.value.splice(index, 1);
    localStorage.titips = JSON.stringify(titips.value);
  }
}
function postData() {
  post.value = !post.value;
}
function back() {
  post.value = !post.value;
}
const totalTabung = computed(() => {
  return titips.value.reduce(
    (total, curr) => (total = total + curr.j_tabung),
    0
  );
});
const totalUang = computed(() => {
  return titips.value.reduce(
    (total, hitung) => (total = total + hitung.j_uang),
    0
  );
});
</script>

<style>
</style>