<template>
  <div class="pt-16">
    <div class="slideshow mx-auto">
      <div class="image-container">
        <img
          :src="heroImages[currentIndex]"
          alt="Slideshow Image"
          class="image"
        />
        <div class="dark-overlay"></div>
      </div>
      <div class="text-overlay">
        <h2>Enjoy Your Holiday!</h2>
        <p>Discover new places and adventures.</p>
      </div>
      <div class="loading-marker">
        <div
          v-for="(image, index) in heroImages"
          :key="index"
          class="marker"
          :class="{ active: index === currentIndex }"
        >
          <div class="progress"></div>
        </div>
      </div>
    </div>

    <!-- MAIN SECTION -->
    <section
      class="flex flex-col items-center justify-center gap-8 p-8 md:flex-row md:justify-between mx-6 md:mx-24 mt-10"
    >
      <!-- LEFT COLUMN: TEXT + BUTTON -->
      <div class="flex flex-col items-start gap-4 max-w-lg">
        <h2 class="text-3xl font-bold text-gray-800">
          Choose from Our Best Travel Plans
        </h2>
        <p class="text-gray-600 leading-relaxed">
          Want to do your own research? No problem! We’ll handle the booking so
          you don’t have to. But, why sweat the details when we can take care of
          everything for you?
        </p>
        <button
          class="bg-blue-600 text-white font-medium py-2 px-6 rounded-md shadow hover:bg-blue-900 focus:outline-none focus:ring-2 focus:ring-blue-400 focus:ring-opacity-75"
        >
          Pick One
        </button>
      </div>

      <!-- RIGHT COLUMN: 3-IMAGE CAROUSEL -->
      <div
        class="relative w-full max-w-2xl h-64 mx-auto overflow-hidden"
        @mousemove="onDrag"
        @touchmove="onDrag"
      >
        <div
          v-for="(image, i) in visibleTravelSlides"
          :key="image"
          class="travel-slide"
          :style="getSlideStyle(i)"
          @mousedown="startDrag($event)"
          @mouseup="endDrag($event)"
          @mouseleave="endDrag($event)"
          @touchstart="startDrag($event)"
          @touchend="endDrag($event)"
        >
          <img :src="image" alt="Planned Travel" />
        </div>
      </div>
    </section>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "IndexPage",
  data() {
    return {
      /* HERO SLIDESHOW */
      heroImages: [
        "https://images.unsplash.com/photo-1542332213-9b5a5a3fad35?q=80&w=2070&auto=format&fit=crop",
        "https://images.unsplash.com/photo-1553968827-4a768f5bb872?q=80&w=2070&auto=format&fit=crop",
        "https://images.unsplash.com/photo-1520312501384-dbdb83a1cb11?q=80&w=1975&auto=format&fit=crop",
      ],
      currentIndex: 0,
      interval: null as ReturnType<typeof setInterval> | null,

      /* RIGHT COLUMN CAROUSEL */
      plannedTravelCarousel: [
        "https://images.unsplash.com/photo-1558870832-c8db4b5b47d1?w=600&auto=format&fit=crop&q=60",
        "https://images.unsplash.com/photo-1506377585622-bedcbb027afc?q=80&w=2070&auto=format&fit=crop",
        "https://images.unsplash.com/photo-1569271726612-dabfbb0364d0?w=600&auto=format&fit=crop&q=60",
      ],
      travelCurrentIndex: 0,

      // For drag/swipe interactions
      isDragging: false,
      startX: 0,
      dragOffset: 0, // how far user has dragged in pixels
    };
  },
  computed: {
    visibleTravelSlides(): string[] {
      const len = this.plannedTravelCarousel.length;
      const leftIdx = (this.travelCurrentIndex - 1 + len) % len;
      const centerIdx = this.travelCurrentIndex;
      const rightIdx = (this.travelCurrentIndex + 1) % len;

      return [
        this.plannedTravelCarousel[leftIdx],
        this.plannedTravelCarousel[centerIdx],
        this.plannedTravelCarousel[rightIdx],
      ];
    },
  },
  mounted() {
    this.startSlideshow();
  },
  beforeUnmount() {
    if (this.interval) {
      clearInterval(this.interval);
    }
  },
  methods: {
    /* HERO SLIDESHOW - auto-rotation */
    startSlideshow() {
      this.interval = setInterval(() => {
        this.currentIndex = (this.currentIndex + 1) % this.heroImages.length;
      }, 3000);
    },

    /* Next/Prev for the Travel Carousel */
    nextTravelSlide() {
      this.travelCurrentIndex =
        (this.travelCurrentIndex + 1) % this.plannedTravelCarousel.length;
    },
    prevTravelSlide() {
      this.travelCurrentIndex =
        (this.travelCurrentIndex - 1 + this.plannedTravelCarousel.length) %
        this.plannedTravelCarousel.length;
    },

    /* Drag Handlers */
    startDrag(e: MouseEvent | TouchEvent) {
      this.isDragging = true;
      this.startX = "touches" in e ? e.touches[0].clientX : e.clientX;
      this.dragOffset = 0;
    },
    onDrag(e: MouseEvent | TouchEvent) {
      if (!this.isDragging) return;
      const currentX = "touches" in e ? e.touches[0].clientX : e.clientX;
      let offset = currentX - this.startX;

      if (offset > 200) offset = 200;
      if (offset < -200) offset = -200;

      this.dragOffset = offset;
    },
    endDrag(e: MouseEvent | TouchEvent) {
      if (!this.isDragging) return;
      this.isDragging = false;

      const endX =
        "changedTouches" in e ? e.changedTouches[0].clientX : e.clientX;
      const diff = endX - this.startX;

      // If the user swiped more than ~50px, we go to next/prev
      if (Math.abs(diff) > 50) {
        if (diff < 0) this.nextTravelSlide(); // swiped left => next
        else this.prevTravelSlide(); // swiped right => prev
      }

      // Reset offset so slides snap back
      this.dragOffset = 0;
    },

    /**
     * Combines each slide's "base" position (left/middle/right)
     * with the user’s real-time drag offset for a smoother experience.
     */
    getSlideStyle(i: number) {
      let leftBase = 0; // "left" position in px (relative to container)
      let scaleBase = 1; // scale factor
      let blurBase = 0; // blur px
      let opacityBase = 0.6; // default side slides are dimmer

      if (i === 0) {
        // Left Slide (partially visible on left)
        leftBase = 20; // place near 20% of container
        scaleBase = 0.85;
        blurBase = 2;
      } else if (i === 1) {
        // Center Slide
        leftBase = 50; // place near the center (50%)
        scaleBase = 1.05;
        blurBase = 0;
        opacityBase = 1;
      } else if (i === 2) {
        // Right Slide
        leftBase = 80; // place near 80% of container
        scaleBase = 0.85;
        blurBase = 2;
      }

      // Combine baseX with user drag offset
      // We'll interpret leftBase as a percentage, then add drag in px.
      // A simpler approach is to just do "left: x% + dragOffset px".
      // We'll do it in transform with translate.
      const finalX = `calc(${leftBase}% + ${this.dragOffset}px)`;

      return {
        position: "absolute",
        top: "50%",
        transform: `translate(-50%, -50%) translateX(0)`,
        left: finalX,
        scale: scaleBase,
        filter: blurBase ? `blur(${blurBase}px)` : "none",
        opacity: opacityBase,
        zIndex: i === 1 ? 2 : 1,
        transition: "transform 0.4s ease, opacity 0.4s ease, filter 0.4s ease",
      };
    },
  },
});
</script>

