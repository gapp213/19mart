<template>
  <!-- Header -->
  <div class="bg-red-600 text-center w-full">
    <h2 class="text-3xl md:text-4xl py-4 md:py-6 text-white font-semibold flex items-start justify-center">
      Türkiye Cumhuriyeti

      <a href="#!" target="_blank" class="ml-4">
        <svg xmlns="http://www.w3.org/2000/svg" width="24px" height="24px" viewBox="0 0 24 24"><path fill="currentColor" d="M12 2A10 10 0 0 0 2 12c0 4.42 2.87 8.17 6.84 9.5c.5.08.66-.23.66-.5v-1.69c-2.77.6-3.36-1.34-3.36-1.34c-.46-1.16-1.11-1.47-1.11-1.47c-.91-.62.07-.6.07-.6c1 .07 1.53 1.03 1.53 1.03c.87 1.52 2.34 1.07 2.91.83c.09-.65.35-1.09.63-1.34c-2.22-.25-4.55-1.11-4.55-4.92c0-1.11.38-2 1.03-2.71c-.1-.25-.45-1.29.1-2.64c0 0 .84-.27 2.75 1.02c.79-.22 1.65-.33 2.5-.33s1.71.11 2.5.33c1.91-1.29 2.75-1.02 2.75-1.02c.55 1.35.2 2.39.1 2.64c.65.71 1.03 1.6 1.03 2.71c0 3.82-2.34 4.66-4.57 4.91c.36.31.69.92.69 1.85V21c0 .27.16.59.67.5C19.14 20.16 22 16.42 22 12A10 10 0 0 0 12 2"/></svg>      </a>
    </h2>

    <!-- Links -->
    <div class="flex gap-3 md:gap-5 text-sm md:text-base text-white font-medium justify-center header-links">
      <NuxtLink class="pt-1.5 pb-3" to="/" >Boykot Listesi</NuxtLink>
      <NuxtLink class="pt-1.5 pb-3" to="/acik-cagri" >Açık Çağrı</NuxtLink>
      <NuxtLink class="pt-1.5 pb-3" to="/sosyal-kanit">Sosyal Kanıt</NuxtLink>
    </div>
  </div>

  <!-- Pages -->
  <NuxtPage />

  <!-- Footer -->
  <div class="flex justify-center p-4 md:p-6 mt-auto text-white text-sm md:text-base bg-[#333]">
    19 Mayıs 2025 Türk Milleti Adına
  </div>
</template>

<style lang="scss">
body {
  background: #f1f1f1;
}

#__nuxt {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

.router-link-exact-active {
  
}

.header-links{
  a {
    transition: .25s all;
    opacity: .75;
  
    &:hover{
      transform: scale(1.05);
      opacity: 1;
    }

    &.router-link-exact-active{
      opacity: 1;
      position: relative;
      &::after {
        content: '';
        position: absolute;
        bottom: 0;
        height: 3px;
        left: -5px;
        right: -5px;
        background: #fff;
        box-shadow: 0 1px 3px rgb(238 11 11 / 19%);
        border-radius: 4px;
      }
    }
  }
}
</style>
