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
          <input type="search" class="form-control form-control-lg rounded-5" placeholder="Filter...">
        </div>
        <div class="my-3 text-muted">menampilkan 1 dari 1</div>
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
            <tr v-for="(visitor,i) in visitors" :key="i">
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

const getPengunjung = async () => {
  const { data, error } = await supabase.from('pengunjung')
    .select(`*, keanggotaan(*), keperluan(*)`)
    .order('id', { ascending: false })
  if(data) visitors.value = data
}

onMounted(() => {
  getPengunjung()
})

</script>