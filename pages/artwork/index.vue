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
            <h4><NuxtLink to="/artwork/archive">Archive</NuxtLink></h4>
          </li>
        </ul>

        <!--
        <!-- I like this for categories but not for my current work. I think this page should really be focused on detail images n shit. -->
        <section class="layout-block grid-small">
          <article v-for="artwork of artworks"
              :key="artwork.id">
            <NuxtLink :to="{ name: 'artwork-slug', params: { slug: artwork.slug } }" class="layout-item">
              <img :srcset="require( `~/assets/artwork/${ artwork.slug }/original.jpg` ).srcSet"
                    :alt="artwork.alt"
                    :title="artwork.alt"
                    class="square">
              <h4>{{ artwork.title }}</h4>
            </NuxtLink>
          </article>
        </section>
        -->

        <!-- No archive on mvp
        <section class="layout-item">
          <h3><NuxtLink to="/archive">Archive</NuxtLink></h3>
          <p>- Everything from my favorite collections to shitty one-offs.</p>
        </section>
        -->
      </aside>

      <main>
        <img :srcset="require( `~/assets/pictures/studioshot.jpg` ).srcSet"
             alt="A photo of Joel Lithgow making art."
             title="Joel Lithgow"
             class="cover">
      </main>

    </div>
  </div>
</template>

<script>
export default {
  // Make photo load random artwork or content every time.
  // Or make a specific photo ( which I think I like more )
}
</script>
