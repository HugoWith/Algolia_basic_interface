<template>
  <div class="banner">
    <div class="banner--wrapper">
      <h1>Inspiring <strong>talks </strong> from <strong>Ted</strong>!</h1>
      <p>Filters & enjoy the best talks</p>
    </div>
  </div>
</template>
<script>
export default {
  name: "Banner",
  data() {
    return {};
  },
};
</script>
<style lang="scss" scoped>
.banner {
  background: url("../Assets/Images/banner.svg");
  background-size: cover;
  background-repeat: no-repeat;
  height: 80vh;
}

.banner--wrapper {
  display: flex;
  justify-content: center;
  flex-direction: column;
  height: 100%;
  h1 {
    margin-top: 16rem;
    align-self: center;
    color: rgb(248, 247, 247);
    font-family: "Poppins", sans-serif;
    font-size: 32px;
  }
  p {
    align-self: center;
    color: rgb(248, 247, 247);
    font-family: "Poppins", sans-serif;
  }
}
</style>
