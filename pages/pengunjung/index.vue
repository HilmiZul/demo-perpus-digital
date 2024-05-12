<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-lg-12">
        <h2 class="text-center my-4">
          <div class="float-start">
            <NuxtLink to="/" class="btn btn-light me-2 rounded-5">Home</NuxtLink>
            <NuxtLink to="/pengunjung/tambah" class="btn btn-light rounded-5">Isi Kunjungan</NuxtLink>
          </div>
          RIWAYAT KUNJUNGAN
        </h2>
        <div class="my-3">
          <input v-model="keyword" type="search" class="form-control form-control-lg rounded-5" placeholder="Filter...">
        </div>
				<div class="my-3 text-muted">menampilkan {{ filteredData.length }} dari {{ visitors.length }}</div>
        <table class="table">
          <thead>
            <tr>
              <td>#</td>
              <td>NAMA</td>
              <td>KEANGGOTAAN</td>
              <td>WAKTU</td>
              <td>KEPERLUAN</td>
            </tr>
          </thead>
          <tbody>
            <tr v-if="loading">
              <td colspan="5" class="text-center"><h1>TUNGGU SEBENTAR...</h1></td>
            </tr>
            <tr v-for="(visitor,i) in filteredData" :key="i">
              <td>{{ i+1 }}.</td>
              <td>{{ visitor.nama }}</td>
              <td>{{ visitor.keanggotaan.nama }}</td>
              <td>{{ visitor.tanggal }}, {{ visitor.waktu }}</td>
              <td>{{ visitor.keperluan.nama }}</td>
            </tr>
          </tbody>
				</table> 
      </div>
    </div>
  </div>
</template>

<script setup>
const supabase = useSupabaseClient()

const visitors = ref([])
const loading = ref(true)
const keyword = ref("")

const getPengunjung = async () => {
  const { data, error } = await supabase.from('pengunjung')
    .select(`*, keanggotaan(*), keperluan(*)`)
    .order('id', { ascending: false })
  if(data) {
    loading.value = false
    visitors.value = data
  }
}

onMounted(() => {
  getPengunjung()
})

const filteredData = computed(() => {
  return visitors.value.filter((i) => {
	  return (
		  i.nama.toLowerCase().includes(keyword.value.toLowerCase())
		)
	})
})
</script>



