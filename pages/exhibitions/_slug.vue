<template>
  <div class="layout-site">

    <section class="layout-block">
      <h5><NuxtLink to="/exhibitions">/ Exhibitions</NuxtLink></h5>
    </section>


    <div class="layout-page grid-plate">

      <main class="layout-block">
        <section class="layout-block">
          <h1>{{ exhibition.showTitle }}</h1>
        </section>

        <hr></hr>

        <section class="layout-item">
          <h3>{{ exhibition.date }}</h3>
          <!--
          <a href="`http://www.googlemaps.com/maps/search/?api=1&query=${ exhibition.location }`" target="_blank" rel="noreferrer noopener">
            <p>{{ exhibition.galleryName }}</p>
          </a>
          <NuxtLink :to="{ name: 'Google Maps', path: '/maps', beforeEnter() { location.href = '`https://www.googlemaps.com/maps/search/?api=1&query=${ exhibition.location }`' } }" target="_blank" rel="noreferrer noopener">
            <p>{{ exhibition.galleryName }}</p>
          </NuxtLink>
          -->
          <a :href="'//' + `www.googlemaps.com/maps/search/?api=1&query=${ exhibition.location }`" target="_blank" rel="noreferrer noopener">
            <p>{{ exhibition.galleryName }}</p>
          </a>
        </section>
      </main>

      <aside class="layout-block">
        <a :href="'//' + `${ exhibition.calendar }`" target="_blank" rel="noreferrer noopener">
          <article @click="saveCal" class="layout-block card">
            <!--
            <section class="layout-item">
              <p>{{ exhibition.showTitle }}</p>
              <p>{{ exhibition.date }}</p>
            </section>
            -->
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

      return { exhibition }
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
