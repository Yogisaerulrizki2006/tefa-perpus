<template>
  <div>
    <div class="container-fluid">
      <div class="row">
        <div class="col-lg-2">
          <nuxt-link to="../">
            <i class="bi bi-arrow-left-short"></i>
          </nuxt-link>
        </div>
        <div class="col-lg-12">
          <div class="mt-1">
            <form @submit.prevent="getBooks">
              <input v-model="keyword" type="search" class="form-control " placeholder="Mau baca apa hari ini?" />
            </form>
          </div>
        </div>
        <div class="my-3 text-muted">menampilkan {{ books.length }} dari {{ book }}</div>
      </div>
      <div class="row">
        <div v-for="(book, i) in books" :key="i" class="col-2">
          <div class="card mb-3">
            <div class="card-body">
              <nuxt-link :to="`buku/${book.id}`">
                <img :src="book.cover" class="cover" alt="cover" />
              </nuxt-link>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
  const supabase = useSupabaseClient()
  const books = ref([])
  const keyword = ref('')
  const book=ref(0)

  const getBooks = async () => {
    const { data, error } = await supabase.from('buku').select(`*`)
      .ilike('judul', `%${keyword.value}%`)
      if(data) books.value = data
  }
  const hitungData = async() => {
    const { data, count } = await supabase.from("buku").select("*", { count : 'exact'})
    if (data) book.value = count
  
  }
  onMounted(() => {
    hitungData()
    getBooks()
  })

</script>

<style scoped>
.card-body {
  width: 100%;
  height: 19em;
  padding: 0;
}

.cover {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: 0 30;
}

i {
  font-size: 60px;
  color: #000000
}

input {
  background-color: rgba(235, 235, 235, 0.699);
}
</style>
