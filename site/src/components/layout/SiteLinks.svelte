<!-- 
This file is part of Jupiterp. For terms of use, please see the file
called LICENSE at the top level of the Jupiterp source tree (online at
https://github.com/atcupps/Jupiterp/LICENSE).
Copyright (C) 2026 Andrew Cupps
-->
<script lang="ts">
  import { page } from '$app/state';
  import type { Pathname } from '$app/types';
  import NavBarLink from './NavBarLink.svelte';
  import DarkModeToggle from './DarkModeToggle.svelte';
  import { AngleDownOutline, GithubSolid, BugSolid } from 'flowbite-svelte-icons';

  interface NavLink {
    link: Pathname;
    text: string;
    children?: { link: Pathname; text: string }[];
  }

  const navLinks: NavLink[] = [
    { link: '/', text: 'Course Planner' },
    { link: '/generate', text: 'Schedule Generator' },
    {
      link: '/about',
      text: 'About',
      children: [
        { link: '/terms-of-use', text: 'Terms of Use' },
        { link: '/privacy-policy', text: 'Privacy Policy' },
        { link: '/changelog', text: 'Changelog' },
      ],
    },
  ];

  const currentPath: string = $derived(page.url.pathname);
</script>

<!-- [START] Nav Menu Toggle -->
<input type="checkbox" id="nav-menu-toggle" class="peer hidden" />

<label for="nav-menu-toggle" class="group relative -mr-4 flex cursor-pointer select-none items-center px-4 md:hidden">
  <svg xmlns="http://w3.org" width="1em" height="1em" viewBox="0 0 24 24" class="h-6 w-6">
    <g fill="none" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2">
      <!-- Top Line: Rotates and slides left by 2px on the X-axis -->
      <path
        d="M4 6h17"
        class="group-peer-checked:rotate-45 group-peer-checked:translate-x-0.5 origin-[4px_6px] transition-transform duration-300"
      />
      <!-- Middle Line: Fades out smoothly -->
      <path d="M4 12h15" class="group-peer-checked:opacity-0 transition-opacity duration-150 ease-in-out" />
      <!-- Bottom Line: Rotates and slides left by 2px on the X-axis -->
      <path
        d="M4 18h17"
        class="group-peer-checked:-rotate-45 group-peer-checked:translate-x-0.5 origin-[4px_18px] transition-transform duration-300"
      />
    </g>
  </svg>
  <div class="group-peer-checked:block fixed inset-0 top-12 hidden bg-black/50"></div>
</label>
<!-- [END] Nav Menu Toggle -->

<aside
  class="max-md:bg-bg-primary max-md:scrollbar-gutter-both custom-scrollbar flex py-2 text-lg max-md:fixed max-md:bottom-0 max-md:right-0 max-md:top-12 max-md:min-w-60 max-md:translate-x-full max-md:flex-col max-md:overflow-y-scroll max-md:border-l-2 max-md:px-4 max-md:transition-transform max-md:duration-300 max-md:peer-checked:translate-x-0 md:gap-2"
>
  <div class="flex gap-2 max-md:flex-col md:gap-5">
    {#each navLinks as item, i (i)}
      {#if item.children}
        <div class="nav-list-wrapper relative">
          <!-- [START] Nav List Toggle  -->
          <input type="checkbox" id="nav-list-{i}" class="peer hidden" />

          <div class="flex justify-between">
            <NavBarLink link={item.link} current={currentPath === item.link}>
              {item.text}
            </NavBarLink>
            <label
              for="nav-list-{i}"
              class="nav-list-toggle hover:text-orange dark:hover:text-light-orange float-right origin-center transition-transform"
              ><AngleDownOutline height="1.75rem" width="1.75rem" class="p-0.75" />
            </label>
          </div>

          <label for="nav-list-{i}" class="fixed inset-0 hidden bg-black/50 md:peer-checked:block"></label>
          <!-- [END] Nav List Toggle  -->

          <div
            style="transition: height 0.3s;"
            class="nav-list-contents md:-left-4.25 md:bg-bg-primary md:peer-checked:border-border md:mt-2.75 -mt-px flex h-auto flex-col overflow-clip peer-checked:h-0 max-md:border-y md:absolute md:h-0 md:rounded-lg md:border md:peer-checked:h-auto"
          >
            {#each item.children as child, j (j)}
              <NavBarLink link={child.link} current={currentPath === child.link} class="md:hover:bg-hover px-4 py-1">
                {child.text}
              </NavBarLink>
            {/each}
          </div>
        </div>
      {:else}
        <NavBarLink link={item.link} current={currentPath === item.link}>
          {item.text}
        </NavBarLink>
      {/if}
    {/each}
  </div>
  <div class="mt-auto max-md:pt-4">
    <div class="flex flex-row justify-between md:gap-1">
      <DarkModeToggle />
      <NavBarLink
        link={'/bugs' as Pathname}
        class="hover:bg-hover rounded-lg px-4 py-1 md:p-1"
        current={currentPath === '/bugs'}
        title="Report a Bug or Issue"
      >
        <BugSolid class="h-6 w-6" />
      </NavBarLink>
      <a
        href="https://github.com/Jupiterp/Jupiterp"
        target="_blank"
        rel="noopener noreferrer"
        class="hover:text-orange dark:hover:text-light-orange hover:bg-hover rounded-lg px-4 py-1 md:p-1"
        title="View GitHub Repository"
      >
        <GithubSolid class="h-6 w-6" />
      </a>
    </div>
    <p class="text-text-secondary my-2 text-sm md:hidden">Made with ❤ by the Jupiterp Team.</p>
  </div>
</aside>

<style>
  div.nav-list-wrapper {
    /* Move to block root: @supports rule is valid inside nesting but unnecessary here */
    interpolate-size: allow-keywords;

    /* Use a single variable to handle all toggle rotations */
    --toggle-deg: 180deg;

    > div > label.nav-list-toggle {
      transform: rotate(var(--toggle-deg));
      transition: transform 0.2s ease; /* Ensure smooth animation */
    }

    &:has(input:checked) {
      --toggle-deg: 0deg;
    }

    @media (min-width: 768px) {
      --toggle-deg: 0deg;

      &:has(input:checked) {
        --toggle-deg: 180deg;
      }

      &:hover {
        --toggle-deg: 180deg;

        > div.nav-list-contents {
          height: auto;
          /* Rely on Tailwind's core utility names if using standard configurations */
          box-shadow:
            0 10px 15px -3px rgb(0 0 0 / 0.1),
            0 4px 6px -4px rgb(0 0 0 / 0.1);
        }
      }
    }
  }
</style>
