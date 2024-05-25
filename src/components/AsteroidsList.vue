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

    <!-- Modern loading spinner -->
    <div ref="loadingSpinner" v-if="loading" class="text-center">
      <div class="lds-spinner">
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <div></div>
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
      this.asteroidById = null;
      this.asteroidCount = null;
      this.scrollToLoading();
      try {
        const response = await axios.get(
          `${process.env.VUE_APP_API_BASE_URL}/asteroids/closest`,
          {
            params: {
              startDate: this.startDate,
              endDate: this.endDate,
            },
          }
        );
        this.asteroids = response.data.asteroids;
      } catch (error) {
        this.handleError(error);
      } finally {
        this.loading = false;
      }
    },
    async fetchAsteroidById() {
      this.loading = true;
      this.error = null;
      this.asteroids = [];
      this.asteroidCount = null;
      this.scrollToLoading();
      try {
        const response = await axios.get(
          `${process.env.VUE_APP_API_BASE_URL}/asteroids/${this.asteroidId}`
        );
        this.asteroidById = response.data.asteroid[0];
      } catch (error) {
        this.handleError(error);
      } finally {
        this.loading = false;
      }
    },
    async fetchAsteroidCountByDistance() {
      this.loading = true;
      this.error = null;
      this.asteroids = [];
      this.asteroidById = null;
      this.scrollToLoading();
      try {
        const response = await axios.get(
          `${process.env.VUE_APP_API_BASE_URL}/asteroids/count-by-distance`,
          {
            params: {
              distance: this.distance,
            },
          }
        );
        this.asteroidCount = response.data.count;
      } catch (error) {
        this.handleError(error);
      } finally {
        this.loading = false;
      }
    },
    async storeTop10Asteroids() {
      this.loading = true;
      this.error = null;
      this.storeResult = null;
      this.scrollToLoading();
      try {
        const response = await axios.post(
          `${process.env.VUE_APP_API_BASE_URL}/asteroids/store-top10`,
          null,
          {
            params: {
              startDate: this.startDateStore,
              endDate: this.endDateStore,
            },
          }
        );
        this.storeResult = response.data;
      } catch (error) {
        this.handleError(error);
      } finally {
        this.loading = false;
      }
    },
    scrollToLoading() {
      this.$nextTick(() => {
        if (this.$refs.loadingSpinner) {
          this.$refs.loadingSpinner.scrollIntoView({ behavior: "smooth" });
        }
      });
    },
    handleError(error) {
      if (error.response) {
        this.error = {
          status: error.response.data.status,
          message: error.response.data.message,
          details: error.response.data.details,
        };
      } else {
        this.error = {
          status: "Error",
          message: "An error occurred while fetching data.",
        };
      }
    },
  },
  mounted() {
    this.$nextTick(() => {
      if (this.$refs.loadingSpinner) {
        this.$refs.loadingSpinner.scrollIntoView({ behavior: "smooth" });
      }
    });
  },
};
</script>

<style scoped>
/* Modern spinner styles */
.lds-spinner {
  display: inline-block;
  position: relative;
  width: 80px;
  height: 80px;
}

.lds-spinner div {
  transform-origin: 40px 40px;
  animation: lds-spinner 1.2s linear infinite;
}

.lds-spinner div:after {
  content: " ";
  display: block;
  position: absolute;
  top: 3px;
  left: 37px;
  width: 6px;
  height: 18px;
  border-radius: 20%;
  background: #007bff;
}

.lds-spinner div:nth-child(1) {
  transform: rotate(0deg);
  animation-delay: -1.1s;
}

.lds-spinner div:nth-child(2) {
  transform: rotate(30deg);
  animation-delay: -1s;
}

.lds-spinner div:nth-child(3) {
  transform: rotate(60deg);
  animation-delay: -0.9s;
}

.lds-spinner div:nth-child(4) {
  transform: rotate(90deg);
  animation-delay: -0.8s;
}

.lds-spinner div:nth-child(5) {
  transform: rotate(120deg);
  animation-delay: -0.7s;
}

.lds-spinner div:nth-child(6) {
  transform: rotate(150deg);
  animation-delay: -0.6s;
}

.lds-spinner div:nth-child(7) {
  transform: rotate(180deg);
  animation-delay: -0.5s;
}

.lds-spinner div:nth-child(8) {
  transform: rotate(210deg);
  animation-delay: -0.4s;
}

.lds-spinner div:nth-child(9) {
  transform: rotate(240deg);
  animation-delay: -0.3s;
}

.lds-spinner div:nth-child(10) {
  transform: rotate(270deg);
  animation-delay: -0.2s;
}

.lds-spinner div:nth-child(11) {
  transform: rotate(300deg);
  animation-delay: -0.1s;
}

.lds-spinner div:nth-child(12) {
  transform: rotate(330deg);
  animation-delay: 0s;
}

@keyframes lds-spinner {
  0% {
    opacity: 1;
  }
  100% {
    opacity: 0;
  }
}
</style>
