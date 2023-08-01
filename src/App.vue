<template>
  <router-view></router-view>
</template>

<script>
export default {
  mounted() {
    this.$socket.on('NEW_BOOKMARK_ADDED', () => {
      console.log('calisti...');
    });
  },
};
</script>
