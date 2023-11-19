<template>
  <div>
    <h4>Buku Yang Tersedia di Perpustakaan</h4>
    <div class="row">
      <div class="col">
        <div class="my-3">
          <form @submit.prevent="getData">
            <input v-model="keyword" class="form-control form-control-lg rounded-pill" placeholder="Cari buku yang kamu mau disini.." />
          </form>
        </div>
      </div>
    </div>

    <div class="row">
      <div v-if="loading">
        <div class="text-center">
          <button class="btn btn-primary" type="button" disabled>
            <span class="spinner-border spinner-border-sm" role="status" aria-hidden="true"></span>
            Sedang memuat buku...
          </button>
        </div>
      </div>

      <div v-for="book in books" :key="book.id" class="col-2">
        <div class="card mb-3">
          <NuxtLink :to="`/book/${book.id}`">
            <div class="card-header">
              <img :src="book.cover" alt="cover" class="cover" />
              <h6>{{ book.judul }}</h6>
            </div>
          </NuxtLink>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
const supabase = useSupabaseClient();
const books = ref([]);
const keyword = ref([]);
const loading = ref(true);

onMounted(() => getData());

async function getData() {
  loading.value = true;
  const { data, error } = await supabase
    .from("buku")
    .select(
      `
      id, judul, penulis, penerbit,
      kategori(nama), rak(kode),cover
    `
    )
    .ilike("judul", `%${keyword.value}%`);
  if (data) {
    books.value = data;
    loading.value = false;
  }
  if (error) throw error;
}
</script>

<style scoped>
.cover {
  width: 100%;
  margin-bottom: 50px;
}
</style>
