<template>
  <div>
    <darkmode></darkmode>
    <todos></todos>
  </div>
</template>

<script>
import Todos from './components/Todos'
import Darkmode from './components/Darkmode'

export default {
  components: {
    Todos,
    Darkmode
  }
}
</script>
