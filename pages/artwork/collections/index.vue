<template>
  <div class="layout-site">

    <section class="layout-block">
      <h5><NuxtLink to="/artwork">/ Artwork</NuxtLink></h5>
    </section>

    <div class="layout-page">
      <section class="layout-block">
        <h1>Collections</h1>
      </section>

      <hr></hr>

      <main class="layout-block grid-big">
        <article v-for= "collection of collections"
                 :key="collection.id">
          <NuxtLink :to="{ name: 'artwork-collections-slug', params: { slug: collection.slug } }"
                    class="layout-block collection">
            <img :srcset="require( `~/assets/artwork/${ collection.associatedArtwork.slug }/photo-1.jpg` ).srcSet"
                 class="square">
            <h3 class="item-center">{{ collection.title }}</h3>
          </NuxtLink>
        </article>
      </main>

    </div>
  </div>
</template>

<script>
export default {
  async asyncData({ $content, params, error }) {
    try {

      const collections = await $content( "collections" )
        .where({ published: true })
        .only([ "title", "description", "featured", "slug" ])
        .sortBy( "title", "asc" )
        .fetch()

      let collectionsArr = collections.map( collection => collection.slug );
      const artworks = await $content( "artwork" )
        .where({ collections: { $containsAny: collectionsArr } })
        .only([ "title", "slug", "collections" ])
        .fetch()

      collections.forEach( collection => {
        let collectionArtwork = artworks.filter( artwork => artwork.collections.includes( collection.slug ) );
        collection.associatedArtwork = collectionArtwork[ Math.floor( Math.random() * collectionArtwork.length ) ];
      });

      return { collections }

    } catch ( err ) {
      error({
        statusCode: 404,
        message: "Page could not be found",
      })
    }
  }
}
</script>
