<template>
  <header>
      <div class="container">
        <div class="row">
          <div class="col-12">
            <input v-model="search" type="text" placeholder="search" id="search">
            <button @click="$emit('start-search', search)">
              search
            </button>
          </div>
        </div>
      </div>
  </header>
</template>

<script>
export default {
  
    data() {
      return {
        search: ""
      }
    },
}
</script>

<style>

</style>