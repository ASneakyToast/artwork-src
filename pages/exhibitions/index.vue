<template>
  <div class="layout-site">

    <section class="layout-block">
      <h5><NuxtLink to="/">jlithgow.com</NuxtLink></h5>
    </section>


    <div class="layout-page">

      <section class="layout-block">
        <h1>Exhibitions + Showings</h1>
      </section>

      <hr></hr>

      <main class="layout-block">

        <article v-for="exhibition of exhibitions"
                 :key="exhibition.id">
          <NuxtLink :to="{ name: 'exhibitions-slug', params: { slug: exhibition.slug } }"
                    class="layout-block card exhibition"
                    :class="{ 'big-grid--featured': exhibition.featured }">

          <!--
            <img :srcset="require( `~/assets/exhibitions/${ exhibition.slug }/cover.jpg` ).srcSet"
                  :alt="exhibition.alt"
                  :title="exhibition.alt">
          -->
            <section class="layout-item row">
              <main class="layout-item">
                <h3>{{ exhibition.showTitle }}</h3>
                <h5>@ {{ exhibition.galleryName }}</h5>
              </main>
              <aside class="item-right">
                <p>{{ exhibition.date }}</p>
              </aside>
            </section>

            <!--
            <section class="layout-item">
              <section class="layout-item">
                <h5>Gallery</h5>
                <p>CCA Hubble Street Galleries</p>
              </section>
              <section class="layout-item">
                <h5>Location</h5>
                <p>{{ exhibitio.location }}</p>
              </section>
              <section class="layout-item">
                <h5>Date + Time</h5>
                <p>{{ exhibitio.date }}</p>
              </section>
            </section>
            -->

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

      const exhibitions = await $content( "exhibitions" )
        .where({ published: true })
        .only([ "showTitle", "description", "featured", "slug", "galleryName", "date", "location" ])
        .sortBy( "date", "asc" )
        .fetch()

      return { exhibitions }

    } catch ( err ) {
      error({
        statusCode: 404,
        message: "Page could not be found",
      })
    }
  }
}
</script>



<style>
@container ( min-width: 600px ) {
  .exhibition {
    display: grid;
    grid-template-columns: 2fr 1fr;
  }
}
</style>
