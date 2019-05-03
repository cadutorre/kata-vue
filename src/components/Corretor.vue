<template>
  <div>
    <b-input-group
      prepend="Username"
      class="mt-3"
    >
      <b-form-input></b-form-input>
      <b-input-group-append>
        <b-button variant="info">Button</b-button>
      </b-input-group-append>
    </b-input-group>
  </div>
</template>

<script>
export default {
  name: 'corretor',
  data () {
    return {
      name: ''
    }
  }
}
</script>

<style>
</style>
