<template>
  <div>
    <nav-bar navBarTitle="车来了精准实时公交" :isRetShow="false" />
  </div>
</template>

<script>
import NavBar from '@/components/navbar/NavBar'

export default {
  name: 'way',
  components: {
    'nav-bar': NavBar
  }
}
</script>