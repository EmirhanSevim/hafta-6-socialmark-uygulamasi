<template>
  <AppHeader />
  <div class="flex flex-row">
    <Sidebar @category-changed="updateBookmarkList" />
    <app-bookmark-list v-if="bookmarkList.length > 0" :items="bookmarkList" />
    <div v-else>Bookmark bulunmamaktadır..</div>
  </div>
</template>
<script>
import Sidebar from '@/components/Home/Sidebar';
export default {
  components: {
    Sidebar,
  },
  data() {
    return {
      bookmarkList: [],
    };
  },
  mounted() {
    this.$socket.on('NEW_BOOKMARK_ADDED', (bookmark) => {
      this.bookmarkList.push(bookmark);
    });
  },
  created() {
    this.fetchData();
  },
  methods: {
    //veriyi getir(fetchData)
    fetchData() {
      this.$appAxios.get('/bookmarks?_expand=category&_expand=user').then((bookmark_list_response) => {
        console.log('bookmark_list_response :>> ', bookmark_list_response);
        this.bookmarkList = bookmark_list_response?.data || [];
      });
    },
    updateBookmarkList(categoryId) {
      const url = categoryId ? `/bookmarks?_expand=category&_expand=user&categoryId=${categoryId}` : `/bookmarks?_expand=category&_expand=user`;
      this.$appAxios.get(url).then((bookmark_list_response) => {
        this.bookmarkList = bookmark_list_response?.data || [];
      });
    },
  },
};
</script>
