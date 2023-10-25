<template>
  <div>
    <h4>Cari buku yang kamu mau disini</h4>
    <div class="my-3">
      <form @submit.prevent="getData">
        <input v-model="keyword" class="form-control form-control-lg rounded-pill" placeholder="Mau baca buku apa nihh.." />
      </form>
    </div>
    <div class="row">
      <div v-for="book in books" :key="book.id" class="col-2" style="margin-bottom: 20px">
        <div class="card">
          <div class="card-header">
            <img :src="book.cover" alt="cover" class="cover" />
            <p>{{ book.judul }}</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
const supabase = useSupabaseClient();
const books = ref([]);
const keyword = ref([]);

onMounted(() => getData());

async function getData() {
  const { data, error } = await supabase
    .from("buku")
    .select(
      `
      id, judul, penulis, penerbit,
      kategori(nama), rak(kode),cover
    `
    )
    .ilike("judul", `%${keyword.value}%`);
  if (data) books.value = data;
  if (error) throw error;
}
</script>

<style scoped>
.cover {
  width: 50%;
  margin-bottom: 50px;
}
</style>
