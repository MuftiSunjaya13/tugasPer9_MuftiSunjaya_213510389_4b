<template>
  <div v-for="abouts in about" :key="abouts.id" class="about">
    <router-link :to="{name: 'AboutDetails', params: { id: abouts.id }}">
      <h2>{{ abouts.tittle }}</h2>
    </router-link>
  </div>
</template>

<script>
export default {
  data() {
    return {
      about: [
        { tittle: 'Pelajar Di Universitas Islam Riau', id: 1, details: 'lorem' },
        { tittle: 'Contact Me', id: 2, details: 'lorem' },
        
      ]
    }
  }
}
</script>

<style>
.about h2{
  background: rgb(0,0,0,0.35);
  padding: 20px;
  border-radius: 10px;
  margin: 10px auto;
  max-width: 600px;
  cursor: pointer;
  color: #070707;
}
.about h2:hover {
  background: rgb(0,0,0,0.15);
}
.about a{
  text-decoration: none;
}
</style>