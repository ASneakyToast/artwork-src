<template>
  <div class="layout-site">

    <section class="layout-block">
      <h5><NuxtLink to="/">jlithgow.com</NuxtLink></h5>
    </section>


    <div class="layout-page grid-plate">


      <main class="layout-section">
        <section class="layout-block">
          <h1>Bio</h1>
          <section class="layout-item">
            <p>Joel Lithgow is a multidisciplinary tech-based artist with a BFA in Fine Art from California College of the Arts.</p>
            <p>His work often goes through a series of exploratory transmutations between the digital and physical. The process involves interfacing with all aspects of technology, from laser cutters to rubber stamps and defunct machines to complex systems such as material distribution systems.</p>
            <p>Charged with a religious-like theory of a technological hyperobject, his practice is informed by the spiritual / synthetic connectivity of today's world.</p>
          </section>
          <NuxtLink to="/statement">Read Statement</NuxtLink>
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
        <img :srcset="require( `~/assets/pictures/studioshot.jpg` ).srcSet"
             alt="A photo of Joel Lithgow making art."
             title="Joel Lithgow"
             class="cover">
      </aside>


    </div>


  </div>
</template>
