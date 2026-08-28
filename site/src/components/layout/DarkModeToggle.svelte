<!-- 
This file is part of Jupiterp. For terms of use, please see the file
called LICENSE at the top level of the Jupiterp source tree (online at
https://github.com/atcupps/Jupiterp/LICENSE).
Copyright (C) 2026 Andrew Cupps
-->
<script lang="ts">
  function toggleDarkMode() {
    const isDark: boolean = localStorage.theme === 'dark';
    if (isDark) {
      localStorage.setItem('theme', 'light');
    } else {
      localStorage.setItem('theme', 'dark');
    }

    document.documentElement.classList.toggle('dark');
  }
</script>

<button
  class="hover:bg-hover hover:text-orange dark:hover:text-light-orange group rounded-lg px-4 py-1 sm:p-1"
  title="Toggle Theme"
  onclick={toggleDarkMode}
>
  <!-- Single Combined Smooth Animated SVG -->
  <svg xmlns="http://w3.org" viewBox="0 0 24 24" class="h-6 w-6 fill-current text-current">
    <defs>
      <!-- The Mask creates the crescent moon shape by subtracting a circle -->
      <mask id="theme-toggle-mask">
        <!-- White area keeps the main center circle visible -->
        <circle cx="12" cy="12" r="12" fill="white" />
        <!-- Dark circle overlay: Radius reduced to 5 to make a smaller, more subtle "bite" -->
        <circle
          cx="20"
          cy="4"
          r="4"
          fill="black"
          class="group-dark:-translate-x-1.25 group-dark:translate-y-1.25 origin-center transition-all duration-500 ease-in-out"
        />
      </mask>
    </defs>

    <!-- Center Icon Core (Transforms between Full Sun and Crescent Moon) -->
    <circle
      cx="12"
      cy="12"
      r="5"
      mask="url(#theme-toggle-mask)"
      class="group-dark:scale-200 origin-center transition-all duration-500 ease-in-out"
    />

    <!-- Outer Rays Group (Bursts out when Sun is active, scales down to 0 in Moon mode) -->
    <g
      stroke="currentColor"
      stroke-width="2"
      stroke-linecap="round"
      stroke-linejoin="round"
      class="group-dark:scale-0 group-dark:opacity-0 origin-center transition-all duration-500 ease-in-out"
    >
      <line x1="12" y1="2" x2="12" y2="4" />
      <line x1="12" y1="20" x2="12" y2="22" />
      <line x1="2" y1="12" x2="4" y2="12" />
      <line x1="20" y1="12" x2="22" y2="12" />

      <line x1="5.11" y1="5.11" x2="6.53" y2="6.53" />
      <line x1="17.47" y1="17.47" x2="18.89" y2="18.89" />
      <line x1="5.11" y1="18.89" x2="6.53" y2="17.47" />
      <line x1="17.47" y1="6.53" x2="18.89" y2="5.11" />
    </g>
  </svg>
</button>
