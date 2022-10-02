<template>
  <div>
    <div
      class="m-6 grid grid-cols-1 sm:grid-cols-2 md:grid-cols-2 lg:grid-cols-2 xl:grid-cols-2 gap-4"
    >
      <div
        v-for="p in products.data"
        :key="p.id"
        class="border rounded-lg bg-gray-100 hover:shadow-lg"
      >
        <nuxt-link :to="`/products/${p.id}`">
          <div class="rounded-t-lg bg-white pt-2 pb-2">
            <img
              width="275px"
              class="crop mx-auto rounded-sm"
              :src="`
            http://localhost:1337${p.attributes.image.data[0].attributes.url}`"
            />
          </div>
          <div class="pl-4 pr-4 pb-4 pt-4 rounded-lg">
            <h4
              class="mt-1 font-semibold text-base leading-tight truncate text-gray-700"
            >
              {{ p.attributes.title }}
            </h4>
            <div class="mt-1 text-sm text-gray-700">
              {{ p.attributes.description }}
            </div>
            <div class="mt-1 text-sm text-gray-700">
              {{ p.attributes.price }}$
            </div>
          </div>
        </nuxt-link>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      products: [],
    };
  },
  created: async function () {
    const res = await fetch("http://localhost:1337/api/products?populate=*");
    this.products = await res.json();
    console.log(this.products);
  },
};
</script>
