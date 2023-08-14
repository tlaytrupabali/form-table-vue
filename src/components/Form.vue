<script>
export default {
  data() {
    return {
      name: "",
      surname: "",
      email: "",
      age: "",
      favoriteColor: "",
      contactPreference: [],
      colors: ["red", "green", "blue", "white", "black"],
      validAge: true,
    };
  },
  methods: {
    // Age Validation

    validateAge() {
      if (this.$refs.age.value < 1 || this.$refs.age.value > 120) {
        this.$refs.age.setCustomValidity(
          "Age must be at least 1 and no more than 120."
        );
        this.$refs.age.reportValidity();
        this.age = '';
        this.validAge = false;
      } else {
        this.$refs.age.setCustomValidity("");
      }
    },
    resetAgeValue() {
      this.$refs.age.value = "";
    },

    // Email Validation

    validateEmail() {
      if (
        /^\w+([\.-]?\w+)*@\w+([\.-]?\w+)*(\.\w{2,3})+$/.test(
          this.$refs.email.value
        )
      ) {
        this.$refs.email.setCustomValidity("");
      } else {
        this.$refs.email.setCustomValidity(
          "Please enter a valid email address."
        );
        this.$refs.email.reportValidity();
      }
    },
    resetEmail() {
      this.$refs.email.value = "";
    },

    // Submit Form

    submitForm() {
      // Check Form Validity
      if (
        !this.name ||
        !this.surname ||
        !this.email ||
        isNaN(this.age) ||
        this.age < 1 ||
        this.age > 120 ||
        !this.favoriteColor ||
        this.contactPreference.length === 0
      ) {
        alert(
          "Please fill in all required fields and ensure the entered values are valid."
        );
        return;
      } else {
        this.$emit("addRow", {
          name: this.name,
          surname: this.surname,
          email: this.email,
          age: this.age,
          favoriteColor: this.favoriteColor,
          contactPreference: this.contactPreference.join(", "),
        });

        // Reset Form fields

        this.name = "";
        this.surname = "";
        this.email = "";
        this.age = "";
        this.favoriteColor = "";
        this.contactPreference = [];
      }
    },
  },
};
</script>

<template>
  <div class="p-4 w-full">
    <form id="dataForm" autocomplete="on">
      <!-- Input Fields -->

      <div class="sm:grid sm:grid-cols-2 sm:gap-4">
        <div>
          <label for="name" class="block mb-2 text-sm font-bold text-gray-900"
            >Name</label
          >
          <input
            v-model="name"
            type="text"
            id="name"
            name="name"
            required
            class="w-full border rounded-lg p-2 mb-4"
          />
        </div>
        <div>
          <label
            for="surname"
            class="block mb-2 text-sm font-bold text-gray-900"
            >Surname</label
          >
          <input
            v-model="surname"
            type="text"
            id="surname"
            name="surname"
            required
            class="w-full border rounded-lg p-2 mb-4"
          />
        </div>
      </div>
      <label for="email" class="block mb-2 text-sm font-bold text-gray-900"
        >Email</label
      >
      <input
        v-model="email"
        @input="validateEmail"
        ref="email"
        type="email"
        id="email"
        name="email"
        required
        class="w-full border rounded-lg p-2 mb-4"
      />
      <div v-if="validAge">
        <label for="age" class="block mb-2 text-sm font-bold text-gray-900"
        >Age</label
        >
        <input
        v-model="age"
        @change="validateAge"
        ref="age"
        type="number"
        id="age"
        name="age"
        min="0"
        max="120"
        required
        class="w-full border rounded-lg p-2 mb-4"
        />
      </div>
      <div v-else>
          <label
            for="age-error"
            class="block mb-2 text-sm font-bold text-red-700"
            >Age</label
          >
          <input
            type="number"
            @focus="validAge = true"
            id="age-error"
            class="w-full p-2 block bg-red-50 border border-red-500 text-red-900 placeholder-red-700 text-sm rounded-lg focus:ring-red-500 focus:border-red-500"
          />
          <p class="mt-2 mb-4 text-sm text-red-600">
            <span class="font-medium">Oops!</span> Age must be at least 1 and no more than 120!
          </p>
        </div>
      
      <!-- Favorite Color -->

      <label
        for="favoriteColor"
        class="block mb-2 text-sm font-bold text-gray-900"
        >Favorite Color</label
      >
      <select
        v-model="favoriteColor"
        id="favoriteColor"
        name="favoriteColor"
        required
        class="mb-4 bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5"
      >
        <option disabled value="">Please choose an option</option>
        <option value="red">Red</option>
        <option value="green">Green</option>
        <option value="blue">Blue</option>
        <option value="white">White</option>
        <option value="black">Black</option>
      </select>

      <!-- Contact Preference -->

      <label for="checkbox" class="block mb-2 text-sm font-bold text-gray-900"
        >Contact Preference</label
      >
      <div class="flex items-center my-4">
        <input
          v-model="contactPreference"
          type="checkbox"
          id="emailPreference"
          name="contactPreference[]"
          value="Email"
          class="w-4 h-4 rounded-lg focus:ring-2 focus:ring-blue-500"
        />
        <label
          for="emailPreference"
          class="ml-2 text-sm font-medium text-gray-900"
          >Email</label
        >
      </div>
      <div class="flex items-center mb-4">
        <input
          v-model="contactPreference"
          type="checkbox"
          id="phoneCallPreference"
          name="contactPreference[]"
          value="Phone Call"
          class="w-4 h-4 rounded-lg focus:ring-2 focus:ring-blue-500"
        />
        <label
          for="phoneCallPreference"
          class="ml-2 text-sm font-medium text-gray-900"
          >Phone Call</label
        >
      </div>
      <div class="flex items-center mb-4">
        <input
          v-model="contactPreference"
          type="checkbox"
          id="smsPreference"
          name="contactPreference[]"
          value="SMS"
          class="w-4 h-4 rounded-lg focus:ring-2 focus:ring-blue-500"
        />
        <label
          for="smsPreference"
          class="ml-2 text-sm font-medium text-gray-900"
          >SMS</label
        >
      </div>
      <div class="flex justify-end">
        <!-- Submit Button -->

        <button
          type="submit"
          @click="submitForm"
          class="text-white bg-sky-500 hover:bg-sky-700 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm w-full sm:w-auto px-5 py-2.5 text-center"
        >
          Submit
        </button>
      </div>
    </form>
  </div>
</template>