<style scoped>
.slideshow {
  position: relative;
  width: 100%;
  max-width: 1200px;
  height: 400px;
  overflow: hidden;
  margin: 0 auto;
}
.image-container {
  width: 100%;
  height: 100%;
  position: relative;
}
.image {
  width: 100%;
  height: 100%;
  object-fit: cover;
}
.dark-overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  z-index: 1;
}
.text-overlay {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  color: #fff;
  text-align: center;
  z-index: 2;
}
.text-overlay h2 {
  font-size: 2rem;
  margin: 0 0 0.25rem;
}
.text-overlay p {
  font-size: 1.2rem;
  margin: 0;
}
.loading-marker {
  position: absolute;
  bottom: 20px;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  gap: 10px;
  z-index: 3;
}
.marker {
  width: 15px;
  height: 15px;
  border: 2px solid #fff;
  border-radius: 50%;
  position: relative;
  overflow: hidden;
}
.marker .progress {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: #fff;
  transform: scaleX(0);
  transform-origin: left;
  transition: transform 3s linear;
}
.marker.active .progress {
  transform: scaleX(1);
}

/* --- RIGHT COLUMN CAROUSEL --- */
.travel-slide {
  width: 200px;
  height: 100%;
  cursor: grab;
  display: flex;
  justify-content: center;
  align-items: center;
}
.travel-slide img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  border-radius: 0.5rem;
  box-shadow: 0 3px 6px rgba(0, 0, 0, 0.15);
}
</style>
