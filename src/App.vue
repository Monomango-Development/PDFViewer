<template>
  <div id="app">
    <pdf-viewer/>
  </div>
</template>

<script>
import PdfViewer from './components/pdf-viewer.vue'



export default {
  name: 'App',
  created() {
    document.body.style="overflow:hidden; margin:0;";
  },
  components: {
    PdfViewer
  }
}
</script>

<style>

</style>
