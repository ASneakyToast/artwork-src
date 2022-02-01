<template>
  <div class="layout-site">

    <section class="layout-block">
      <h5>jlithgow.com</h5>
    </section>


    <div class="layout-page grid-plate">


      <aside class="layout-section">

        <section class="layout-block">
          <h1>Joel Lithgow</h1>
        </section>

        <hr></hr>

        <section class="layout-block">

          <section class="layout-item">
            <h3><NuxtLink to="/exhibitions">Exhibitions + Showings</NuxtLink></h3>
            <!-- <p>Upcoming events.</p> -->
          </section>

          <section class="layout-item">
            <h3><NuxtLink to="/artwork/current">Current Artwork</NuxtLink></h3>
            <!-- <p>Fine art and conceptual work.</p> -->
          </section>

          <section class="layout-item">
            <h3><NuxtLink to="/artwork/archive">Archive</NuxtLink></h3>
            <!-- <p>Fine art and conceptual work.</p> -->
          </section>

          <section class="layout-item">
            <h3><NuxtLink to="/bio">Bio</NuxtLink></h3>
            <!-- <p>More about my life and how to contact me.</p> -->
          </section>

        </section>

      </aside>


      <main class="layout-section">
        <section class="layout-block">
          <img :srcset="require( `~/assets/pictures/studioshot.jpg` ).srcSet"
               alt="A photo of Joel Lithgow making art."
               title="Joel Lithgow"
               class="cover">
        </section>
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
