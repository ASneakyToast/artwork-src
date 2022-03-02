<template>
  <div class="layout-site">

    <section class="layout-block">
      <h5><NuxtLink to="/exhibitions">/ Exhibitions</NuxtLink></h5>
    </section>


    <div class="layout-page grid-plate">


      <main class="layout-section">
        <section id="header" class="layout-block row">
          <section class="layout-block">
            <h1>{{ exhibition.showTitle }}</h1>
            <a :href="'//' + `www.googlemaps.com/maps/search/?api=1&query=${ exhibition.location }`" target="_blank" rel="noreferrer noopener">
              <h3>@ {{ exhibition.galleryName }}</h3>
            </a>
          </section>
          <h3 class="item-right">{{ exhibition.date }}</h3>
        </section>

        <hr></hr>

        <div class="layout-block">
          <NuxtContent :document="exhibition" class="layout-block"/>

          <!-- when this data was static and not in content body
          <section v-if="exhibition.description" class="layout-item">
            <h6>Description:</h6>
            <p>{{ exhibition.description }}</p>
          </section>

          <section v-if="exhibition.entryDetails" class="layout-item">
            <h6>Entry Details:</h6>
            <p>{{ exhibition.entryDetails }}</p>
          </section>
          -->
        </div>

        <hr></hr>

        <div v-if="artworks.length > 1" class="layout-block">
          <h2>Artwork Included</h2>
          <section v-for="artwork of artworks"
                   :key="index">
            <NuxtLink :to="{ name: 'artwork-slug', params: { slug: artwork.slug } }"
                      class="layout-item">
              <img :srcset="require( `~/assets/artwork/${ artwork.slug }/original.jpg` ).srcSet"
                    :alt="artwork.alt"
                    :title="artwork.alt">
              <p class="item-center">{{ artwork.title }}</p>
            </NuxtLink>
          </section>
        </div>

      </main>


      <aside v-if="exhibition.calendar != undefined" class="layout-block">
        <a :href="'//' + `${ exhibition.calendar }`" target="_blank" rel="noreferrer noopener">
          <article @click="saveCal" class="layout-block card">
            <h5>Save to Calendar</h5>
          </article>
        </a>
      </aside>


    </div>

  </div>
</template>

<script>
export default {
  async asyncData({ $content, params, error }) {
    try {
      const exhibition = await $content( "exhibitions", params.slug ).fetch()

      const artworks = await $content( "artwork" )
        .where({ 'exhibitions': { $contains: exhibition.showTitle } })
        .only([ "title", "slug", "collections", "date" ])
        .sortBy( "title", "asc" )
        //.sortBy( "date", "asc" )
        .fetch()

      return { exhibition, artworks }
    } catch ( err ) {
      error({
        statusCode: 404,
        message: "Exhibition could not be found."
      })
    }
  },
  methods: {
    saveCal: function() {
    }
  }
}
</script>

<style>
/*
#header {
  background-image: url( "~/assets/exhibitions/Senior-Show/cover.jpg");
  min-height: 20vh;
}
*/
</style>
