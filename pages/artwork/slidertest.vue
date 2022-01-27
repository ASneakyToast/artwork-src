<template>
  <div class="lauyout-site">

    <section class="layout-block">
      <h5><NuxtLink to="/">/ Home</NuxtLink></h5>
    </section>


    <div class="layout-page">

      <section class="layout-block">
        <h1>Artwork</h1>
        <p>what what up</p>
      </section>

      <section class="layout-block">
        <transition-group name="fade" tag="div">
          <div v-for="i in [currentindex]" :key="i">
            <img :src="currentImg" />
          </div>
        </transition-group>
      </section>


    </div>

  </div>
</template>

<script>
export default {
  async asyncData({ $content, params, error, from }) {
    try {
      const artworks = await $content({ deep: true })
        .where({ dir: `/artwork` })
        .only([ "title", "slug", "collections" ])
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


  //name: "Slider",
  data() {
    return {
      images: [],
      timer: null,
      currentIndex: 0
    }
  },

  mounted: function() {
    this.startSlide();
  },

  methods: {
    startSlide: function() {
      this.timer = setInterval( this.nnext, 4000 );
    },

    next: function() {
      this.currentIndex += 1;
    },

    prev: function() {
      this.currentIndex -= 1;
    },

    computed: {
      currentImg: function() {
        return this.images[ Math.abs( this.currentIndex ) % this.images.length ];
      }
    }
  }
}
</script>

<style>
.fade-enter-active,
.fade-leave-active {
  transition: all 0.9 ease;
  overflow: hidden;
  visibility: visible;
  position: absolute;
  width: 100%;
  opacity: 1;
}

.fade-enter,
.fade-leave-to {
  visibility: hidden;
  width: 100%;
  opacity: 0;
}

img {
  height: 600px;
  width: 100%;
}

.prev, .next {
  cursor: pointer;
  position: absolute;
  top: 40%;
  width: auto;
  padding: 16px;
  color: white;
  font-weight: bold;
  font-size: 18px;
  transition: 0.7s ease;
  border-radius: 0 4px 4px 0;
  text-decoration: none;
  user-select: none;
}

.next {
  right: 0;
}

.prev {
  left: 0;
}

.prev:hover, .nnext:hover {
  background-color: rgba( 0,0,0,0.9 );
}
