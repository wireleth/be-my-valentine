<script setup>
const name = useRuntimeConfig().public.INVITEE_NAME;
</script>
<template>
  <title>Приглашение</title>
  <div class="">
    <div class="px-4">
      <h1 class="text-center" style="margin-top: 50px;">{{ name }}!</h1>
      <ClientOnly>
        <Countdown />
      </ClientOnly>
      <p class="pt-4 text-center">Я сделал этот сайт специально для тебя! 🐱</p>
      <p class="invitation text-center">
        Хочу пригласить тебя вместе отметить этот день. 
      </p>
    </div>
    <MainQuestion />
  </div>
</template>
<style>
body {
  transition: all 1s ease;
}
</style>
