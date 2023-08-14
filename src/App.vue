<script>
import Form from "./components/Form.vue";
import Table from "./components/Table.vue";

export default {
  components: {
    Form,
    Table,
  },
  data() {
    return {
      tableData: [],
    };
  },
  methods: {
    addRow(row) {
      this.tableData.push(row);
    },
    removeRow(index) {
      if (confirm("Are you sure you want to delete this row?")) {
        this.tableData.splice(index, 1);
      }
    },
    exportData() {
      const exportModal = document.getElementById("exportModal");
      exportModal.classList.remove("hidden");
    },
  },
};
</script>

<template>
  <header>
    <h1 class="pt-8 pb-8 text-sky-500 text-3xl font-bold text-center">
      Form-Table App
    </h1>
  </header>

  <main class="container mx-auto p-8">
    <Form @addRow="addRow" />
    <Table :data="tableData" @removeRow="removeRow" @exportData="exportData" />
  </main>
</template>

<style></style>
