<script setup>
import { ref } from "vue";

defineProps({
  // msg: String,
});
</script>

<template>
  <div v-if="isLoading" class="loader">Loading...</div>
  <div v-else>
    <h1 class="text-3xl font-bold underline">Card List</h1>
    <div class="grid grid-cols-4 gap-4">
      <div v-for="item in items" key="id">
        <div class="card card-compact w-96 bg-base-100 shadow-xl">
          <figure>
            <img src="`${item.logo}`" alt="AirlineImg" />
          </figure>
          <div class="card-body">
            <h2 class="card-title">{{ item.name }}</h2>
            <p>{{ item.country }}</p>

            <p>{{ item.slogan }}</p>
            <p>{{ item.head_quaters }}</p>
            <p>{{ item.website }}</p>
            <p>{{ item.established }}</p>
            <div class="card-actions justify-end">
              <button class="btn btn-primary">Buy Now</button>
            </div>
          </div>
        </div>
        <!-- <Card /> -->
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      isLoading: true,
      items: [],
    };
  },
  created() {
    // Call to my API
    this.fetchData();
  },
  methods: {
    async fetchData() {
      try {
        let response = await Promise.race([
          fetch("http://localhost:80/api/airlines"),
          new Promise((resolve, reject) =>
            setTimeout(() => reject(new Error("Timeout")), 30000)
            // PROBLEM: CORS same origin
          ),
        ]).then((response) => response.json());
        this.items = response;
        this.isLoading = false;
      } catch (error) {
        this.fetchOnline();
        console.log("Backend Error");
        console.log(error);
        this.isLoading = false;
      }
    },

    async fetchOnline() {
      try {
        let response = await fetch(
          "https://api.instantwebtools.net/v1/airlines"
        )
          .then((response) => {
            return response.json();
          })
          .then((data) => {
            this.items = data;
            // console.log(data)
          });
        // this.isLoading = false;
      } catch (error) {
        // Problem with the timeout
        console.log("ultimate error");
        console.log(error);
        //Fake items
        this.items = [
          {
            id: 1,
            name: "Quatar Airways",
            country: "Quatar",
            logo: "https://upload.wikimedia.org/wikipedia/en/thumb/9/9b/Qatar_Airways_Logo.svg/300px-Qatar_Airways_Logo.svg.png",
            slogan: "Going Places Together",
            head_quaters: "Qatar Airways Towers, Doha, Qatar",
            website: "www.qatarairways.com",
            established: "1994",
          },
          {
            id: 2,
            name: "Singapore Airlines",
            country: "Singapore",
            logo: "https://upload.wikimedia.org/wikipedia/en/thumb/6/6b/Singapore_Airlines_Logo_2.svg/250px-Singapore_Airlines_Logo_2.svg.png",
            slogan: "A Great Way to Fly",
            head_quaters: "Airline House, 25 Airline Road, Singapore 819829",
            website: "www.singaporeair.com",
            established: "1947",
          },
          {
            id: 3,
            name: "Emirates",
            country: "United Arab Emirates",
            logo: "https://upload.wikimedia.org/wikipedia/commons/thumb/5/5a/Emirates_logo.svg/1024px-Emirates_logo.svg.png",
            slogan: "Fly Emirates. Hello Tomorrow.",
            head_quaters: "Dubai, United Arab Emirates",
            website: "www.emirates.com",
            established: "1985",
          },
          {
            id: 4,
            name: "British Airways",
            country: "United Kingdom",
            logo: "https://upload.wikimedia.org/wikipedia/commons/thumb/2/23/British_Airways_Logo.svg/1024px-British_Airways_Logo.svg.png",
            slogan: "To Fly. To Serve.",
            head_quaters: "Waterside, Harmondsworth, England",
            website: "www.ba.com",
            established: "1974",
          },
        ];
      }
    },
  },
};
</script>

<style scoped>
/* .read-the-docs {
  color: #888;
} */
</style>
