<template>
  <div class="layout-site">

    <section class="layout-block">
      <h5>jlithgow.com</h5>
    </section>


    <div class="layout-page">


      <main class="layout-section">
        
        <section class="layout-block">
          <h1>Joel Lithgow</h1>
          <h2>is a multidisciplinary tech-based artist who's work often goes through a series of transmutations between the digital and physical.</h2>
        </section>

        <hr></hr>

        <section class="layout-block">
          <section class="layout-item">
            <h3><NuxtLink to="/exhibitions">Exhibitions</NuxtLink></h3>
          </section>
          <section class="layout-item">
            <h3><NuxtLink to="/artwork">Artwork</NuxtLink></h3>
          </section>
          <section class="layout-item">
            <h3><NuxtLink to="/bio">Bio</NuxtLink></h3>
          </section>
          <section class="layout-item">
            <h3><NuxtLink to="/cv">cv</NuxtLink></h3>
          </section>
        </section>

      </main>


      <aside class="layout-section">
        <section class="layout-block">
          <video width="100%" autoplay muted loop>
            <!-- <source :src="require( `~/assets/pictures/AGardenCampus.mp4` )" type="video/mp4"> -->
            <source src="https://storage.googleapis.com/joellithgow-media/AGardenCampus.mp4" type="video/mp4">
          </video>
        </section>
      </aside>


    </div>

    
  </div>
</template>



<script>
export default {
  // Make photo load random artwork or content every time.
  // Or make a specific photo ( which I think I like more )
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
