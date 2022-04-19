<template>
  <div class="layout-site">

    <section class="layout-block">
      <h5><NuxtLink to="/">jlithgow.com</NuxtLink></h5>
    </section>


    <div class="layout-page">


      <main class="layout-section">
        <h1>Influences</h1>

        <hr></hr>

        <ul class="layout-block">
          <li v-for="influence of influences" :id="influence.id">
            <article class="layout-item">
              <a :href="influence.link" target="_blank" rel="noopener noreferrer">
                <h4>{{ influence.title }}</h4>
              </a>
              <p>{{ influence.description }}</p>
            </article>
          </li>
        </ul>
      </main>


    </div>

  </div>
</template>



<script>
export default {
  async asyncData({ $content, params, error }) {
    try {

      const influences = await $content( "influences" )
        .where({ published: true })
        .fetch()

      return { influences }

    } catch ( err ) {
      error({
        statusCode: 404,
        message: "Page could not be found",
      })
    }
  }
}
</script>
