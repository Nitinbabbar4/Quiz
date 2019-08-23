<template>
  <div>
    <div class="img">
      <img src="../assets/download.jpg">
    <div>
       <b-nav tabs>
      <b-nav-item disabled>Fancy Quiz App</b-nav-item>
      <b-nav-item disabled>Counter:{{numcorrect}}/{{numTotal}}</b-nav-item>
      <b-nav-item disabled>Total Question:{{10}}</b-nav-item>
    </b-nav>
  </div>
</template>

<script>
export default {
  props: ["numcorrect", "numTotal"]
};
</script>
