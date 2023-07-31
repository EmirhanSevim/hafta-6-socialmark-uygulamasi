<template>
  <AppHeader />
  <div class="flex flex-row">
    <Sidebar @category-changed="updateBookmarkList" />
    <app-bookmark-list v-if="bookmarkList.length > 0" :items="bookmarkList" />
    <div v-else>Bookmark bulunamadı..</div>
  </div>
</template>
<script>
import Sidebar from '@/components/Home/Sidebar.vue';
export default {
  components: {
    Sidebar,
  },
  data() {
    return {
      bookmarkList: [],
    };
  },
  created() {
    this.$appAxios.get('/bookmarks?_expand=category&_expand=user').then((bookmark_list_response) => {
      console.log('bookmark_list_response :>> ', bookmark_list_response);
      this.bookmarkList = bookmark_list_response?.data || [];
    });

    // Bookmark olarak eklediklerimizi çekmek için user_bookmarks üzerinden çekelim
    // this.$appAxios.get('/user_bookmarks/?_expand=bookmark&_expand=user').then((user_bookmark_response) => {
    //   // console.log('user_bookmark_response :>> ', user_bookmark_response);
    //   this.$store.commit('setBookmarks', user_bookmark_response?.data);
    // });

    // Like olarak eklediklerimizi çekmek için user_likes üzerinden çekelim
    // this.$appAxios.get('/user_likes/?_expand=bookmark&_expand=user').then((user_likes_response) => {
    //   this.$store.commit('setLikes', user_likes_response?.data);
    // });
  },
  methods: {
    updateBookmarkList(categoryId) {
      const url = categoryId ? `/bookmarks?_expand=category&_expand=user&categoryId=${categoryId}` : `/bookmarks?_expand=category&_expand=user`;
      this.$appAxios.get(url).then((bookmark_list_response) => {
        this.bookmarkList = bookmark_list_response?.data || [];
      });
    },
  },
};
</script>
