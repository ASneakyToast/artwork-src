<template>
  <div class="layout-site">

    <div class="layout-page">


        <section class="navigation">
          <h5>jlithgow.com</h5>
          <h3><NuxtLink to="/bio">About</NuxtLink></h3>
          <h3><NuxtLink to="/artwork">Artwork</NuxtLink></h3>
          <h3><NuxtLink to="/exhibitions">Exhibitions</NuxtLink></h3>
          <!-- <h3><NuxtLink to="/cv">CV</NuxtLink></h3> -->
        </section>

        <section class="artworks">
          <p class="artworks__title">ARTWORK</p>
          <NuxtLink v-for="artwork of artworks"
                   :key="artwork.id"
                   class="artworks__item"
                   :to="{ name: 'artwork-slug', params: { slug: artwork.slug } }">
            <img :srcset="require( `~/assets/artwork/${ artwork.slug }/photo-1.jpg` ).srcSet"
                 :alt="artwork.alt"
                 :title="artwork.alt" />
            <p class="artworks__info">{{ artwork.title }}</p>
          </NuxtLink>
        </section>


    </div>
    
  </div>
</template>



<script>
export default {
  async asyncData({ $content, params }) {
    try {

      const artworks = await $content( "artwork" )
        .where({ featured: true })
        //.only([ "title", "slug", "date", "size", "medium", "materials", "description" ])
        .sortBy( "title", "asc" )
        .fetch()

      return { artworks }

    } catch ( err ) {
      error({
        statusCode: 404,
        message: "Page could not be found",
      })
    }
  },
}
</script>


<style scoped lang="scss">
.navigation {
  display: flex;
  flex-direction: column;
  gap: 16px; 
  position: fixed;
  left: 0;
  top: 128px;
  background: white;
  padding: 32px 32px 32px 16px;
}

.artworks {
}

.artworks__title {
  font-size: 64px;
  font-weight: bolder;
  font-family: sans-serif;
  color: lightgray;
  margin-top: 30px;
  margin-bottom: 64px;
  margin-left: 40px;
}


.artworks__item:first-child {
  width: 700px;
  margin-top: 64px;
  margin-left: 140px;
  max-width: 630px;
}

.artworks__item:nth-child(3) {
  width: 800px;
  float: right;
  margin-left: 140px;
}

.artworks__item:nth-child(4) {
  width: 600px;
  margin-top: 64px;
}

.artworks__item:nth-child(5) {
  width: 400px;
  margin-top: 64px;
  max-width: 630px;
}

.artworks__item:nth-child(6) {
  width: 100%;
  margin-top: 64px;
}

.artworks__item:nth-child(7) {
  width: 800px;
  float: right;
  margin-top: 64px;
}

.artworks__item:nth-child(8) {
  width: 600px;
  margin-top: 64px;
  max-width: 630px;
}

.artworks__item {
  display: block;
  margin-bottom: 64px;
}

.artworks__info:link {
  text-decoration: none;
}
.artworks__info {
  display: inline-block;
  margin-left: 16px;
  padding: 16px;
  background: white;
  min-height: 0;
  height: fit-content;
  align-self: end;
  border-radius: 1px;
  box-shadow: 0 1px 2px rgba(0,0,0,0.1);

  font-size: 12px;

}


@media only screen and ( max-width: 640px ) {
  .layout-page {
    flex-direction: column;
    grid-gap: 32px;
  }

  .layout-page > main {
    max-width: 460px;
  }

  .layout-page > aside {
    display: block;
  }
}

@media only screen and ( max-width: 800px ) and ( min-width: 640px ) {
  .layout-page > main {
    max-width: 460px;
  }

  .layout-page > aside {
    display: block;
  }
}

@media only screen and ( min-width: 800px ) {
  .layout-page > main {
    max-width: 460px;
  }

  .layout-page > aside {
    display: block;
  }
}
</style>
