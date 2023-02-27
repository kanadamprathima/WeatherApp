<template>
  <div class="container my-5">
    <h1 class="title text-center">Weather Forecast</h1>
    <form>
      <input
        type="text"
        placeholder="Enter location"
        auto-complete="on"
        class="form-control text-muted form-rounded p-3 shadow"
      />
    </form>
    <div class="card rounded my-4 shadow-lg back-card overflow:hidden">
      <!-- top of card starts here -->
      <div class="card-top text-center" style="margin-bottom: 15rem">
        <div class="city-name my-3">
          <p>INDIA</p>
          <span>....</span>
          <p class="">IND</p>
        </div>
      </div>
      <!-- card middle body -->
      <div class="card-body">
        <div class="card-mid">
          <div class="row">
            <div class="col-12 text-center temp">
              <span>20&deg;C</span>
              <p class="my-4">clouds everywhere</p>
            </div>
          </div>
          <div class="row">
            <div class="col d-flex justify-content-between px-5 mx-5">
              <p>25&deg;C</p>
              <p>
                18&deg;C
                <!-- <img src="./assets/logo.png" /> -->
              </p>
            </div>
          </div>
        </div>
        <!-- bottom card -->
        <div class="card-bottom px-5 py-4 row">
          <!-- starts here -->
          <div class="col text-center">
            <p>23&deg;C</p>
            <span>feels like</span>
          </div>
          <div class="col text-center">
            <p>55%</p>
            <span>humidity</span>
          </div>
        </div>
        <!-- card ends here -->
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {},
};
</script>

<style>
@import "./assets/styles.css";
</style>
