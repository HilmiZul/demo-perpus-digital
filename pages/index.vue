<template>
  <div class="container-fluid">
    <div class="row my-5">
      <div class="col-lg-6">
        <nuxt-link to="/pengunjung/tambah"> <!-- ini akan menuju ke halaman tambah pengunjung -->
          <div class="card bg-pengunjung rounded-5">
            <div class="card-body">
              <h2>Pengunjung</h2>
            </div>
          </div>
        </nuxt-link>
      </div>

      <div class="col-lg-6">
        <nuxt-link to="/buku">
          <div class="card bg-buku rounded-5">
            <div class="card-body">
              <h2>Cari Buku</h2>
            </div>
          </div>
        </nuxt-link>
      </div>
    </div>
    
    <div class="row my-5">
      <h2 class="mb-3">STATISTIK</h2>
      <div class="col-lg-6">
        <div class="card bg-stat-pengunjung rounded-5 align-items-center">
          <div class="card-body">
            <div class="statistik"><span class="angka">{{ visitor }}</span> Pengunjung</div>
          </div>
        </div>
      </div>
      <div class="col-lg-6">
        <div class="card bg-stat-buku rounded-5 align-items-center">
          <div class="card-body">
            <div class="statistik"><span class="angka">{{ book }}</span> Buku</div>
          </div>
        </div>
      </div>
    </div>

    <div class="row my-5">
      <div class="col-lg-12">
        <ChartStatistik />
      </div>
    </div>
  </div>
</template>

<script setup>
const supabase = useSupabaseClient()

const visitor = ref(0)
const book = ref(0)

const getVisitorCount = async () => {
  const { data, count } = await supabase
    .from('pengunjung')
    .select('*', { count: 'exact' })
  if(data) visitor.value = count
}

const getBookCount = async () => {
  const { data, count } = await supabase.from('buku')
    .select('*', { count: 'exact' })
  if(data) book.value = count
}

onMounted(() => {
  getVisitorCount()
  getBookCount()
})
</script>

<style scoped>
.card {
  height: 250px; /* tinggi card untuk semua elemen di komponen ini */
  box-shadow: 1px 1px 10px #424242;
}
.card.bg-pengunjung {
  background-image: url('../assets/img/bg-home-kunjungan.jpeg');
  background-repeat: no-repeat;
  background-position: center center;
  background-size: cover;
}
.card.bg-buku {
  background: url('../assets/img/bg-home-cari-buku.jpg') no-repeat center center;
  background-size: cover;
}

.bg-stat-pengunjung {
  background-color: #FCFFDB;
}
.bg-stat-buku {
  background-color: #ECFFF4;
}

.statistik {
  color: #5F5F5F;
  font-size: 4vh;
}
.statistik .angka {
  font-size: 10vh;
}
</style>