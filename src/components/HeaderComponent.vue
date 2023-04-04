<template>
  <header>
    <div class="logo">
      <h1>calc</h1>
    </div>

    <div class="theme-toggle">
      <h3>
        THEME
      </h3>

      <div class="toggle-holder">
        <div class="theme-numbers">
          <span>1</span>
          <span>2</span>
          <span>3</span>
        </div>

        <div class="toggle-button" @click="$emit('toggle-theme')">
          <span class="toggle-selector"></span>
        </div>
      </div>
    </div>
  </header>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent( {
  name: "HeaderComponent",
  methods: {
    
  }
});
</script>

<style scoped>
.logo {
  h1 {
    font-size: 32px;
  }
}

header {
  display: flex;
  justify-content: space-between;
  align-items: center;

  .theme-toggle {
    display: flex;
    align-items: flex-end;
    h3 {
      font-size: 12px;
      margin-right: 26px;
    }

    .toggle-holder {
      width: 72px;
      display: flex;
      flex-direction: column;
    }

    .theme-numbers {
      display: flex;
      width: 100%;
      justify-content: space-around;
    }

    .toggle-button {
      height: 26px;
      border-radius: 13px;
      display: flex;
      align-items: center;
      padding: 5px;
      margin-top: 8px;
      cursor: pointer;

      .toggle-selector {
        display: block;
        border-radius: 100%;
        width: 16px;
        height: 16px;
      }
    }


  }
}



</style>