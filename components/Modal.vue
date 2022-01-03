<template>
  <div class="quickview-wrapper" @click="close">

    <div class="quickview">
      <slot name="aside">
        Default Image
      </slot>
      <main>
        <slot name="header">
          Default Title
        </slot>
        <slot name="body">
          Default Body
        </slot>
        <slot name="footer">
          Default footer
        </slot>
      </main>
    </div>

  </div>
</template>

<script>
export default ({
  name: "Modal",
  methods: {
    close() {
      this.$emit( "close" );
    }
  }
})
</script>

<style>
.quickview-wrapper {
  position: fixed;
  top: 0;
  bottom: 0;
  right: 0;
  left: 0;
  background-color: rgba( 0,0,0, 0.3);
  display: flex;
  justify-content: center;
  align-items: center;
}

.quickview {
  display: grid;
  grid-template-columns: 1fr 3fr;
  grid-gap: 24px;
  padding: 24px;
}

.quickview > main {
  display: grid;
  grid-template-rows: auto 1fr auto;
  grid-gap: 16px;
}
</style>
