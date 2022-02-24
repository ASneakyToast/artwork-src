<template>
  <div class="layout-site">


    <section class="layout-block">
      <h5><NuxtLink to="/">jlithgow.com</NuxtLink></h5>
    </section>


    <div class="layout-page">

      <section class="layout-block">
        <h1>Artist Statement</h1>
      </section>

      <hr></hr>

      <nuxt-content :document="statement" class="layout-block"/>

    </div>


  </div>
</template>



<script>
export default {
  async asyncData({ $content, params, error }) {
    try {
      /* Doesn't work
      const statement = await $content( "artist-statements", "feb-15-2021" )
        .sortBy( "date", "asc" )
        .limit( 1 )
        .fetch();
      */
      const statement = await $content( "artist-statements", "feb-15-2021" ).fetch();

      return { statement }

    } catch ( err ) {
      error({
        statusCode: 404,
        message: "Page could not be found or had issues loading.",
      })
    }
  }
}
</script>
