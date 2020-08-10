<template>
  <div>
    <navbar/>
    <nuxt />
  </div>
</template>

<script>
  import navbar from '@/components/partials/navbar'
  export default {
    components: {navbar},
  }
</script>