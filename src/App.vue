<template>
  <div id="app">
    <CheckboxInput
      id="1"
      name="name"
    />
    <span class="text">(no label)</span>
    <CheckboxInput
      id="2"
      name="name"
    >
      Enabled
    </CheckboxInput>
    <CheckboxInput
      id="4"
      name="name"
      :checked="true"
    >
      Checked
    </CheckboxInput>
    <CheckboxInput
      id="3"
      name="name"
      :disabled="true"
      :checked="true"
    >
      Disabled, Checked
    </CheckboxInput>
  </div>
</template>

<script>
import CheckboxInput from './components/CheckboxInput.vue';

export default {
  name: 'app',
  components: {
    CheckboxInput,
  },
};
</script>

<style lang="less">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  display: flex;
  justify-content: center;
  align-items: center;
}

.checkbox-input {
  margin: 20px;
}

.text {
  font-size: 18px;
}
</style>
