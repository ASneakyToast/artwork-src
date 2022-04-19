<template>
  <div class="layout-site">

    <section class="layout-block">
      <h5><NuxtLink to="/artwork/collections">/ Artwork-Collections</NuxtLink></h5>
    </section>


    <div class="layout-page">
      
      <section class="layout-block">
        <h1>{{ params.slug }}</h1>
      </section>

      <hr></hr>

      <section class="layout-block">
        <p>Artworks included</p>
        <ol class="layout-item">
          <li v-for="artwork of artworks"
              :key="artwork.id">

            <section class="inline">
              <!-- Hyperlink instead of link <NuxtLink :to="{ path: '/portfolio', hash: `#${ artwork.slug }` }"> -->
              <NuxtLink :to="{ name: 'artwork-slug', params: { slug: artwork.slug } }">
                <h4>{{ artwork.title }}</h4>
              </NuxtLink>
              <p>( {{ artwork.date | formatDate }} )</p>
            </section>

          </li>
        </ol>
      </section>

      <hr></hr>

      <section id="artworks" class="layout-block">

        <article v-for="artwork of artworks"
            :key="artwork.id"
            :id="artwork.slug">
          <aside class="item-center">
            <ArtworkPhotos :artwork="artwork" />
          </aside>
          <main class="layout-item well">
            <header>
              <h4>
                <NuxtLink :to="{ name: 'artwork-slug', params: { slug: artwork.slug } }">
                  {{ artwork.title }}
                </NuxtLink>
              </h4>
            </header>
            <main>
              <p>{{ artwork.date | formatDate }}</p>
              <p>{{ artwork.size }}</p>
              <p>{{ artwork.medium }}</p>
              <p>{{ artwork.materials }}</p>
            </main>
            </header>
            <footer>
              <p>{{ artwork.flavor }}</p>
            </footer>
          </main>

          <br></br>

        </article>

      </section>


    </div>

  </div>
</template>



<script>
export default {
  async asyncData({ $content, params }) {
    try {

      const artworks = await $content( "artwork" )
        .where({ collections: { $contains: params.slug } })
        .only([ "id", "title", "photoAmount", "slug", "date", "flavor" ])
        .sortBy( "title", "asc" )
        .fetch()

      return { artworks, params }

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

@media only screen and ( max-width: 860px ) {

  #artworks > article {
    display: grid;
    grid-gap: 16px;
  }

}

@media only screen and ( min-width: 860px ) {
  
  #artworks > article {
    display: grid;
    grid-template-columns: 3fr 1fr;
  }

}

.well {
  margin: 24px;
  height: fit-content;
}
</style>
