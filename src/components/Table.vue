<script>
export default {
  props: {
    data: Array,
  },
  data() {
    return {
      windowWidth: window.innerWidth,
    };
  },
  mounted() {
    this.$nextTick(() => {
      window.addEventListener("resize", this.onResize);
    });
  },
  beforeUnmount() {
    window.removeEventListener("resize", this.onResize);
  },
  methods: {
    onResize() {
      this.windowWidth = window.innerWidth;
    },
    removeRow(index) {
      this.$emit("removeRow", index);
    },
    exportData() {
      this.$emit("exportData");
    },
    closeExportModal() {
      const exportModal = document.getElementById("exportModal");
      exportModal.classList.add("hidden");
    },
  },
};
</script>

<template>
  <div v-if="data.length > 0">
    <!-- Table Desktop -->

    <div class="hidden md:block shadow-md overflow-x-auto rounded-lg">
      <table class="min-w-full text-sm text-left">
        <thead class="text-sm text-white uppercase bg-slate-700">
          <tr>
            <th scope="col" class="px-6 py-3">Name</th>
            <th scope="col" class="px-6 py-3">Surname</th>
            <th scope="col" class="px-6 py-3">Email</th>
            <th scope="col" class="px-6 py-3">Age</th>
            <th scope="col" class="px-6 py-3">Favorite Color</th>
            <th scope="col" class="px-6 py-3">Contact Preference</th>
            <th scope="col" class="px-6 py-3">
              <a
                @click="exportData"
                href="#"
                class="bg-green-600 px-5 py-2.5 rounded-lg normal-case font-medium text-sm hover:bg-green-800"
                >Export</a
              >
            </th>
          </tr>
        </thead>
        <tbody v-for="(row, index) in data" :key="index">
          <tr class="bg-gray-50 text-gray-900 border-b">
            <th scope="row" class="px-6 py-4">{{ row.name }}</th>
            <td class="px-6 py-4">{{ row.surname }}</td>
            <td class="px-6 py-4">{{ row.email }}</td>
            <td class="px-6 py-4">{{ row.age }}</td>
            <td class="px-6 py-4">{{ row.favoriteColor }}</td>
            <td class="px-6 py-4">{{ row.contactPreference }}</td>
            <td class="px-6 py-4 text-right">
              <a
                @click="removeRow(index)"
                href="#"
                class="font-medium text-red-600 hover:underline"
                >Delete</a
              >
            </td>
          </tr>
        </tbody>
      </table>
    </div>

    <!-- Mobile Table -->

    <div class="block md:hidden overflow-x-auto mx-auto">
      <div class="w-full flex justify-end px-3 pb-4">
        <a
          @click="exportData"
          href="#"
          class="w-full text-center  sm:w-auto bg-green-600 px-5 py-2.5 rounded-lg normal-case font-medium text-sm text-white hover:bg-green-800"
          >Export</a
        >
      </div>
      <table
        v-for="(row, index) in data"
        :key="index"
        class="text-sm text-left rounded-lg mb-6 shadow-md mx-auto w-full rounded-lg"
      >
        <tr class="tr">
          <th class="th">Name</th>
          <td class="td">{{ row.name }}</td>
        </tr>
        <tr class="tr">
          <th class="th">Surname</th>
          <td class="td">{{ row.surname }}</td>
        </tr>
        <tr class="tr">
          <th class="th">Email</th>
          <td class="td">{{ row.email }}</td>
        </tr>
        <tr class="tr">
          <th class="th">Age</th>
          <td class="td">{{ row.age }}</td>
        </tr>
        <tr class="tr">
          <th class="th">Favorite Color</th>
          <td class="td">{{ row.favoriteColor }}</td>
        </tr>
        <tr class="tr">
          <th class="th">Contact Preference</th>
          <td class="td">{{ row.contactPreference }}</td>
        </tr>
        <tr class="tr">
          <th class="th"></th>
          <td class="td">
            <a
              @click="removeRow(index)"
              href="#"
              class="font-medium text-red-600 hover:underline"
              >Delete</a
            >
          </td>
        </tr>
      </table>
    </div>
  </div>
  <div v-else class="flex flex-col items-center">
    <p class="mt-4 text-gray-500">No Data</p>
  </div>

  <!-- Export Modal -->

  <div
    id="exportModal"
    class="hidden fixed inset-0 flex items-center justify-center bg-black bg-opacity-50"
  >
    <div class="bg-white p-4 rounded-lg shadow-md normal-case text-gray-900">
      <pre>{{ JSON.stringify(data, null, 2) }}</pre>
      <button
        @click="closeExportModal"
        class="mt-4 border-2 border-gray-500 bg-gray-200 hover:bg-gray-400 text-gray-700 font-medium text-sm px-5 py-2.5 rounded-lg"
      >
        Close
      </button>
    </div>
  </div>
</template>

<style>
.th {
  @apply bg-slate-700 uppercase text-white text-xs font-medium px-6 py-3 w-1/3 whitespace-nowrap;
}

.td {
  @apply bg-gray-50 text-xs text-gray-900 px-6 py-4;
}

.tr {
  @apply border-b;
}
</style>
