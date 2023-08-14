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
  <div class="container p-4 flex flex-col md:w-screen w-full items-center">
    <!-- Mobile Export Button -->

    <div
      v-if="data.length > 0 && windowWidth < 768"
      class="justify-end flex w-full"
    >
      <button
        @click="exportData"
        class="bg-green-500 text-white text-sm px-5 py-2.5 rounded-lg mb-6 sm:w-auto w-full"
      >
        Export
      </button>
      <!-- <div
        id="exportModal"
        class="hidden fixed inset-0 flex items-center justify-center bg-black bg-opacity-50"
      >
        <div class="bg-white p-4 rounded-lg shadow-md normal-case">
          <pre>{{ JSON.stringify(data, null, 2) }}</pre>
          <button
            @click="closeExportModal"
            class="mt-4 bg-blue-500 text-white text-sm px-4 py-2 rounded"
          >
            Close
          </button>
        </div>
      </div> -->
    </div>

    <!-- Table -->

    <table
      v-if="data.length > 0"
      class="table-fixed flex flex-row sm:flex-col text-center w-80 sm:w-full overflow-auto text-sm text-left text-gray-500 border-2 border-blue-300"
    >
      <div class="flex md:flex-col rounded-lg border-2 border-green-400">
        <!-- Mobile Table Header -->

        <div v-if="windowWidth < 768" class="flex flex-col gap-y-6">
          <thead
            v-for="(row, index) in data"
            :key="index"
            class="text-xs text-gray-700 left-60 right-0 uppercase"
          >
            <tr class="flex flex-col md:flex-row bg-gray-200 rounded-l-lg">
              <th scope="row" class="px-6 py-3 border-b border-gray-300">
                Name
              </th>
              <th scope="row" class="px-6 py-3 border-b border-gray-300">
                Surname
              </th>
              <th scope="row" class="px-6 py-3 border-b border-gray-300">
                Email
              </th>
              <th scope="row" class="px-6 py-3 border-b border-gray-300">
                Age
              </th>
              <th
                scope="row"
                class="px-6 py-3 border-b border-gray-300 whitespace-nowrap"
              >
                Favorite Color
              </th>
              <th
                scope="row"
                class="px-6 py-3 border-b border-gray-300 whitespace-nowrap"
              >
                Contact Preference
              </th>
              <th scope="row" class="px-6 py-3 whitespace-nowrap">Action</th>
            </tr>
          </thead>
        </div>

        <!-- Desktop Table Header -->

        <div v-else class="rounded-lg border-2 border-red-800">
          <thead
            class="text-xs text-gray-700 uppercase bg-gray-50 border-2 border-pink-200 flex"
          >
            <tr
              class="flex flex-row items-center justify-between w-full bg-gray-200 rounded-t-lg"
            >
              <th scope="col" class="px-6 py-3">Name</th>
              <th scope="col" class="px-6 py-3">Surname</th>
              <th scope="col" class="px-6 py-3">Email</th>
              <th scope="col" class="px-6 py-3">Age</th>
              <th scope="col" class="px-6 py-3 whitespace-nowrap">
                Favorite Color
              </th>
              <th scope="col" class="px-6 py-3 whitespace-nowrap">
                Contact Preference
              </th>
              <th scope="col" class="px-6 py-3">
                <button
                  @click="exportData"
                  class="bg-green-500 text-white text-sm px-4 py-2 rounded-lg"
                >
                  Export
                </button>
                <!-- <div
                  id="exportModal"
                  class="hidden fixed inset-0 flex items-center justify-center bg-black bg-opacity-50"
                >
                  <div class="bg-white p-4 rounded-lg shadow-md normal-case">
                    <pre>{{ JSON.stringify(data, null, 2) }}</pre>
                    <button
                      @click="closeExportModal"
                      class="mt-4 bg-blue-500 text-white text-sm px-4 py-2 rounded"
                    >
                      Close
                    </button>
                  </div>
                </div> -->
              </th>
            </tr>
          </thead>
        </div>

        <!-- Table Body -->

        <div class="w-full">
          <tbody
            class="w-full flex flex-col gap-y-6 md:gap-0 md:flex-none w-64 text-xs text-gray-700 rounded-r-lg border-2 border-black overflow-x-hidden"
          >
            <tr
              v-for="(row, index) in data"
              :key="index"
              class="flex flex-col md:flex-row md:whitespace-wrap w-full md:justify-between items-center text-left w-full text-xs bg-gray-50"
            >
              <td
                class="border-b border-gray-300 border-2 border-pink-600 w-full"
              >
                {{ row.name }}
              </td>
              <td
                class="border-b border-gray-300 border-2 border-pink-600 w-full"
              >
                {{ row.surname }}
              </td>
              <td
                class="border-b border-gray-300 border-2 border-pink-600 w-full"
              >
                {{ row.email }}
              </td>
              <td
                class="border-b border-gray-300 border-2 border-pink-600 w-full"
              >
                {{ row.age }}
              </td>
              <td
                class="border-b border-gray-300 border-2 border-pink-600 w-full"
              >
                {{ row.favoriteColor }}
              </td>
              <td
                class="border-b border-gray-300 border-2 border-pink-600 w-full whitespace-nowrap"
              >
                {{ row.contactPreference }}
              </td>
              <td v-if="windowWidth < 768" class="px-6 py-1 w-full">
                <button
                  @click="removeRow(index)"
                  class="bg-red-600 px-6 py-2 text-white rounded-lg hover:bg-red-700"
                >
                  Delete
                </button>
              </td>
              <td
                v-else
                class="px-6 py-1 border-2 border-pink-600 w-full flex justify-end"
              >
                <button
                  @click="removeRow(index)"
                  class="bg-red-600 px-6 py-2 text-white rounded-lg hover:bg-red-700"
                >
                  Delete
                </button>
              </td>
            </tr>
          </tbody>
        </div>
      </div>
    </table>
    <p v-else class="mt-4 text-gray-500">No Data</p>
  </div>

  <div v-if="data.length > 0">
    <!-- New Table Desktop -->
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
              <div
                id="exportModal"
                class="hidden fixed inset-0 flex items-center justify-center bg-black bg-opacity-50"
              >
                <div
                  class="bg-white p-4 rounded-lg shadow-md normal-case text-gray-900"
                >
                  <pre>{{ JSON.stringify(data, null, 2) }}</pre>
                  <button
                    @click="closeExportModal"
                    class="mt-4 border-2 border-gray-500 bg-gray-200 hover:bg-gray-400 text-gray-700 font-medium text-sm px-5 py-2.5 rounded-lg"
                  >
                    Close
                  </button>
                </div>
              </div>
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
    <!-- New Mobile Table -->
    <div class="block md:hidden shadow-md overflow-x-auto">
      <table
        v-for="(row, index) in data"
        :key="index"
        class="text-sm text-left rounded-lg mb-6"
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
      </table>
    </div>
  </div>
  <div v-else>
    <p class="mt-4 text-gray-500">No Data</p>
  </div>
</template>

<style>
.th {
  @apply bg-slate-700 uppercase text-white text-xs font-medium px-6 py-3 whitespace-nowrap
}

.td {
  @apply bg-gray-50 text-xs text-gray-900 px-6 py-4
}

.tr {
  @apply border-b w-full
}
</style>
