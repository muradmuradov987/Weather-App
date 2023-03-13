<template>
  <div
    class="modal__container bg-black bg-opacity-50 position-absolute w-100 d-flex flex-column align-items-center"
  >
    <div class="modal__body rounded-2">
      <div class="mb-4">
        <h3>About:</h3>
        <p class="mb-4">
          The Local Weather allows you to track the current and future weather
          of cities of your choosing.
        </p>
        <h3>How it works:</h3>
        <ol class="list-decimal list-inside mb-4">
          <li>
            Search for your city by entering the name into the search bar.
          </li>
          <li>
            Select a city within the results, this will take you to the current
            weather for your selection.
          </li>
          <li>
            Track the city by clicking on the "+" icon in the top right. This
            will save the city to view at a later time on the home page.
          </li>
        </ol>

        <h3>Removing a city</h3>
        <p>
          If you no longer wish to track a city, simply select the city within
          the home page. At the bottom of the page, there will be am option to
          delete the city.
        </p>
      </div>
      <button class="btn text-white py-2 px-6 close">Close</button>
    </div>
  </div>
</template>

<script setup></script>

<style lang="css" scoped>
.modal__container {
  height: 100vh;
  top: 0;
  left: 0;
}
.modal__body {
  margin-top: 100px;
  width: 50%;
  z-index: 10;
  background: white;
  padding: 30px;
}
.close {
  background: #00668a;
}
</style>
