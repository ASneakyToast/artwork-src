<template>
  <div class="layout-site">

    <section class="layout-block">
      <h5><NuxtLink to="/">jlithgow.com</NuxtLink></h5>
    </section>


    <div class="layout-page">


      <main class="layout-section">
        <section class="layout-block">
          <h1>Bio</h1>
          <section class="layout-item">
            <p>Joel Lithgow is a multidisciplinary tech-based artist with a BFA in Fine Art from California College of the Arts.</p>
            <p>He was born in Australia but raised in the US. Growing up mainly in the Santa Cruz Mountains in California. His love for art started with wanted to spend as much time as he could in creative programs such as Autocad, Illustrator, Photoshop, and Blender.</p>
            <p>His work often goes through a series of exploratory transmutations between the digital and physical. The process involves interfacing with all aspects of technology, from laser cutters to rubber stamps and defunct machines to complex systems such as material distribution systems.</p>
            <!-- <p>Charged with a religious-like theory of a technological hyperobject, his practice is informed by the spiritual / synthetic connectivity of today's world.</p> -->
          </section>
        </section>

        <hr></hr>

        <section class="layout-block">
          <NuxtLink to="/influences">Influences</NuxtLink>
          <NuxtLink to="/statement">Artist's Statement</NuxtLink>
        </section>

        <hr></hr>

        <section class="layout-block">
          <section class="layout-item">
            <h5>Email:</h5>
            <a href="mailto:joelnotlithgow@gmail.com"><p>joelnotlithgow@gmail.com</p></a>
          </section>
          <section class="layout-item">
            <h5>Instagram</h5>
            <a href="https://www.instagram.com/joellithgow"target="_blank" rel="noreferrer noopener"><p>joellithgow</p></a>
          </section>
        </section>
      </main>


      <aside>
        <img :srcset="require( `~/assets/pictures/studioshot-${ imageNumber }.jpg` ).srcSet"
             alt="A photo of Joel Lithgow making art."
             title="Joel Lithgow"
             class="cover"
             @click="changePhoto()">
      </aside>


    </div>


  </div>
</template>



<script>
export default {
  data: () => {
    return {
      imageNumber: Math.floor(Math.random()*3)+1,
    }
  },
  methods: {
    changePhoto: function() {
      if ( this.imageNumber >= 3 ) {
        this.imageNumber = 1;
      } else {
        this.imageNumber++;
      }
    }
  }
}
</script>



<style scoped>
.layout-page {
  display: flex;
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
