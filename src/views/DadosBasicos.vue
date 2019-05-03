<template>
  <b-container fluid>
    <h1>Dados Basicos</h1>
    <b-row>
      <b-col cols="6">
        <corretor />
      </b-col>
    </b-row>
  </b-container>
</template>

<script>

import Corretor from '../components/Corretor'

export default {
  name: 'dados-basicos',
  components: {
    Corretor
  }
}
</script>
