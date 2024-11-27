<template>
    <div class="contact-form mt-3 mb-3">
        <h4>Search for a contact</h4>
        <form @submit.prevent="handleSubmit">
            <!-- Row 1 -->
            <div class="row mb-3">
                <!-- First Name, Last Name, Date of Birth -->
                <div class="col-md-6">
                    <div class="mb-3">
                        <label for="FirstName" class="form-label">First Name</label>
                        <input type="text" id="FirstName" v-model="formData.FirstName" class="form-control" />
                    </div>
                    <div class="mb-3">
                        <label for="LastName" class="form-label">Last Name</label>
                        <input type="text" id="LastName" v-model="formData.LastName" class="form-control" />
                    </div>
                    <div class="mb-3">
                        <label for="DateOfBirth" class="form-label">Date of Birth</label>
                        <input type="date" id="DateOfBirth" v-model="formData.DateOfBirth" class="form-control" />
                    </div>
                </div>

                <!-- Street Address -->
                <div class="col-md-6">
                    <div class="mb-3">
                        <label for="Address" class="form-label">Street Address</label>
                        <input type="text" id="Address" v-model="formData.Address" class="form-control" />
                    </div>
                </div>
            </div>

            <!-- Row 2 -->
            <div class="row">
                <!-- Email and Phone -->
                <div class="col-md-6">
                    <div class="mb-3">
                        <label for="EmailAddress" class="form-label">Email Address</label>
                        <input type="email" id="EmailAddress" v-model="formData.EmailAddress" class="form-control" />
                    </div>
                    <div class="mb-3">
                        <label for="PhoneNumber" class="form-label">Phone Number</label>
                        <input type="text" id="PhoneNumber" v-model="formData.PhoneNumber" class="form-control"
                            :class="{ 'is-invalid': !isPhoneNumberValid && formData.PhoneNumber !== '' }"
                            @blur="validatePhoneNumber" placeholder="e.g., 416-555-1234" />
                        <div v-if="formData.PhoneNumber !== '' && !isPhoneNumberValid" class="invalid-feedback">
                            Please enter a valid phone number (e.g., 416-555-1234).
                        </div>
                    </div>
                </div>

                <!-- City, State, and Zip Code -->
                <div class="col-md-6">
                    <div class="mb-3">
                        <label for="City" class="form-label">City</label>
                        <input type="text" id="City" v-model="formData.City" class="form-control" />
                    </div>
                    <div class="mb-3">
                        <label for="State" class="form-label">State/Province</label>
                        <select id="State" v-model="formData.State" class="form-select">
                            <option value="">Select State/Province</option>
                            <option v-for="province in provinces" :key="province.value" :value="province.value">
                                {{ province.name }}
                            </option>
                        </select>
                    </div>
                    <div class="mb-3">
                        <label for="ZipCode" class="form-label">Zip Code</label>
                        <input type="text" id="ZipCode" v-model="formData.ZipCode" class="form-control"
                            :class="{ 'is-invalid': !isValidZipCode && formData.ZipCode !== '' }"
                            @blur="validateZipCode" placeholder="e.g., A1A 1A1" />
                        <div v-if="formData.ZipCode !== '' && !isValidZipCode" class="invalid-feedback">
                            Please enter a valid Canadian postal code (e.g., A1A 1A1).
                        </div>
                    </div>
                </div>
            </div>

            <!-- Footer Level Buttons -->
            <div class="d-flex gap-2">
                <button type="submit" class="btn btn-primary" :disabled="!isFormValid">
                    Search
                </button>
                <button type="button" class="btn btn-secondary" @click="clearForm">
                    Clear
                </button>
            </div>
        </form>
    </div>
</template>

<script setup>
import { ref, reactive, watch, computed } from 'vue';

// Define the emits
const emit = defineEmits(['submit']);

// Define the props
const props = defineProps({
    record: {
        type: Object,
        default: () => ({}),
    },
});

// Reactive form data
const formData = reactive({
    FirstName: '',
    LastName: '',
    DateOfBirth: '',
    Address: '',
    EmailAddress: '',
    PhoneNumber: '',
    City: '',
    State: '',
    ZipCode: '',
});

// List of Canadian provinces
const provinces = [
    { name: "Alberta", value: "AB" },
    { name: "British Columbia", value: "BC" },
    { name: "Manitoba", value: "MB" },
    { name: "New Brunswick", value: "NB" },
    { name: "Newfoundland and Labrador", value: "NL" },
    { name: "Northwest Territories", value: "NT" },
    { name: "Nova Scotia", value: "NS" },
    { name: "Nunavut", value: "NU" },
    { name: "Ontario", value: "ON" },
    { name: "Prince Edward Island", value: "PE" },
    { name: "Quebec", value: "QC" },
    { name: "Saskatchewan", value: "SK" },
    { name: "Yukon", value: "YT" },
];

// Reactive variables
const isPhoneNumberValid = ref(true);
const isValidZipCode = ref(true);

// Regex for Canadian Phone Number validation (e.g., 416-555-1234)
const phoneNumberPattern = /^\d{3}-\d{3}-\d{4}$/;

const validatePhoneNumber = () => {
    // If the phone number is not empty, validate it
    isPhoneNumberValid.value = formData.PhoneNumber === '' || phoneNumberPattern.test(formData.PhoneNumber);
};

// Regex for Canadian postal code validation
const zipCodePattern = /^[A-Za-z]\d[A-Za-z] \d[A-Za-z]\d$/;

/**
 * Validates the Canadian postal code.
 */
const validateZipCode = () => {
    // If the postal code is not empty, validate it
    isValidZipCode.value = formData.ZipCode === '' || zipCodePattern.test(formData.ZipCode);
};

/**
 * Handles the form submission.
 */
const handleSubmit = () => {
    emit('submit', { ...formData });
};

/**
 * Clears the form.
 */
const clearForm = () => {
    for (let key in formData) {
        formData[key] = '';
    }
    handleSubmit();
};

// Watch for record changes
watch(
    () => props.record,
    (newRecord) => {
        if (newRecord && Object.keys(newRecord).length > 0) {
            // Populate formData with the new record
            Object.assign(formData, newRecord);
        } else {
            // Clear the form if the record is empty or null
            clearForm();
        }
    },
    { immediate: true, deep: true }
);

// Computed to check if form is valid
const isFormValid = computed(() => {
    // Ensure that the form is valid only when both the postal code and phone number (if filled) are valid
    return isValidZipCode.value && isPhoneNumberValid.value;
});
</script>

<style lang="scss">
.contact-form {
    background-color: white;
    padding: 20px;
    margin: 0px 15px;

    h4 {
        text-align: start;
    }

    input {
        width: 200px;
        border-color: black;
    }

    select {
        border-color: black;
    }

    .row {
        >div {
            display: flex;
            flex-direction: row;
            gap: 1em;

            >div {
                display: flex;
                flex-direction: column;
            }
        }
    }

    .is-invalid {
        border-color: red;
    }

    .invalid-feedback {
        color: red;
        font-size: 0.875em;
    }
}
</style>
