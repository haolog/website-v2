<template>
  <div class="group flex items-center delay-200 h-10 bg-primary text-black">
    <p class="sm:top-1.5 w-full pl-4 sm:pl-0 sm:text-center text-sm font-medium">
      {{$i18n.t('other.appBanner.title1')}} <a href="https://nuxt.com" class="font-bold hover:underline">{{$i18n.t('other.appBanner.title2')}}</a>，
      {{$i18n.t('other.appBanner.title3')}} <a href="https://www.herodevs.com/support/nuxt-nes?utm_source=nuxtjs&utm_medium=affiliate&utm_campaign=nuxt2eol&utm_content=link" class="font-bold hover:underline" target="_blank" rel="noopener">{{$i18n.t('other.appBanner.title4')}}</a>{{$i18n.t('other.appBanner.title5')}}
    </p>
  </div>
</template>
