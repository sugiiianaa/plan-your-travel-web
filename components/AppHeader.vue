<template>
  <header class="fixed w-full z-50 bg-neutral-100 shadow-md">
    <nav
      class="mx-auto flex items-center justify-between max-w-7xl px-4 py-2 lg:px-8"
      aria-label="Global"
    >
      <!-- Hamburger Menu (Shown on smaller screens) -->
      <div class="lg:hidden">
        <button
          id="menu-toggle"
          class="text-gray-900 text-2xl p-2 rounded bg-neutral-100 shadow-md focus:outline-none"
          @click="isMenuOpen = true"
        >
          <span class="material-icons">menu</span>
        </button>
      </div>

      <!-- Project Name -->
      <div class="flex-none">
        <a
          href="/"
          class="font-bold text-lg sm:text-xl md:text-2xl text-gray-900"
        >
          PlanYourTravel
        </a>
      </div>

      <!-- Overlay for when menu is open (on smaller screens) -->
      <div
        v-if="isMenuOpen"
        @click="isMenuOpen = false"
        class="fixed inset-0 bg-black/40 z-40 transition-opacity duration-300 ease-in-out lg:hidden"
      ></div>

      <!-- Navigation Links -->
      <div
        :class="[
          isMenuOpen ? 'translate-x-0' : '-translate-x-full',
          'lg:translate-x-0 lg:flex lg:static lg:flex-grow lg:mx-16 lg:items-center lg:gap-8',
        ]"
        class="fixed inset-y-0 left-0 z-50 w-64 h-screen bg-neutral-100 text-gray-900 text-lg px-6 py-4 transition-transform duration-300 ease-in-out lg:w-auto lg:h-auto lg:bg-neutral-100 lg:shadow-none"
      >
        <!-- Close Button (Only visible on small screens) -->
        <div class="flex items-center justify-between mb-6 lg:hidden">
          <button
            class="text-gray-900 text-2xl p-2 rounded bg-neutral-100 shadow-md focus:outline-none"
            @click="isMenuOpen = false"
          >
            <span class="material-icons">arrow_back</span>
          </button>
        </div>

        <!-- Menu Items -->
        <a
          href="/holidayplan"
          class="block py-2 hover:text-blue-600 hover:scale-105 transition duration-300 ease-in-out"
        >
          HolidayPlan
        </a>
        <a
          href="/flights"
          class="block py-2 hover:text-blue-600 hover:scale-105 transition duration-300 ease-in-out"
        >
          Flight
        </a>
        <a
          href="/hotels"
          class="block py-2 hover:text-blue-600 hover:scale-105 transition duration-300 ease-in-out"
        >
          Hotel
        </a>
        <a
          href="/promo"
          class="block py-2 hover:text-blue-600 hover:scale-105 transition duration-300 ease-in-out"
        >
          Promo
        </a>
      </div>

      <!-- Login / Profile -->
      <div class="flex-none">
        <a
          href="#"
          class="text-gray-900 text-base sm:text-lg hover:text-blue-600 hover:underline transition duration-300 ease-in-out"
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
      isMenuOpen: false, // Controls the visibility of the nav menu
    };
  },
};
</script>

<!-- Include Google Fonts Material Icons -->
<style>
@import url("https://fonts.googleapis.com/icon?family=Material+Icons");
</style>
