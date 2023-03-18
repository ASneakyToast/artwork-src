<template>
  <div class="layout-site">

    <section class="layout-block">
      <h5><NuxtLink to="/artwork">/ Artwork</NuxtLink></h5>
    </section>

    <div class="layout-page">

      <section class="layout-block">
        <h1>Archive</h1>
        <p>All documented artwork of mine, spanning several years and practices.</p>
      </section>

      <hr></hr>

      <!--
      <section class="layout-block">
        <h3>Digital Gallery</h3>
        <NuxtLink to="/artwork/showcase">Showcase</NuxtLink></h4>
      </section>
      -->

      <!--
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
      -->

      <!--
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
      -->

      <section class="layout-block list">
        <section v-for="( year, index ) of artworkByYears"
                 :key="index"
                 class="haskey layout-item">
          <aside>
            <h3>{{ unique_years[ index ] }}</h3>
          </aside>
          <main>
            <ul class="artworks">
              <article v-for="artwork of year"
                       :key="artwork.id"
                       class="artworks__item">
                <NuxtLink :to="{ name: 'artwork-slug', params: { slug: artwork.slug } }">
                  <p>{{ artwork.title }}</p>
                  <img :srcset="require( `~/assets/artwork/${ artwork.slug }/photo-1.jpg` ).srcSet"
                       :alt="artwork.alt"
                       :title="artwork.alt" />
                </NuxtLink>
              </article>
            </ul>
          </main>
        </section>
        <hr></hr>
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
  async asyncData({ $content, params, error }) {
    try {

      const artworks = await $content({ deep: true })
        .where({ dir: `/artwork`, published: true })
        .only([ "title", "slug", "collections", "date" ])
        //.sortBy( "title", "asc" )
        .sortBy( "date", "desc" )
        .fetch()

      let all_years = [];
      artworks.forEach( artwork => {
        all_years.push( new Date( artwork.date ).getFullYear() );
      });
      let unique_years = [ ...new Set( all_years.flat( 1 ) ) ].sort();
      unique_years = unique_years.sort().reverse(); // From newest to oldest
      console.log( unique_years );

      /*
      let artworkByYears = [];
      unique_years.forEach( ( year, index ) => {
        artworkByYears[ index ] = [];
        artworks.forEach( artwork => {
          //console.log( new Date( artwork.date ).getFullYear() );
          //console.log( new Date( year ).getFullYear() );
          //console.log( year );
          if ( new Date( artwork.date ).getFullYear() == year ) {
            artworkByYears[ index ].push( artwork );
          }
        })
      });
      console.log( artworkByYears );
      console.log( artworkByYears[ "2020" ] );
      */

      let artworkByYears = [];
      unique_years.forEach( ( year, index ) => {
        artworkByYears[ index ] = [];
        artworks.forEach( artwork => {
          if ( new Date( artwork.date ).getFullYear() == year ) {
            artworkByYears[ index ].push( artwork );
          }
        })
      });
      console.log( artworkByYears );

      let all_collections = [];
      artworks.forEach( artwork => {
        if ( Array.isArray( artwork.collections ) ) {
          artwork.collections.forEach( collection => {
            all_collections.push( collection )
          })
        } else if ( artwork.collections instanceof String ) {
          all_collections.push( artwork.collections )
        } else if ( artwork.collections == undefined ) {
          // Does not have a collection, ignore.
        } else {
          console.log( "error: something wrong with artwork collection:" )
          console.log( artwork );
        }
      });
      let collections = [ ...new Set( all_collections.flat( 1 ) ) ].sort();
      console.log( collections );

      //return { artworks, collections, years }
      return { artworks, collections, unique_years, artworkByYears }
    
    } catch ( err ) {
      error({
        statusCode: 404,
        message: "Page could not be found or had issues loading.",
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

<style scoped>
.artworks {
  columns: 3;
  break-inside: avoid-column;
}

.artworks__item {
  break-inside: avoid-column;
}
</style>
