<template>
  <div class="container mt-4">
    <h1 class="text-center mb-4">Closest Asteroids</h1>

    <!-- Form to fetch asteroids by date range -->
    <div class="d-flex justify-content-center mb-4">
      <form
        @submit.prevent="fetchAsteroids"
        class="w-100"
        style="max-width: 600px"
      >
        <div class="form-row justify-content-center">
          <div class="form-group col-md-6">
            <label for="startDate">Start Date:</label>
            <input
              type="date"
              id="startDate"
              v-model="startDate"
              class="form-control"
              required
            />
          </div>
          <div class="form-group col-md-6">
            <label for="endDate">End Date:</label>
            <input
              type="date"
              id="endDate"
              v-model="endDate"
              class="form-control"
              required
            />
          </div>
        </div>
        <div class="d-flex justify-content-center">
          <button type="submit" class="btn btn-primary">
            Get Asteroids by Date Range
          </button>
        </div>
      </form>
    </div>

    <!-- Form to fetch asteroid by ID -->
    <h1 class="text-center mb-4">Asteroid by ID</h1>
    <div class="d-flex justify-content-center mb-4">
      <form
        @submit.prevent="fetchAsteroidById"
        class="w-100"
        style="max-width: 600px"
      >
        <div class="form-row justify-content-center">
          <div class="form-group col-md-10">
            <label for="asteroidId">Asteroid ID:</label>
            <input
              type="text"
              id="asteroidId"
              v-model="asteroidId"
              class="form-control"
              required
            />
          </div>
        </div>
        <div class="d-flex justify-content-center">
          <button type="submit" class="btn btn-primary">
            Get Asteroid by ID
          </button>
        </div>
      </form>
    </div>

    <!-- Form to fetch asteroid count by distance -->
    <h1 class="text-center mb-4">Count Asteroids by Distance</h1>
    <div class="d-flex justify-content-center mb-4">
      <form
        @submit.prevent="fetchAsteroidCountByDistance"
        class="w-100"
        style="max-width: 600px"
      >
        <div class="form-row justify-content-center">
          <div class="form-group col-md-10">
            <label for="distance">Distance (in kilometers):</label>
            <input
              type="number"
              id="distance"
              v-model="distance"
              class="form-control"
              required
            />
          </div>
        </div>
        <div class="d-flex justify-content-center">
          <button type="submit" class="btn btn-primary">
            Get Asteroid Count by Distance
          </button>
        </div>
      </form>
    </div>

    <!-- Form to store top 10 closest asteroids -->
    <h1 class="text-center mb-4">Store Top 10 Closest Asteroids</h1>
    <div class="d-flex justify-content-center mb-4">
      <form
        @submit.prevent="storeTop10Asteroids"
        class="w-100"
        style="max-width: 600px"
      >
        <div class="form-row justify-content-center">
          <div class="form-group col-md-6">
            <label for="startDateStore">Start Date:</label>
            <input
              type="date"
              id="startDateStore"
              v-model="startDateStore"
              class="form-control"
              required
            />
          </div>
          <div class="form-group col-md-6">
            <label for="endDateStore">End Date:</label>
            <input
              type="date"
              id="endDateStore"
              v-model="endDateStore"
              class="form-control"
              required
            />
          </div>
        </div>
        <div class="d-flex justify-content-center">
          <button type="submit" class="btn btn-primary">
            Store Top 10 Asteroids
          </button>
        </div>
      </form>
    </div>

    <!-- Loading spinner -->
    <div v-if="loading" class="text-center">
      <div class="spinner-border text-primary" role="status">
        <span class="sr-only">Loading...</span>
      </div>
    </div>

    <!-- Displaying asteroids by date range -->
    <div v-if="asteroids.length && !loading">
      <h2 class="text-center mb-4">Top 10 Closest Asteroids</h2>
      <div class="row">
        <div
          v-for="(asteroid, index) in asteroids"
          :key="asteroid.id"
          class="col-md-6 mb-4"
        >
          <div class="card">
            <div class="card-body">
              <h5 class="card-title">#{{ index + 1 }} - {{ asteroid.name }}</h5>
              <p class="card-text"><strong>ID:</strong> {{ asteroid.id }}</p>
              <p class="card-text">
                <strong>Absolute Magnitude:</strong>
                {{ asteroid.absoluteMagnitude }}
              </p>
              <p class="card-text">
                <strong>Estimated Diameter (Min):</strong>
                {{ asteroid.estimatedDiameterMin }} km
              </p>
              <p class="card-text">
                <strong>Estimated Diameter (Max):</strong>
                {{ asteroid.estimatedDiameterMax }} km
              </p>
              <p class="card-text">
                <strong>Miss Distance:</strong>
                {{ asteroid.missDistanceKilometers }} km
              </p>
              <p class="card-text">
                <strong>Potentially Hazardous:</strong>
                {{ asteroid.isPotentiallyHazardousAsteroid }}
              </p>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Displaying asteroid by ID -->
    <div v-if="asteroidById && !loading">
      <h2 class="text-center mb-4">Asteroid Details</h2>
      <div class="row justify-content-center">
        <div class="col-md-6 mb-4">
          <div class="card">
            <div class="card-body">
              <h5 class="card-title">{{ asteroidById.name }}</h5>
              <p class="card-text">
                <strong>ID:</strong> {{ asteroidById.id }}
              </p>
              <p class="card-text">
                <strong>Absolute Magnitude:</strong>
                {{ asteroidById.absoluteMagnitude }}
              </p>
              <p class="card-text">
                <strong>Estimated Diameter (Min):</strong>
                {{ asteroidById.estimatedDiameterMin }} km
              </p>
              <p class="card-text">
                <strong>Estimated Diameter (Max):</strong>
                {{ asteroidById.estimatedDiameterMax }} km
              </p>
              <p class="card-text">
                <strong>Miss Distance:</strong>
                {{ asteroidById.missDistanceKilometers }} km
              </p>
              <p class="card-text">
                <strong>Potentially Hazardous:</strong>
                {{ asteroidById.isPotentiallyHazardousAsteroid }}
              </p>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Displaying asteroid count by distance -->
    <div v-if="asteroidCount !== null && !loading">
      <h2 class="text-center mb-4">Asteroid Count by Distance</h2>
      <div class="row justify-content-center">
        <div class="col-md-6 mb-4">
          <div class="card">
            <div class="card-body">
              <h5 class="card-title">Asteroid Count: {{ asteroidCount }}</h5>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Displaying store result -->
    <div v-if="storeResult && !loading">
      <h2 class="text-center mb-4">Store Result</h2>
      <div class="alert alert-success" v-if="storeResult.status === 200">
        <h4>{{ storeResult.message }}</h4>
        <p>{{ storeResult.details }}</p>
      </div>
      <div class="alert alert-danger" v-else>
        <h4>{{ storeResult.message }}</h4>
        <p>{{ storeResult.details }}</p>
      </div>
    </div>

    <div v-if="error" class="alert alert-danger mt-4">
      <h4>Error: {{ error.status }}</h4>
      <p><strong>Message:</strong> {{ error.message }}</p>
      <p><strong>Details:</strong> {{ error.details }}</p>
    </div>

    <!-- Footer -->
    <footer class="text-center mt-4">
      <p>&copy; 2024 Asteroid Tracker. All rights reserved.</p>
    </footer>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      startDate: "",
      endDate: "",
      startDateStore: "",
      endDateStore: "",
      asteroidId: "",
      distance: null,
      asteroids: [],
      asteroidById: null,
      asteroidCount: null,
      storeResult: null,
      loading: false,
      error: null,
    };
  },
  methods: {
    async fetchAsteroids() {
      this.loading = true;
      this.error = null;
      this.asteroidById = null; // Clear asteroid by ID result
      this.asteroidCount = null; // Clear asteroid count result
      try {
        const response = await axios.get(
          "http://localhost:8080/asteroids/closest",
          {
            params: {
              startDate: this.startDate,
              endDate: this.endDate,
            },
          }
        );
        console.log("Response Data for Date Range:", response.data); // Debugging
        this.asteroids = response.data.asteroids;
      } catch (error) {
        if (error.response) {
          // Extracting error details from the response
          this.error = {
            status: error.response.data.status,
            message: error.response.data.message,
            details: error.response.data.details,
          };
        } else {
          this.error = {
            status: "Error",
            message: "Failed to fetch asteroids.",
          };
        }
        console.error(error);
      } finally {
        this.loading = false;
      }
    },
    async fetchAsteroidById() {
      this.loading = true;
      this.error = null;
      this.asteroids = []; // Clear asteroid by date range results
      this.asteroidCount = null; // Clear asteroid count result
      try {
        const response = await axios.get(
          `http://localhost:8080/asteroids/${this.asteroidId}`
        );
        console.log("Response Data for ID:", response.data); // Debugging
        this.asteroidById = response.data.asteroid[0];
      } catch (error) {
        if (error.response) {
          // Extracting error details from the response
          this.error = {
            status: error.response.data.status,
            message: error.response.data.message,
            details: error.response.data.details,
          };
        } else {
          this.error = {
            status: "Error",
            message: "Failed to fetch asteroid by ID.",
          };
        }
        console.error(error);
      } finally {
        this.loading = false;
      }
    },
    async fetchAsteroidCountByDistance() {
      this.loading = true;
      this.error = null;
      this.asteroids = []; // Clear asteroid by date range results
      this.asteroidById = null; // Clear asteroid by ID result
      try {
        const response = await axios.get(
          `http://localhost:8080/asteroids/count-by-distance`,
          {
            params: {
              distance: this.distance,
            },
          }
        );
        console.log("Response Data for Count by Distance:", response.data); // Debugging
        this.asteroidCount = response.data.count;
      } catch (error) {
        if (error.response) {
          // Extracting error details from the response
          this.error = {
            status: error.response.data.status,
            message: error.response.data.message,
            details: error.response.data.details,
          };
        } else {
          this.error = {
            status: "Error",
            message: "Failed to fetch asteroid count by distance.",
          };
        }
        console.error(error);
      } finally {
        this.loading = false;
      }
    },
    async storeTop10Asteroids() {
      this.loading = true;
      this.error = null;
      this.storeResult = null; // Clear previous store result
      try {
        const response = await axios.post(
          `http://localhost:8080/asteroids/store-top10`,
          null,
          {
            params: {
              startDate: this.startDateStore,
              endDate: this.endDateStore,
            },
          }
        );
        console.log("Store Top 10 Response:", response.data); // Debugging
        this.storeResult = response.data;
      } catch (error) {
        if (error.response) {
          // Extracting error details from the response
          this.storeResult = {
            status: error.response.data.status,
            message: error.response.data.message,
            details: error.response.data.details,
          };
        } else {
          this.storeResult = {
            status: 500,
            message: "Error storing top 10 asteroids.",
            details: "End date is maximum 7 days after start date.",
          };
        }
        console.error(error);
      } finally {
        this.loading = false;
      }
    },
  },
};
</script>

<style scoped>
/* Add any custom styles here */
</style>
