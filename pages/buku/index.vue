<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-lg-12">
        <h2 class="text-center my-4">BUKU</h2>
        <div class="my-3">
          <form @submit.prevent="getBooks">
            <input v-model="keyword" type="search" class="form-control form-control-lg rounded-5" placeholder="Mau baca apa hari ini?">
          </form>
        </div>
        <h1 v-if="loading" class="text-center my-5">TUNGGU SEBENTAR....</h1>
        <div class="my-3 text-muted">menampilkan 3 dari 3</div>
        <div class="row">
          <div v-for="(book,i) in books" :key="i" class="col-lg-2">
            <NuxtLink :to="`/buku/${book.id}`">
              <div class="card mb-3">
                <div class="card-body">
                  <img :src="book.cover" class="cover" alt="cover">
                </div>
              </div>
            </NuxtLink>
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
const loading = ref(true)

const getBooks = async () => {
  const { data, error } = await supabase.from('buku').select(`*, kategori(*)`).ilike('judul', `%${keyword.value}%`)
  if(data) {
    books.value = data
    loading.value = false
  }
}

onMounted(() => {
  getBooks()
})
</script>

<style scoped>
.card-body {
  width: 100%;
  height: 20em;
  padding: 0;
}
.cover {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: 0 30;
}
</style>