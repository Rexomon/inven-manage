<script>
import { ref } from "vue";
import axios from "axios";

const totalProducts = ref(0);
const lowStockProducts = ref(0);
const outOfStockProducts = ref(0);

export default {
	data() {
		return {
			totalProducts,
			lowStockProducts,
			outOfStockProducts,
		};
	},

	mounted() {
		this.fetchData();
	},

	methods: {
		async fetchData() {
			try {
				const response = await axios.get(
					`${import.meta.env.VITE_BACKEND_PORT}/products/summary`,
				);
				const isiData = response.data.summary;

				if (isiData) {
					totalProducts.value = isiData.productCount;
					lowStockProducts.value = isiData.lowStock.length;
					outOfStockProducts.value = isiData.outOfStock.length;
				}
			} catch (error) {
				console.error("Error fetching products:", error);
			}
		},
	},
};
</script>

<template>
  <main class="p-4">
    <h1 class="text-2xl font-bold mb-4">Dashboard Inventaris</h1>

    <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
      <div class="bg-white p-4 rounded shadow">
        <h2 class="text-xl font-semibold">Total Produk</h2>
        <p class="text-3xl">{{ totalProducts }}</p>
      </div>
      <div class="bg-white p-4 rounded shadow">
        <h2 class="text-xl font-semibold">Produk Stok Rendah</h2>
        <p class="text-3xl">{{ lowStockProducts }}</p>
      </div>
      <div class="bg-white p-4 rounded shadow">
        <h2 class="text-xl font-semibold">Produk Stok Habis</h2>
        <p class="text-3xl">{{ outOfStockProducts }}</p>
      </div>
    </div>

    <div class="mt-8">
      <h2 class="text-xl font-semibold mb-4">Tautan Cepat</h2>
      <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
        <router-link to="/add-product" class="bg-blue-500 text-white p-4 rounded shadow text-center hover:bg-blue-600">
          Tambah Produk Baru
        </router-link>
        <router-link to="/list-products" class="bg-green-500 text-white p-4 rounded shadow text-center hover:bg-green-600">
          Lihat Daftar Produk
        </router-link>
        <router-link to="/reports" class="bg-yellow-500 text-white p-4 rounded shadow text-center hover:bg-yellow-600">
          Laporan Inventaris
        </router-link>
      </div>
    </div>
  </main>
</template>

<style scoped>

main {
  margin-top: 60px;
}

</style>
