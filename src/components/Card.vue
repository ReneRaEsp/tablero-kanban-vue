<template>
  <div class="card">
    <slot />
  </div>
</template>

<script>
export default {

}
</script>

<style lang="scss" scoped>
.card {
  align-self: center;
  text-align: center;
  background: rgb(84, 120, 210);
  color: white;
  width: 12rem;
  padding: .6rem 0 .6rem 0;
  border-radius: .4rem;
  font-size: 1.2rem;
  margin-top: 0.8rem;
  cursor: pointer;
  font-weight: 800;
}

</style>
