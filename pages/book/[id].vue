<template>
  <div class="row">
    <div class="col-lg-12">
      <NuxtLink to="/" class="btn btn-primary mb-3">⬅️Back to home</NuxtLink>
    </div>
    <div class="col-3">
      <span v-if="loading">Sedang memuat gambar...</span>
      <span v-else><img :src="book.cover" alt="cover" class="cover" /></span>
    </div>
    <div class="col">
      <div v-if="loading">Sedang memuat data...</div>
      <div v-else>
        <h5>Judul: {{ book.judul }}</h5>
        <h6>Kategori: {{ book.kategori.nama }}</h6>
        <h6>Rak: {{ book.rak.kode }}</h6>
        <h6>Penulis: {{ book.penulis }}</h6>
        <h6>Penerbit: {{ book.penerbit }}</h6>
        <h6>Tahun terbit: {{ book.tahun_terbit }}</h6>
      </div>
    </div>
  </div>
</template>

<script setup>
const route = useRoute();
const id_buku = route.params.id;
const supabase = useSupabaseClient();
const book = ref({});
const loading = ref(true);

onMounted(() => getData());

async function getData() {
  loading.value = true;
  const { data, error } = await supabase
    .from("buku")
    .select(
      `
        id, judul, penulis, penerbit,
        kategori(nama), rak(kode),cover, tahun_terbit
      `
    )
    .eq("id", id_buku);
  if (data) {
    book.value = data[0];
    loading.value = false;
  }
  if (error) throw error;
}
</script>

<style scoped>
.cover {
  width: 100%;
}
</style>
