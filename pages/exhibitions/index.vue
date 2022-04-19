<template>
  <div class="layout-site">

    <section class="layout-block">
      <h5><NuxtLink to="/">jlithgow.com</NuxtLink></h5>
    </section>


    <div class="layout-page">

      <section class="layout-block">
        <h1>Exhibitions</h1>
      </section>

      <hr></hr>

      <main class="layout-section">

        <section v-if="ongoingEx.length > 0" class="layout-block">
          <h2>Ongoing</h2>
          <article v-for="exhibition of ongoingEx"
                   :key="exhibition.id">
            <NuxtLink :to="{ name: 'exhibitions-slug', params: { slug: exhibition.slug } }"
                      class="layout-block card exhibition"
                      :class="{ 'big-grid--featured': exhibition.featured }">
              <section class="layout-item row">
                <main class="layout-item">
                  <h3>{{ exhibition.showTitle }}</h3>
                  <h5>@ {{ exhibition.galleryName }}</h5>
                </main>
                <aside class="item-right">
                  <p>{{ exhibition.dateStart | formatDate }} - {{ exhibition.dateEnd | formatDate }}</p>
                </aside>
              </section>
            </NuxtLink>
          </article>
        </section>

        <hr v-if="ongoingEx.length > 0 && upcommingEx.length > 0" class="secondary"></hr>

        <section v-if="upcommingEx.length > 0" class="layout-block">
          <h2>Upcomming</h2>
          <article v-for="exhibition of upcommingEx"
                   :key="exhibition.id"
                   class="layout-block grid-plate">

              <main class="layout-item card card--noshadow row">
                <main class="layout-item">
                  <h3>{{ exhibition.showTitle }}</h3>
                  <a :href="'//' + `www.googlemaps.com/maps/search/?api=1&query=${ exhibition.location }`" target="_blank" rel="noreferrer noopener">
                    <h5>@ {{ exhibition.galleryName }}</h5>
                  </a>
                </main>
                <aside class="item-right">
                  <p>{{ exhibition.dateStart | formatDate }} - {{ exhibition.dateEnd | formatDate }}</p>
                </aside>
              </main>

              <aside v-if="exhibition.calendar != undefined" class="layout-block">
                <a :href="'//' + `${ exhibition.calendar }`" target="_blank" rel="noreferrer noopener">
                  <article @click="saveCal" class="layout-block card">
                    <h5>Save to Calendar</h5>
                  </article>
                </a>
              </aside>
              
          </article>
        </section>

        <hr class="secondary"></hr>

        <section v-if="pastEx" class="layout-block">
          <h2>past</h2>
          <article v-for="exhibition of pastEx"
                   :key="exhibition.id">
            <NuxtLink :to="{ name: 'exhibitions-slug', params: { slug: exhibition.slug } }"
                      class="layout-block card exhibition"
                      :class="{ 'big-grid--featured': exhibition.featured }">
              <section class="layout-item row">
                <main class="layout-item">
                  <h3>{{ exhibition.showTitle }}</h3>
                  <h5>@ {{ exhibition.galleryName }}</h5>
                </main>
                <aside class="item-right">
                  <p>{{ exhibition.dateStart | formatDate }} - {{ exhibition.dateEnd | formatDate }}</p>
                </aside>
              </section>
            </NuxtLink>
          </article>
        </section>

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
        .only([ "showTitle", "calendar", "description", "featured", "slug", "galleryName", "dateStart", "dateEnd", "location" ])
        .sortBy( "dateStart", "desc" )
        .fetch()
      
      //return { ongoing, past, upcomming, exhibitions }
      return { exhibitions }

    } catch ( err ) {
      error({
        statusCode: 404,
        message: "Page could not be found",
      })
    }
  },
  computed: { 
    pastEx: function () {
      let dateToday = new Date().toISOString();
      return this.exhibitions.filter( ex => ex.dateStart < dateToday );
    },
    upcommingEx: function() {
      let dateToday = new Date().toISOString();
      return this.exhibitions.filter( ex => ex.dateStart > dateToday );
    },
    ongoingEx: function() {
      let dateToday = new Date().toISOString();
      return this.exhibitions.filter( ex => ex.dateStart < dateToday && ex.dateEnd > dateToday );
    }
  },
  filters: {
    formatDate: function ( date ) {
      const options = { year: "numeric", month: "numeric", day: "numeric" };
      let formatted = new Date( date ).toLocaleDateString( "en", options );

      if ( formatted != "Invalid Date" ) {
        return formatted;
      } else {
        return date;
      }
    }
  },
  methods: {
    saveCal: function() {
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
