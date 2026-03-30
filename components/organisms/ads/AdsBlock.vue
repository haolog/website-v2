<template>
  <article class="mt-4" v-if="false">
    <AdsFallback v-if="$store.state.adBlocked" />
    <AdsCarbon v-else :key="$route.params.slug" />
  </article>
</template>
