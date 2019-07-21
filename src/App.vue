<template>
<div id="app">

    
    <router-view />
</div>
</template>

<script>
export default {
  name: 'App'
}

// document.addEventListener('DOMContentLoaded', () => {
//   const html = document.querySelector('html')
//   let fontSize = window.innerWidth / 10
//   fontSize = fontSize > 50 ? 30 : fontSize
//   html.style.fontSize = fontSize + 'px'
// })
document.addEventListener('DOMContentLoaded', () => {
  const html = document.querySelector('html')
  let fontSize = window.innerWidth / 15
  fontSize = fontSize > 50 ? 50 : fontSize
  html.style.fontSize = fontSize + 'px'
})
</script>

<style>

</style>
