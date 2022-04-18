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

        <section v-if="ongoing.length > 0" class="layout-block">
          <h2>Ongoing</h2>
          <article v-for="exhibition of ongoing"
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

        <hr v-if="ongoing.length > 0 && upcomming.length > 0" class="secondary"></hr>

        <section v-if="upcomming.length > 0" class="layout-block">
          <h2>Upcomming</h2>
          <article v-for="exhibition of upcomming"
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

        <hr class="secondary"></hr>

        <section v-if="past" class="layout-block">
          <h2>past</h2>
          <article v-for="exhibition of past"
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
    const dateToday = new Date().getTime();
    try {

      const ongoing = await $content( "exhibitions" )
        .where({ published: true })
        .where({ dateStart: { $lt: dateToday }, dateEnd: { $gt: dateToday }})
        //.where({ dateEnd: { $gt: dateToday } })
        .only([ "showTitle", "description", "featured", "slug", "galleryName", "dateStart", "dateEnd", "location" ])
        .sortBy( "dateStart", "desc" )
        .fetch()

      const past = await $content( "exhibitions" )
        .where({ published: true })
        .where({ dateStart: { $lt: dateToday } })
        .where({ dateEnd: { $lt: dateToday } })
        .only([ "showTitle", "description", "featured", "slug", "galleryName", "dateStart", "dateEnd", "location" ])
        .sortBy( "dateStart", "desc" )
        .fetch()

      const upcomming = await $content( "exhibitions" )
        .where({ published: true })
        .where({ dateStart: { $gte: dateToday } })
        .only([ "showTitle", "description", "featured", "slug", "galleryName", "dateStart", "dateEnd", "location" ])
        .sortBy( "dateStart", "desc" )
        .fetch()

      return { ongoing, past, upcomming }

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
      let formatted = new Date( date ).toLocaleDateString( "en", options );

      if ( formatted != "Invalid Date" ) {
        return formatted;
      } else {
        return date;
      }
    }
  },
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
