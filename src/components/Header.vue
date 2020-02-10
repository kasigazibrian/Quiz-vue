<template>
    <div>
       <b-nav tabs>
           <b-nav-item disabled>Fancy Quiz App</b-nav-item>
           <b-nav-item disabled>Counter: {{ numCorrect }}/{{ numTotal }}</b-nav-item>
       </b-nav>
    </div>
</template>

<script>
  export default {
    name: "Header",
    props: ['numTotal', 'numCorrect' ]
  }
</script>

<style scoped>

</style>
