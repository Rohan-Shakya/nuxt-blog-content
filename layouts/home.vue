<script lang="ts" setup>
const { data: topArticles } = await useAsyncData(
  "content:home:modern-frontend-frameworks",
  () =>
    queryContent("/articles/")
      .where({
        top: true,
      })
      .only([
        "_path",
        "title",
        "categories",
        "description",
        "publishedAt",
        "image",
        "authors",
      ])
      .sort({ publishedAt: -1 })
      .limit(4)
      .find()
);
const { data: latestArticles } = await useAsyncData(
  "content:home:latest-articles",
  () =>
    queryContent("/articles/")
      .where({
        top: { $ne: true },
        _partial: false,
      })
      .sort({ publishedAt: -1 })
      .only([
        "_path",
        "title",
        "categories",
        "description",
        "publishedAt",
        "image",
        "authors",
      ])
      .limit(4)
      .find()
);
const { data: modernFrameWorks } = await useAsyncData(
  "content:home:modern-frontend-frameworks",
  () =>
    queryContent("/articles/")
      .where({
        categories: { $contains: "modern-frontend-frameworks" },
      })
      .only([
        "_path",
        "title",
        "categories",
        "description",
        "publishedAt",
        "image",
        "authors",
      ])
      .sort({ publishedAt: -1 })
      .limit(3)
      .find()
);
const { data: performanceOptimizations } = await useAsyncData(
  "content:home:performance-optimization",
  () =>
    queryContent("/articles/")
      .where({ categories: { $contains: "performance-optimization" } })
      .only([
        "_path",
        "title",
        "categories",
        "description",
        "publishedAt",
        "image",
        "authors",
      ])
      .sort({ publishedAt: -1 })
      .limit(3)
      .find()
);

const firstTopArticle = computed(() => topArticles.value[0]);
const otherTopArticles = computed(() => topArticles.value.slice(1));
</script>

<template>
  <ULandingSection>
    <div class="grid grid-cols-1 lg:grid-cols-2 gap-16">
      <UCard
        :ui="{
          base: 'relative group',
          body: { base: 'space-y-6', padding: '' },
        }"
      >
        <header>
          <div v-if="firstTopArticle.image" class="mb-2 overflow-hidden">
            <img
              :src="firstTopArticle.image.src"
              :alt="firstTopArticle.image.alt"
              class="w-full h-80 lg:h-48 object-cover transform group-hover:scale-[101%] transition-transform ease-in duration-200"
            />
          </div>

          <h3 class="text-4xl font-bold">
            <NuxtLink :to="firstTopArticle._path">
              <span class="absolute inset-0 z-10" />
              <span>
                {{ firstTopArticle.title }}
              </span>
            </NuxtLink>
          </h3>
        </header>

        <p class="text-stone-500">
          {{ firstTopArticle.description }}
        </p>

        <footer>
          <dl
            v-if="firstTopArticle.date || firstTopArticle.authors"
            class="flex justify-between items-center text-sm"
          >
            <template v-if="firstTopArticle.authors">
              <dt class="sr-only">Author</dt>
              <dd>
                <ArticleCardAuthors :authors="firstTopArticle.authors" />
              </dd>
            </template>
            <template v-if="firstTopArticle.publishedAt">
              <dt class="sr-only">Published at</dt>
              <dd>
                <ArticleCardDate :date="firstTopArticle.publishedAt" />
              </dd>
            </template>
          </dl>
        </footer>
      </UCard>
      <div class="flex flex-col gap-8">
        <UCard
          v-for="article in otherTopArticles"
          :key="article._path"
          :ui="{ base: 'relative group', body: { padding: '' } }"
        >
          <template #header>
            <header class="grid grid-cols-5 gap-3 items-center">
              <div
                v-if="article.image"
                class="overflow-hidden rounded-l-md col-span-2"
              >
                <img
                  :src="article.image.src"
                  :alt="article.image.alt"
                  class="aspect-[16/9] object-cover transform group-hover:scale-[101%] transition-transform ease-in duration-200"
                />
              </div>

              <div class="col-span-3">
                <h3 class="text-lg font-semibold">
                  <NuxtLink :to="article._path">
                    <span class="absolute inset-0 z-10" />
                    <span>
                      {{ article.title }}
                    </span>
                  </NuxtLink>
                </h3>

                <p class="text-stone-500 text-sm">
                  {{ article.description }}
                </p>
              </div>
            </header>
          </template>
        </UCard>
      </div>
    </div>
  </ULandingSection>
  <ULandingSection title="Latest blogs">
    <div
      class="bg-stone-100 p-4 xl:-m-12 lg:p-12 rounded-lg grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-8"
    >
      <ArticleCard
        v-for="article in latestArticles"
        :key="article._path"
        :to="article._path"
        :title="article.title"
        :description="article.description"
        :date="article.publishedAt"
        :image="article.image"
        :authors="article.authors"
        class="bg-transparent"
      />
    </div>
  </ULandingSection>
  <ULandingSection
    title="Responsive Strategies"
    description="Discover the art of creating adaptable and user-centric web experiences with responsive design techniques. Uncover strategies for fluid layouts, media optimization, and mobile-first approaches to ensure your website shines across all devices."
    :links="[
      {
        label: 'Responsive Web Design',
        color: 'gray',
        trailingIcon: 'i-heroicons-arrow-right',
        size: 'lg',
        to: '/categories/responsive-web-design',
      },
    ]"
    align="left"
  >
    <div class="flex flex-col gap-4">
      <UCard
        v-for="article in modernFrameWorks"
        :key="article._path"
        :ui="{ base: 'relative group', body: { padding: '' } }"
      >
        <template #header>
          <header class="grid grid-cols-5 gap-3 items-center">
            <div
              v-if="article.image"
              class="overflow-hidden rounded-l-md col-span-2"
            >
              <img
                :src="article.image.src"
                :alt="article.image.alt"
                class="aspect-[16/9] object-cover transform group-hover:scale-[101%] transition-transform ease-in duration-200"
              />
            </div>

            <div class="col-span-3">
              <h3 class="text-lg font-semibold">
                <NuxtLink :to="article._path">
                  <span class="absolute inset-0 z-10" />
                  <span>
                    {{ article.title }}
                  </span>
                </NuxtLink>
              </h3>

              <p class="text-stone-500 text-sm">
                {{ article.description }}
              </p>
            </div>
          </header>
        </template>
      </UCard>
    </div>
  </ULandingSection>
  <ULandingSection
    title="Maximizing Speed"
    align="right"
    description="Explore cutting-edge strategies and advanced techniques to turbocharge website speed and performance. Dive into JavaScript optimizations, frontend rendering enhancements, and resource management to elevate user experiences."
    :links="[
      {
        label: 'Performance Optimization',
        color: 'gray',
        trailingIcon: 'i-heroicons-arrow-right',
        size: 'lg',
        to: '/categories/performance-optimization',
      },
    ]"
  >
    <ArticleCard
      v-for="article in performanceOptimizations"
      :key="article._path"
      :to="article._path"
      :title="article.title"
      :description="article.description"
      :date="article.publishedAt"
      :image="article.image"
      :authors="article.authors"
      class="place-self-start"
    />
  </ULandingSection>
</template>
