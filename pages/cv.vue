<template>
  <div class="layout-site">

    <section class="layout-block">
      <h5><NuxtLink to="/">jlithgow.com</NuxtLink></h5>
    </section>


    <div class="layout-page grid-plate">

      <main id="resume" class="layout-page">
        <section class="layout-section">
          <p class="detail">cv. {{ new Date | formatDate }}</p>
        </section>

        <section class="layout-section">
          <section class="layout-block">
            <h1 class="title">Joel Lithgow</h1>
            <section class="layout-row">
              <article class="inline-row">
                <IconsPalette class="icon icon-secondary"/><p class="label"><b>Artist</b></p>
              </article>
              <article class="inline-row">
                <IconsRulers class="icon icon-secondary"/><p class="label"><b>Interaction Designer</b></p>
              </article>
              <article class="inline-row">
                <!-- <img class="icon icon-secondary" src="~/assets/icons/hammer.svg"><p>Preparator</p> -->
                <IconsHammer class="icon icon-secondary"/><p class="label"><b>Preparator</b></p>
              </article>
            </section>
          </section>
        </section>

        <section class="layout-section">
          <p class="accent">EXPERIENCE</p>
          <section class="layout-block">
            <article v-for="experience of experiences"
                     id="experience.id"
                     class="layout-item">
              <!-- <p class="label"><b>{{ experience.organization }}</b>, {{ experience.location }} - <i>{{ experience.title }}</i></p> -->
              <p class="label"><b>{{ experience.title }}</b>, {{ experience.organization }} - <i>{{ experience.location }}</i></p>
              <p class="secondary">{{ experience.startDate | formatDate }} - {{ experience.endDate | formatDate }}</p>
              <p>{{ experience.description }}</p>
            </article>
          </section>
        </section>

        <section class="layout-section">
          <p class="accent">EDUCATION</p>
          <section class="layout-block">
            <article v-for="item of education"
                     id="item.id"
                     class="layout-item">
              <!-- <p class="label"><b>{{ item.school }}</b>, {{ item.location }} - <i>{{ item.title }}</i></p> -->
              <p class="label"><b>{{ item.title }}</b> - {{ item.school }}, <i>{{ item.location }}</i></p>
              <p class="secondary">{{ item.startDate | formatDate }} - {{ item.endDate | formatDate }}</p>
              <p>{{ item.description }}</p>
            </article>
          </section>
        </section>
        
        <section class="layout-section">
          <p class="accent">EXHIBITIONS</p>
          <section class="layout-block">
            <article v-for="exhibition of exhibitions"
                     id="exhibition.id"
                     class="layout-item">
              <p class="label"><b>{{ exhibition.showTitle }}</b> - {{ exhibition.dateStart | dateToYear }}</p>
              <p>{{ exhibition.description }}</p>
            </article>
          </section>
        </section>

        <section class="layout-section">
          <p class="accent">AWARDS</p>
          <ul class="layout-block">
            <li v-for="award of awards"
                     id="award.id">
              <a :href="award.link">
                <p class="label"><b>{{ award.title }}</b></p>
              </a>
            </li>
          </ul>
        </section>

        <section class="layout-section">
          <p class="accent">SKILLS</p>
          <ul class="layout-row">
            <li><p class="secondary">Photoshop</p></li>
            <li><p class="secondary">Illustrator</p></li>
            <li><p class="secondary">Autocad</p></li>
            <li><p class="secondary">Figma</p></li>
            <li><p class="secondary">Intermediate woodworking</p></li>
            <li><p class="secondary">Paper making</p></li>
            <li><p class="secondary">Screenprinting</p></li>
            <li><p class="secondary">Vinyl Cutter</p></li>
            <li><p class="secondary">Laser Cutter</p></li>
          </ul>
        </section>

      </main>

      <aside class="layout-page">
        <section id="contact" class="layout-section">
          <article class="layout-item">
            <p class="label"><b>email</b></p>
            <a href="mailto:joelnotlithgow@gmail.com"><p>joelnotlithgow@gmail.com</p></a>
          </article>
          <article class="layout-item">
            <p class="label"><b>instagram</b></p>
            <a href="https://www.instagram.com/joellithgow"target="_blank" rel="noreferrer noopener"><p>joellithgow</p></a>
          </article>
        </section>

        <!-- don't actually eed this download yet
        <hr></hr>

        <section class="layout-section">
          <button class="secondary">Download PDF</button>
        </section>
        -->
      </aside>


    </div>


  </div>
</template>



<script>
export default {
  async asyncData({ $content, error }) {
    try {

      const experiences = await $content( "experiences" )
        .where({ published: true })
        .only([ "id", "title", "description", "organization", "location", "slug", "startDate", "endDate"  ])
        .sortBy( "startDate", "asc" )
        .fetch()

      const awards = await $content( "awards" )
        .where({ published: true })
        .only([ "id", "title", "link", "slug", "date" ])
        .sortBy( "date", "desc" )
        .fetch()

      const education = await $content( "education" )
        .where({ published: true })
        //.only([ "id", "title", "slug", "startDate", "endDate"  ])
        .sortBy( "startDate", "asc" )
        .fetch()

      const exhibitions = await $content( "exhibitions" )
        .where({ published: true })
        //.only([ "showTitle", "calendar", "description", "featured", "slug", "galleryName", "dateStart", "dateEnd", "location" ])
        .sortBy( "dateStart", "desc" )
        .fetch()

      return { experiences, exhibitions, awards, education }

    } catch ( err ) {
      error({
        statusCode: 404,
        message: "Page could not be found or could it?",
      })
    }
  },
  filters: {
    formatDate: function ( date ) {
      const options = { year: "numeric", month: "numeric" };
      let formatted =  new Date( date ).toLocaleDateString( "en-us", options );

      if ( formatted != "Invalid Date") {
        return formatted;
      } else {
        return date;
      }
    },
    dateToYear: function( date ) {
      const options = { year: "numeric" };
      return new Date( date ).toLocaleDateString( "en", options );
    }
  },
}
</script>



<style scoped>
#resume {
  box-shadow: 0 2px 4px 0 rgba(0,0,0,0.5);
  background-color: white;
  border-radius: 8px;
  border: 1px solid lightgray;
}

#contact a {
  text-decoration: none;
}

aside.layout-page {
  position: sticky;
}

hr {
  border: 1px solid darkred;
  opacity: 16%;
}

p.detail {
  font-size: 1rem;
  color: darkslategray;
}

.title {
  font-size: 2.125rem;
}

.label {
  font-size: 1.125rem;
  font-family: Sans-serif;
}

p.secondary {
  font-size: 1rem;
  font-family: Sans-serif;
  color: darkslategray;
}

p.accent {
  font-size: .8125rem;
  font-family: Sans-serif;
  font-weight: bold;
  color: darkred; /* darkred, darkgreen, brown, */
}

ul {
  padding-left: 16px;
}

.layout-row {
  display: flex;
  flex-flow: wrap;
  gap: 16px;
  list-style: none;
}

.inline-row {
  display: flex;
  gap: 8px;
  align-items: center;
  /* flex-grow: 1; */
}
</style>
