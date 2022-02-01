<template>
  <div class="layout-site">

    <section class="layout-block">
      <h5><NuxtLink to="/">jlithgow.com</NuxtLink></h5>
    </section>

    <div class="layout-page">

      <section class="layout-block">
        <h1>Current Work</h1>
      </section>

      <hr></hr>

      <!--
      <section class="layout-block">
        <article v-for="artwork of artworks"
            :key="artwork.id">
          <NuxtLink :to="{ name: 'artwork-slug', params: { slug: artwork.slug } }">
            <img :srcset="require( `~/assets/artwork/${ artwork.slug }/original.jpg` ).srcSet"
                  :alt="artwork.alt"
                  :title="artwork.alt">
          </NuxtLink>
        </article>
      </section>
      -->

      <section id="artworks" class="layout-block">

        <article v-for="artwork of artworks"
            :key="artwork.id"
            :id="artwork.slug">
          <aside class="item-center">
            <NuxtLink :to="{ name: 'artwork-slug', params: { slug: artwork.slug } }">
              <img :srcset="require( `~/assets/artwork/${ artwork.slug }/original.jpg` ).srcSet"
                   :alt="artwork.alt"
                   :title="artwork.alt" />
            </NuxtLink>
          </aside>
          <main class="layout-block well">
            <header>
              <h4>
                <NuxtLink :to="{ name: 'artwork-slug', params: { slug: artwork.slug } }">
                  {{ artwork.title }}
                </NuxtLink>
              </h4>
            </header>
            <main class="layout-item">
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
export default{
  async asyncData({ $content, params, error }) {
    try {

      const artworks = await $content( "artwork" )
        .where({ featured: true })
        //.only([ "title", "featured", "slug", "collections", "date" ])
        //.sortBy( "title", "asc" )
        //.sortBy( "date", "desc" )
        .fetch()

      return { artworks }
    
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
</style>
