<template>
  <button>
    <slot></slot>
  </button>
</template>

<script lang="ts">
export default {

}
</script>

<style>
@define-mixin button-reset {
    border: none;
    outline: none;
}

button {
    @mixin button-reset;
    font-weight: 700;
    height: 64px;
    width: 100%;
    border-radius: 5px;
    cursor: pointer;
}
</style>