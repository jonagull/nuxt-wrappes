<template>
  <div class="flex justify-center m-6">
    <div v-if="this.product !== null">
      <div class="flex flex-col items-center border rounded-lg bg-gray-100">
        <div class="w-full bg-white rounded-lg flex justify-center">
          <img
            class="rounded-sm"
            :src="`
            http://localhost:1337${path.image.data[0].attributes.url}`"
            width="375"
          />
        </div>
        <div class="w-full p-5 flex flex-col justify-between">
          <div>
            <h4
              class="mt-1 font-semibold text-lg leading-tight truncate text-gray-700"
            >
              lol
              {{ path.title }}
            </h4>
            <div class="mt-1 text-gray-600">{{ path.description }}</div>
            <div class="mt-1 text-gray-600">{{ path.price }}$</div>
          </div>
          <button
            class="snipcart-add-item mt-4 bg-white hover:bg-gray-100 text-gray-800 font-semibold py-2 px-4 border border-gray-400 rounded shadow"
            :data-item-id="product.data.id"
            :data-item-price="path.price"
            :data-item-url="`${this.$route.fullPath}`"
            :data-item-description="path.description"
            :data-item-image="`
            http://localhost:1337${path.image.data[0].attributes.url}`"
            :data-item-name="path.title"
            v-bind="customFields"
          >
            Add to cart
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      product: null,
      storeUrl: process.env.storeUrl,
    };
  },
  created: async function () {
    const res = await fetch(
      `http://localhost:1337/api/products/${this.$route.params.id}?populate=*`
    );
    this.product = await res.json();
    this.path = this.product.data.attributes;
  },
  computed: {
    customFields() {
      return this.path.custom_field
        .map(({ title, required, options }) => ({
          name: title,
          required,
          options,
        }))
        .map((x, index) =>
          Object.entries(x).map(([key, value]) => ({
            [`data-item-custom${index + 1}-${key.toString().toLowerCase()}`]:
              value,
          }))
        )
        .reduce((acc, curr) => acc.concat(curr), [])
        .reduce((acc, curr) => ({ ...acc, ...curr }));
    },
  },
};
</script>
<style></style>
