<template>
  <div class="container-fluid" style="width: 95%">
    <div class="row">
      <div class="col-2">
        <nuxt-link to="../">
          <i class="bi bi-arrow-left-short"></i>
        </nuxt-link>
      </div>
      <div class="col-8">
        <h2 style="
            text-align: center;
            font-family: inter;
            margin-top: 20px;
            text-align: center;
          ">
          ISI FORM PENGUNJUNG
        </h2>
      </div>
      <div class="col-lg-12">
        <form @submit.prevent="kirimData">
          <div class="mb-3">
            <input v-model="form.nama" type="text" class="form-control" placeholder="NAMA........" />
          </div>
          <div class="mb-3">
            <select v-model="form.keanggotaan" class="form-control">
              <option value="">keanggotaan</option>
              <option v-for="(member,i) in members" :key="i" :value="member.id">{{ member.nama }}</option>
            </select>
          </div>
          <div class="mb-3" v-if="form.keanggotaan == 2 ">
            <div class="row">
              <div class="col-md-4">
                <select v-model="form.kelas" class="form-control  mb-2">
                  <option value="">KELAS</option>
                  <option value="X">X</option>
                  <option value="XI">XI</option>
                  <option value="XII">XII</option>
                </select>
              </div>
              <div class="col-md-4">
                <select v-model="form.jurusan" class="form-control  mb-2">
                  <option value="">JURUSAN</option>
                  <option value="PPLG">PPLG</option>
                  <option value="TJKT">TJKT</option>
                  <option value="TSM">TSM</option>
                  <option value="DKV">DKV</option>
                  <option value="TOI">TOI</option>
                </select>
              </div>
              <div class="col-md-4">
                <select v-model="form.no_kelas" class="form-control  mb-2">
                  <option value="">NO KELAS</option>
                  <option value="1">1</option>
                  <option value="2">2</option>
                  <option value="3">3</option>
                  <option value="4">4</option>
                </select>
              </div>
            </div>
          </div>
          <div class="input-group mt-3">
            <select v-model="form.keperluan" class="form-control mb-2">
                  <option value="">Keperluan </option>
                  <option v-for="(item,i) in objectives " :key="i" :value="item.id">{{ item.nama }}</option>
                </select>
          </div>
          <div class="input-group mt-4">
            <button type="submit" class="btn btn-dark">
              KIRIM
            </button>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>


<script setup>
const supabase= useSupabaseClient()
const members = ref([])
const objectives = ref([])

const form = ref({
  nama:"", 
  keanggotaan:"",
  no_kelas:"",
  jurusan:"",
  kelas:"",
  keperluan:"",
})

const kirimData = async() => {
  const { error } = await supabase
  .from('pengunjung')
  .insert([form.value]) 
  console.log([form.value])
  if(!error) navigateTo('/pengunjung')
}
const getKeanggotaan=async () => {
  const { data, error } = await supabase.from('keanggotaan').select('*')
  if(data) members.value = data
}
const getKeperluan = async () => {
  const { data, error } = await supabase.from('keperluan').select('*')
  if(data) objectives.value = data
}
onMounted(() => {
  getKeanggotaan()
  getKeperluan()
}) 
</script>

<style scoped>
i{
  font-size: 60px; 
  color: #000000;
}
input{
  background-color: rgba(235, 235, 235, 0.699);
}
select{
  background-color: rgba(235, 235, 235, 0.699);
}
</style>