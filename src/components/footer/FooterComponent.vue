<template>
  <footer>
    <p>
      Design fait par <a href="https://robinchausson.com/">Robin Chausson</a>
    </p>
  </footer>
</template>

<script>

export default {
  name: "FooterComponent"
}

</script>

<style lang="scss">
@import "../../utils/computer/variables";
footer {
  position: relative;
  background-color: $primary-red;
  padding: .8rem 0;

  p {
    position: relative;
    text-align: center;
    font-size: $p-text;
    color: $primary-white;
    margin: 0;

    a {
      display: inline-block;
      position: relative;
      font-size: $p-text;
      font-weight: 600;
      text-decoration: none;
      color: inherit;

      &::before {
        position: absolute;
        content: '';
        top: 100%;
        left: 0;
        transform: translate(0, -100%);
        height: 2px;
        width: 0%;
        background-color: $primary-white;
        transition: .4s;
      }
      &:hover::before {
        width: 100%;
      }
    }
  }
}

</style>