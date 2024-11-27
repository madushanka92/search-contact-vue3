<template>
    <div class="contact-table">
        <!-- Data Table -->
        <table border="1" cellspacing="0" cellpadding="5">
            <thead>
                <tr>
                    <th>Select</th>
                    <th>First Name</th>
                    <th>Last Name</th>
                    <th>Date of Birth</th>
                    <th>Address</th>
                    <th>City</th>
                    <th>State</th>
                    <th>Zip Code</th>
                    <th>Email</th>
                    <th>Phone Number</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(row, index) in paginatedData" :key="index"
                    :class="{ selected: selectedRow === rowIndex(index) }">
                    <td>
                        <input type="checkbox" :checked="selectedRow === rowIndex(index)"
                            @change="() => handleCheckboxChange(rowIndex(index))" />
                    </td>
                    <td>{{ row.FirstName }}</td>
                    <td>{{ row.LastName }}</td>
                    <td>{{ row.DateOfBirth }}</td>
                    <td>{{ row.Address }}</td>
                    <td>{{ row.City }}</td>
                    <td>{{ row.State }}</td>
                    <td>{{ row.ZipCode }}</td>
                    <td>{{ row.EmailAddress }}</td>
                    <td>{{ row.PhoneNumber }}</td>
                </tr>
            </tbody>
        </table>

        <!-- Pagination Controls -->
        <div class="pagination">
            <button :disabled="currentPage === 1" @click="changePage(currentPage - 1)">
                Previous
            </button>
            <span>Page {{ currentPage }} of {{ totalPages }}</span>
            <button :disabled="currentPage === totalPages" @click="changePage(currentPage + 1)">
                Next
            </button>
        </div>
    </div>
</template>

<script setup>
import { ref, computed, watch, onMounted } from 'vue';
import data from "../assets/dataset/data.json";

// Define the props
const props = defineProps({
    filters: {
        type: Object,
        default: () => ({}),
    },
});

// Emit the selected row data to the parent component
const emit = defineEmits(['selectRow']);

// Reactive variables
const tableData = ref([]);
const selectedRow = ref(null);
const currentPage = ref(1);
const rowsPerPage = ref(10);

/**
 * This function return the total number of pages based on the page count and total num of records. 
 */
const totalPages = computed(() => {
    return Math.ceil(filteredData.value.length / rowsPerPage.value);
});

/**
 * This function return the paginated data based on the current page and rows per page. 
 */
const paginatedData = computed(() => {
    const start = (currentPage.value - 1) * rowsPerPage.value;
    const end = start + rowsPerPage.value;
    return filteredData.value.slice(start, end);
});

/**
 * This function return the filtered data based on the search filters. 
 */
const filteredData = computed(() => {
    return tableData.value.filter(item => {
        return Object.entries(props.filters).every(([key, searchValue]) => {
            const normalizedSearchValue = searchValue.toString().toLowerCase();

            if (item.hasOwnProperty(key) && item[key].toString().toLowerCase().includes(normalizedSearchValue)) return item;
        });
    });
});

/**
 * This function receives the selected row index and emits the selected row data back to the parent component.
 * @param index selected row index
 */
const handleCheckboxChange = (index) => {
    const row = paginatedData.value[index];
    if (selectedRow.value === index) {
        selectedRow.value = null;
        emit('selectRow', null); // Clear selection
    } else {
        selectedRow.value = index;
        emit('selectRow', row); // Emit the selected row data
    }
};

/**
 * This function changes the current page based on the page number.
 * @param page page number
 */
const changePage = (page) => {
    currentPage.value = page;
};

/**
 * This function returns the row index based on the current page and row index.
 * @param index row index
 */
const rowIndex = (index) => {
    return (currentPage.value - 1) * rowsPerPage.value + index;
};

/**
 * This function watches for the filters changes and resets the selected row.
 */
onMounted(() => {
    tableData.value = data;
});

/**
 * This function watches for the filters changes and resets the selected row.
 */
watch(() => props.filters, () => {
    selectedRow.value = null;
}, { deep: true });
</script>

<style scoped>
.contact-table {
    background-color: white;
    padding: 60px;
    margin: 0px 15px;

    table {
        width: 100%;
    }

    .selected-info {
        margin-top: 20px;
        padding: 10px;
        border: 1px solid #ccc;
        background: #fafafa;
    }

    .pagination {
        margin-top: 20px;
        display: flex;
        align-items: center;
        gap: 10px;
    }

    .pagination button {
        padding: 5px 10px;
        cursor: pointer;
    }

    .pagination button:disabled {
        cursor: not-allowed;
        background-color: #ddd;
    }

    .selected {
        background-color: #f0f8ff;
    }
}
</style>