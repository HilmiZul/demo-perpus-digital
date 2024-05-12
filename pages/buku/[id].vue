<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-md-12">
        <h2 class="text-start my-4">
          <div class="float-start"><NuxtLink to="/" class="btn btn-light me-2 rounded-5">Home</NuxtLink></div>
          <div class="float-start"><NuxtLink to="/buku" class="btn btn-light me-2 rounded-5">Kembali</NuxtLink></div>
					{{ buku.judul }}
        </h2>
        <div class="row mb-5">
          <div class="col-md-3">
            <img :src="buku.cover" class="cover" alt="cover buku">
          </div>
          <div class="col-md-6">
            <div class="badge bg-primary p-2">
						  <span v-if="buku.kategori">{{ buku.kategori.nama }}</span>
						  <span v-else>loading...</span>
						</div>
            <ul class="list-group list-group-flush">
              <li class="list-group-item">Penulis: {{ buku.penulis }}</li>
              <li class="list-group-item">Penerbit: {{ buku.penerbit }}</li>
              <li class="list-group-item">{{ buku.deskripsi }}</li>
            </ul>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
const supabase = useSupabaseClient()

const route = useRoute()
const buku = ref({})

const getBookById = async () => {
  const { data, error } = await supabase
	  .from('buku')
		.select(`*, kategori(*)`)
		.eq('id', route.params.id)
		.single()
  if(data) buku.value = data
}

onMounted(() => {
  getBookById()
})
</script>

<style scoped>
.cover {
  width: 100%;
}
</style>
