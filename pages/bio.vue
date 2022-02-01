<template>
  <div class="layout-site">

    <section class="layout-block">
      <h5><NuxtLink to="/">jlithgow.com</NuxtLink></h5>
    </section>


    <div class="layout-page grid-plate">


      <aside class="layout-section">
        <section class="layout-block">
          <h1>Bio</h1>
          <p>Site still under construction!</p>
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
