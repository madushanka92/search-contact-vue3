<script setup>
import { ref } from "vue";
import ContactForm from "./components/contact-form.vue";
import DataTable from "./components/contact-table.vue";

// Reactive search filters
const searchFilters = ref({});

// Reactive selected record
const selectedRecord = ref(null);

// Function to handle row selection
const handleRowSelection = (record) => {
  selectedRecord.value = { ...record };
};
</script>

<template>
  <!-- Pass the search filters and selected record to the child components -->
  <ContactForm @submit="searchFilters = $event" :record="selectedRecord" />

  <!-- Pass the search filters to the child component -->
  <DataTable :filters="searchFilters" @selectRow="handleRowSelection" />
</template>
