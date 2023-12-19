<script lang="ts" setup>
const { data: navigation } = await useAsyncData("navigation", () =>
  fetchContentNavigation()
);
const { data: files } = useLazyFetch("/api/search.json", {
  default: () => [],
  server: false,
});

const links = [
  {
    label: "Modern Frontend Frameworks",
    to: "/categories/modern-frontend-frameworks",
  },
  {
    label: "Responsive Web Design",
    to: "/categories/responsive-web-design",
  },
  {
    label: "Performance Optimization",
    to: "/categories/performance-optimization",
  },
];

const footerLinks = [
  {
    label: "Explore",
    children: [
      {
        label: "Home",
        to: "#",
      },
      {
        label: "Categories",
        to: "#",
      },
      {
        label: "About Us",
        to: "#",
      },
      {
        label: "Contact",
        to: "#",
      },
    ],
  },
  {
    label: "Popular Topics",
    children: [...links],
  },
  {
    label: "Connect",
    children: [
      {
        label: "Twitter",
        to: "https://twitter.com/frontendblogs",
        external: true,
      },
      {
        label: "GitHub",
        to: "https://github.com/frontendblogs",
        external: true,
      },
      {
        label: "LinkedIn",
        to: "https://linkedin.com/company/frontendblogs",
        external: true,
      },
      {
        label: "Newsletter",
        to: "/newsletter",
      },
    ],
  },
];

const { toggleDocsSearch } = useUIState();
const { metaSymbol } = useShortcuts();
</script>

<template>
  <UHeader :links="links">
    <template #logo>
      <span class="text-xl font-bold">
        Frontend <span class="text-blue-800">Blogs</span>
      </span>
    </template>

    <template #right>
      <UTooltip text="Search" :shortcuts="[metaSymbol, 'k']">
        <UButton
          icon="i-heroicons-magnifying-glass-solid"
          variant="ghost"
          color="gray"
          @click="toggleDocsSearch"
        >
          <span class="sr-only">Search</span>
        </UButton>
      </UTooltip>
      <FollowUs />
    </template>
  </UHeader>

  <UMain>
    <NuxtPage />
  </UMain>

  <UFooter class="mt-24">
    <template #top>
      <div class="grid grid-cols-1 xl:grid-cols-2 gap-12 xl:gap-0">
        <span class="text-xl font-bold">
          Frontend <span class="text-blue-800">Blogs</span>
        </span>

        <ol
          class="xl:place-self-end grid grid-cols-2 md:grid-cols-3 gap-8 xl:gap-20"
        >
          <li v-for="item in footerLinks" :key="item.label">
            <span class="font-semibold">
              {{ item.label }}
            </span>
            <ol class="mt-3 flex flex-col text-zinc-700">
              <li v-for="child in item.children" :key="child.label">
                <ULink
                  :to="child.to"
                  class="block py-1 hover:underline hover:underline-offset-2"
                >
                  {{ child.label }}
                </ULink>
              </li>
            </ol>
          </li>
        </ol>
      </div>
    </template>
    <template #left>
      <div class="flex flex-col md:flex-row items-center gap-4">
        <span class="text-sm text-stone-500">
          © 2023 Frontend Blogs. All rights reserved.
        </span>
        <div class="flex gap-2 text-xs text-stone-500">
          <ULink to="#"> Privacy Policy </ULink>
          <ULink to="#"> Terms of Service </ULink>
        </div>
      </div>
    </template>
    <template #right>
      <FollowUs />
    </template>
  </UFooter>

  <ClientOnly>
    <LazyUDocsSearch :files="files" :links="links" :navigation="navigation" />
  </ClientOnly>
</template>
