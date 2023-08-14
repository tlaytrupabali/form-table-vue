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
  <div class="p-4 w-full flex flex-col w-screen items-center">
    <table
      v-if="data.length > 0"
      class="flex flex-row flex-wrap sm:flex-col items-center w-80 sm:w-full overflow-auto text-sm text-left text-gray-500"
    >
      <div v-if="windowWidth < 768">
        <button
          @click="exportData"
          class="bg-green-500 text-white px-4 py-2 rounded-lg mb-6"
        >
        Export
      </button>
    </div>
      <div class="flex md:flex-col rounded-lg">
        <div v-if="windowWidth < 768" class="flex flex-col gap-y-6 relative">
          <thead
            v-for="(row, index) in data"
            :key="index"
            class="text-xs text-gray-700 sticky left-60 right-0"
          >
            <tr class="flex flex-col md:flex-row bg-gray-200 rounded-l-lg">
              <th scope="col" class="px-6 py-3 border-b border-gray-300">
                Name
              </th>
              <th scope="col" class="px-6 py-3 border-b border-gray-300">
                Surname
              </th>
              <th scope="col" class="px-6 py-3 border-b border-gray-300">
                Email
              </th>
              <th scope="col" class="px-6 py-3 border-b border-gray-300">
                Age
              </th>
              <th
                scope="col"
                class="px-6 py-3 border-b border-gray-300 whitespace-nowrap"
              >
                Favorite Color
              </th>
              <th
                scope="col"
                class="px-6 py-3 border-b border-gray-300 whitespace-nowrap"
              >
                Contact Preference
              </th>
              <th scope="col" class="px-6 py-3 whitespace-nowrap">Action</th>
            </tr>
          </thead>
        </div>
        <div v-else>
          <thead class="text-xs text-gray-700 uppercase bg-gray-50">
            <tr class="flex flex-col md:flex-row bg-gray-200">
              <th scope="col" class="px-6 py-3">Name</th>
              <th scope="col" class="px-6 py-3">Surname</th>
              <th scope="col" class="px-6 py-3">Email</th>
              <th scope="col" class="px-6 py-3">Age</th>
              <th scope="col" class="px-6 py-3">Favorite Color</th>
              <th scope="col" class="px-6 py-3">Contact Preference</th>
              <th scope="col" class="px-6 py-3">
                <button
                  @click="exportData"
                  class="bg-green-500 text-white px-4 py-2 rounded-lg"
                >
                  Export
                </button>
                <div
                  id="exportModal"
                  class="hidden fixed inset-0 flex items-center justify-center bg-black bg-opacity-50"
                >
                  <div
                    class="bg-white p-4 rounded-lg shadow-md min-w-64 normal-case"
                  >
                    <pre>{{ JSON.stringify(data, null, 2) }}</pre>
                    <button
                      @click="closeExportModal"
                      class="mt-4 bg-blue-500 text-white px-4 py-2 rounded"
                    >
                      Close
                    </button>
                  </div>
                </div>
              </th>
            </tr>
          </thead>
        </div>
        <div>
          <tbody
            class="flex flex-col gap-y-6 items-stretch w-64 text-xs text-gray-700 rounded-r-lg"
          >
            <tr
              v-for="(row, index) in data"
              :key="index"
              class="flex flex-col md:flex-row text-xs bg-gray-50"
            >
              <td class="px-6 py-3 border-b border-gray-300">{{ row.name }}</td>
              <td class="px-6 py-3 border-b border-gray-300">
                {{ row.surname }}
              </td>
              <td class="px-6 py-3 border-b border-gray-300">
                {{ row.email }}
              </td>
              <td class="px-6 py-3 border-b border-gray-300">{{ row.age }}</td>
              <td class="px-6 py-3 border-b border-gray-300">
                {{ row.favoriteColor }}
              </td>
              <td class="px-6 py-3 border-b border-gray-300 whitespace-nowrap">
                {{ row.contactPreference }}
              </td>
              <td v-if="windowWidth < 768" class="px-6 py-1">
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
</template>
