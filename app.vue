<script setup lang="ts">
type answer = "" | "yes" | "no"
useState<answer>("answer", () => "")
</script>

<template>
  <div>
    <NuxtPage />
    <Sakura />
  </div>
</template>

<style>
body {
  background: #ffe9e4;
  font-family: "Manrope", sans-serif;
}
h1, h2, h3, h4, h5, h6 {
    font-family: "Pacifico", cursive;
    font-weight: 600;
}
</style>
