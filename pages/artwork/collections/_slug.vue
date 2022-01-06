<template>
  <div class="layout-site">
    
    <section class="layout-block">
      <h5><NuxtLink to="/artwork">/ Artwork</NuxtLink></h5>
    </section>

    <div class="layout-page">

      <h1>{{ params.slug }}</h1>

      <!-- alternate listing with spacing
      <section class="layout-block">
        <article v-for="artwork of artworks"
                 :key="artwork.id"
                 class="layout-item">
          <NuxtLink :to="{ name: 'artwork-slug', params: { slug: artwork.slug } }">
            {{ artwork.title }}
          </NuxtLink>
        </article>
      </section>
      -->

      <ul>
        <li v-for="artwork of artworks"
                 :key="artwork.id">
          <NuxtLink :to="{ name: 'artwork-slug', params: { slug: artwork.slug } }">
            {{ artwork.title }}
          </NuxtLink>
        </li>
      </ul>

      <section class="layout-block">
        <article v-for="artwork of artworks"
                 :key="artwork.id"
                 class="layout-item">
          <NuxtLink :to="{ name: 'artwork-slug', params: { slug: artwork.slug } }">
            <img :src="require( `/assets/artwork/${ artwork.slug }/original.jpg` )"
                   class=""
                   :alt="artwork.alt"
                   :title="artwork.alt">
          </NuxtLink>
        </article>
      </section>

    </div>

  </div>
</template>

<script>
export default {
  async asyncData({ $content, params, error }) {
    try {
      const artworks = await $content( "artwork" )
        //.where({ dir: `/artwork` })
        .where({ collections: { $contains: params.slug } })
        .only([ "id", "title", "slug", "date", "flavor" ])
        .sortBy( "title", "asc" )
        .fetch()

      return { artworks, params }

    } catch ( err ) {
      error({
        statusCode: 404,
        message: "Page could not be found"
      })
    }
  },
}
</script>
