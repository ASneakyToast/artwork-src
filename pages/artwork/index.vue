<template>
  <div class="layout-site">
    <section class="layout-block">
      <h5><NuxtLink to="/">jlithgow.com</NuxtLink></h5>
    </section>

    <div class="layout-page grid-plate">

      <aside class="layout-block">
        <h1>Artwork</h1>

        <hr></hr>

        <ul class="layout-block">
          <li>
            <h4><NuxtLink to="/artwork/current">Current Work</NuxtLink></h4>
          </li>
          <li>
            <h4><NuxtLink to="/artwork/collections">Collections</NuxtLink></h4>
          </li>
          <li>
            <h4><NuxtLink to="/artwork/archive">Archive</NuxtLink></h4>
          </li>
        </ul>
      </aside>

      <main class="layout-block">
        <NuxtLink :to="{ name: 'artwork-slug', params: { slug: artwork.slug } }">
          <article class="layout-item">
            <img :srcset="require( `~/assets/artwork/${ artwork.slug }/original.jpg` ).srcSet"
                 alt="A photo of Joel Lithgow making art."
                 title="Joel Lithgow"
                 class="cover">
            <h5 class="item-center">{{ artwork.title }}</h5>
          </article>
        </NuxtLink>
      </main>

    </div>
  </div>
</template>

<script>
export default {
  async asyncData({ $content, params, error }) {
    try {

      const artworks = await $content( "artwork" )
        .where({ featured: true })
        .only([ "title", "featured", "slug" ])
        .fetch()

      const artwork = artworks[ Math.floor( Math.random() * artworks.length ) ];

      return { artwork }

    } catch ( err ) {
      error({
        statusCode: 404,
        message: "Page could not be found",
      })
    }
  }
}
</script>
