<template>
  <div class="layout-site">

    <section class="layout-block">
      <h5 v-if="from"><NuxtLink :to="from.fullPath">/ {{ from.name | capitalize }}</NuxtLink></h5>
      <h5 v-else><NuxtLink to="/artwork">/ Artwork</NuxtLink></h5>
    </section>


    <div class="layout-page">

      <section class="layout-block">
        <h1 v-if="artwork.title">{{ artwork.title }}</h1>
        <h1 v-else><i>Untitled</i></h1>
        <p v-if="artwork.flavor">{{ artwork.flavor }}</p>

        <!--
        <section v-if="artwork.detail_images" class="overflow-x">
          <img :srcset="require( `~/assets/artwork/${ artwork.slug }/original.jpg` ).srcSet"
                class="item-center"
                :alt="artwork.alt"
                :title="artwork.alt">
          <img v-for="number of artwork.detail_images"
               :key="number"
               :srcset="require( `~/assets/artwork/${ artwork.slug }/detail-${ number }.jpg` ).srcSet"
               :alt="artwork.alt"
               :title="artwork.alt">
        </section>
        -->

        <section class="item-center">
          <img :srcset="require( `~/assets/artwork/${ artwork.slug }/original.jpg` ).srcSet"
                :alt="artwork.alt"
                :title="artwork.alt">
        </section>

      </section>


      <hr></hr>


      <NuxtContent :document="artwork" class="layout-block"/>


      <hr></hr>


      <article id="data" class="layout-block">
        <section class="layout-item">
          <h5>Known As</h5>
          <p v-if="artwork.title">{{ artwork.title }}</p>
          <p v-else><i>Untitled</i></p>
        </section>
        <section class="layout-item">
          <h5>People Involved</h5>
          <p v-if="artwork.authors">{{ artwork.authors }}</p>
          <p v-else>Joel Lithgow</p>
        </section>
        <section class="layout-item">
          <h5>Described As</h5>
          <p v-if="artwork.description">{{ artwork.description }}</p>
          <p v-else>Undescribed</p>
        </section>
        <section class="layout-item">
          <h5>Completed On</h5>
          <p v-if="artwork.date">{{ artwork.date | formatDate }}</p>
          <p v-else>Date unknown</p>
        </section>
        <section class="layout-item">
          <h5>Physical Size</h5>
          <p v-if="artwork.size">{{ artwork.size }}</p>
          <p v-else>Size unknown</p>
        </section>
        <section class="layout-item">
          <h5>Categorized as</h5>
          <p v-if="artwork.medium">{{ artwork.medium }}</p>
          <p v-else>Category unknown</p>
        </section>
        <section class="layout-item">
          <h5>Created From</h5>
          <p v-if="artwork.materials">{{ artwork.materials }}</p>
          <p v-else>Unknown materials</p>
        </section>
        <section class="layout-item">
          <h5>Created With</h5>
          <p v-if="artwork.tools">{{ artwork.tools }}</p>
          <p v-else>Unknown tools</p>
        </section>
        <section class="layout-item">
          <h5>Apart Of</h5>
          <p v-if="artwork.series">{{ artwork.series }}</p>
          <p v-else>This is a standalone piece.</p>
        </section>
        <!-- Not doing atm
        <section class="layout-item">
          <h5>Featured In</h5>
          <p v-if="artwork.collections">{{ artwork.collections }}</p>
          <p v-else>This work is independant from any collections.</p>
        </section>
        -->
        <section class="layout-item">
          <h5>Showed In:</h5>
          <p v-if="artwork.exhibitions">{{ artwork.exhibitions }}</p>
          <p v-else>This work has not been shown.</p>
        </section>
      </article>

      <hr></hr>

      <!-- not used yet, has a bug!
      <section id="pagination" class="layout-block row">
        <section>
          <p v-if="prev">Previous:
            <NuxtLink :to="{ name: 'artwork-slug', params: { slug: prev.slug } }">
              {{ prev.title }}
            </NuxtLink>
          </p>
        </section>
        <section class="item-right">
          <p v-if="next">Next: 
            <NuxtLink :to="{ name: 'artwork-slug', params: { slug: next.slug } }">
              {{ next.title }}
            </NuxtLink>
          </p>
        </section>
      </section>
      -->
      

    </div>

  </div>
</template>

<script>
export default {
  async asyncData({ $content, params, error, from }) {
    try {
      const artwork = await $content( "artwork", params.slug ).fetch()

      const [ prev, next ] = await $content( "artwork" )
        .only([ "title", "slug" ])
        .sortBy( "title", "asc" )
        .surround( params.slug )
        .fetch()

      if ( from.name == "artwork-slug" ) {
        from = undefined;
      } else if ( from.name == "artwork-collections-slug" ) {
        from.name = from.params.slug;
      } else if ( from.name == "exhibitions-slug" ) {
        from.name = "exhibitions-" + from.params.slug;
        //from.name = from.params.slug;
      }

      return { artwork, prev, next, from }
    } catch ( err ) {
      error({
        statusCode: 404,
        message: "Artwork with specified slug could not be found."
      })
    }
  },
  filters: {
    capitalize: function( value ) {
      if ( !value ) {
        return "";
      }
      value = value.toString();
      return value.charAt( 0 ).toUpperCase() + value.slice(1);
    },
    formatDate: function ( date ) {
      const options = { year: "numeric", month: "numeric", day: "numeric" };
      return new Date( date ).toLocaleDateString( "en", options );
    }
  },
}
</script>

<style>
#footer {
  justify-content: center;
}

.overflow-x {
  overflow-x: scroll;
  display: flex;
  gap: 24px;
  padding: 16px;
}

</style>
