<template>
  <header class="bg-indigo-950">
    <nav
      class="mx-auto flex items-center justify-between max-w-7xl p lg:px-8"
      aria-label="Global"
    >
      <!-- Hamburger Menu (Shown on smaller screens) -->
      <div class="lg:hidden">
        <button
          id="menu-toggle"
          class="text-white text-2xl focus:outline-none"
          @click="isMenuOpen = true"
        >
          ☰
        </button>
      </div>

      <!-- Project Name -->
      <div class="flex-none px-4">
        <a href="/" class="text-white text-2xl font-bold"> PlanYourTravel </a>
      </div>

      <!-- Semi-transparent Overlay -->
      <div
        v-if="isMenuOpen"
        @click="isMenuOpen = false"
        class="fixed inset-0 bg-black/50 z-40 transition-opacity duration-300 ease-in-out"
      ></div>

      <!-- Navigation Links (Peek Menu for Smaller Screens) -->
      <div
        :class="[
          isMenuOpen ? 'translate-x-0' : '-translate-x-full',
          'lg:translate-x-0 lg:flex lg:static lg:flex-grow lg:mx-16 lg:items-center lg:gap-8',
        ]"
        class="fixed inset-y-0 left-0 z-50 w-64 h-screen bg-indigo-950 text-white text-lg px-6 py-4 transition-transform duration-300 ease-in-out lg:w-auto lg:h-auto lg:bg-transparent"
      >
        <!-- Close Button (Only visible on small screens) -->
        <div class="flex items-center justify-between mb-6 lg:hidden">
          <button
            class="text-white text-2xl focus:outline-none"
            @click="isMenuOpen = false"
          >
            <span class="material-icons">arrow_back</span>
          </button>
        </div>

        <a
          href="/holidayplan"
          class="block py-2 hover:text-indigo-300 hover:scale-105 transition duration-300 ease-in-out"
        >
          HolidayPlan
        </a>
        <a
          href="/flights"
          class="block py-2 hover:text-indigo-300 hover:scale-105 transition duration-300 ease-in-out"
        >
          Flight
        </a>
        <a
          href="/hotels"
          class="block py-2 hover:text-indigo-300 hover:scale-105 transition duration-300 ease-in-out"
        >
          Hotel
        </a>
        <a
          href="/promo"
          class="block py-2 hover:text-indigo-300 hover:scale-105 transition duration-300 ease-in-out"
        >
          Promo
        </a>
      </div>

      <!-- Login / Profile -->
      <div class="flex-none px-4">
        <a
          href="#"
          class="text-white text-lg hover:text-gray-300 hover:underline transition duration-300 ease-in-out"
        >
          Login
        </a>
      </div>
    </nav>
  </header>
</template>

<script>
export default {
  data() {
    return {
      isMenuOpen: false, // Controls the visibility of the navigation menu
    };
  },
};
</script>

<!-- Include Google Fonts Material Icons -->
<style>
@import url("https://fonts.googleapis.com/icon?family=Material+Icons");
</style>
