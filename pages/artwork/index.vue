<template>
  <div class="layout-site">

    <section class="layout-block">
      <h5><NuxtLink to="/">/ Home</NuxtLink></h5>
    </section>

    <div class="layout-page">

      <section class="layout-block">
        <h1>Artwork</h1>
        <p>Here you will find all my artwork that not only made the cut but I still find relevant today.</p>
      </section>

      <hr></hr>

      <section class="layout-block">
        <h3>Collections</h3>
        <ul>
          <li>
            <NuxtLink to="portfolio">
              Current Portfolio
            </NuxtLink>
          </li>
          <li v-for="collection of collections"
              :key="collection.id">
            <NuxtLink :to="{ name: 'artwork-collections-slug', params: { slug: collection } }">
              {{ collection }}
            </NuxtLink>
          </li>
        </ul>
      </section>

      <section class="layout-block">
        <h3>All Artwork</h3>
        <ul class="listing-list">
          <li v-for="artwork of artworks"
              :key="artwork.id">
            <NuxtLink :to="{ name: 'artwork-slug', params: { slug: artwork.slug } }">
              {{ artwork.title }}
            </NuxtLink>
          </li>
        </ul>
      </section>

    </div>
  </div>
</template>

<!-- Date templating
          <li v-for="artwork of artworks"
              :key="artwork.id">
            {{ artwork.date | formatDate }} | 
            <NuxtLink :to="{ name: 'artwork-slug', params: { slug: artwork.slug } }">
              {{ artwork.title }}
            </NuxtLink>
          </li>
-->

<script>
export default{
  async asyncData({ $content, params }) {
    try {

      const artworks = await $content({ deep: true })
        .where({ dir: `/artwork` })
        .only([ "title", "slug", "collections" ])
        .sortBy( "title", "asc" )
        .fetch()

      let all_collections = [];
      artworks.forEach( artwork => {
        if ( Array.isArray( artwork.collections ) ) {
          artwork.collections.forEach( collection => {
            all_collections.push( collection )
          })
        } else if ( artwork.collections instanceof String ) {
          all_collections.push( artwork.collections )
        } else {
          console.log( "error: something wrong with artwork collection" )
        }
      })

      let collections = [ ...new Set( all_collections.flat( 1 ) ) ].sort();
      console.log( collections )

      return { artworks, collections }
    
    } catch ( err ) {
      error({
        statusCode: 404,
        message: "Page could not be found",
      })
    }
  },
  filters: {
    formatDate: function ( date ) {
      const options = { year: "numeric", month: "numeric", day: "numeric" };
      return new Date( date ).toLocaleDateString( "en", options );
    }
  }
}
</script>

<style>
</style>
