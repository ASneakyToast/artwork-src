<template>
  <div class="wrapper">


    <Modal v-show="isQuickviewOpen" @close="closeQuickview">
      <template v-slot:aside>
        default aside
      </template>
      <template v-slot:header>
        <h4>{{ selectedItem.title }}</h4>
      </template>
      <template v-slot:body>
        <p>{{ selectedItem }}</p>
      </template>
      <template v-slot:footer>
        <NuxtLink :to="{ name: 'artwork-slug', params: { slug: selectedItem.slug } }">{{ selectedItem.title }}</NuxtLink>
      </template>
    </Modal>


    <nav>
      <p>reuse nav component here, is it a toc?</p>
    </nav>


    <main class="main">
      
      <section>
        <p>Joel Lithgow's</p>
        <h1>Archive</h1>
        <p>This is where all of my shit lives! or at least all the documentation of everythinng I've made and shit people have given me ( along with the permission to display it )</p>
      </section>

      <section>
        <h3>All Articles</h3>
        <article v-for="artwork of artworks"
                 :key="artwork.id"
                 @click="openQuickview( artwork )">
          <h4>{{ artwork.title }}</h4>
          <p>{{ artwork.description }}</p>
        </article>
      </section>

    </main>


  </div>
</template>

<script>
export default ({
  async asyncData({ $content, params }) {
    try {
      const artworks = await $content({ deep: true })
        .where({ dir: `/artwork` })
        .only([ "id", "title", "description", "image", "path", "slug" ])
        .sortBy( "createdAt", "desc" )
        .fetch()
      return { artworks }
    } catch ( err ) {
      error({
        statusCode: 404,
        message: "Page could not be found",
      })
    }
  },
  data() {
    return {
      isQuickviewOpen: false,
      selectedItem: {},
    }
  },
  methods: {
    openQuickview: function( item ) {
      console.log( item );
      this.selectedItem = item;
      this.isQuickviewOpen = true;
    },
    closeQuickview: function() {
      this.isQuickviewOpen = false;
    },
  }
})
</script>

<style>
.wrapper {
  display: grid;
  grid-template-columns: 1fr 3fr;
  grid-gap: 64px;
}

.main {
  display: grid;
  grid-gap: 32px;
}
</style>
