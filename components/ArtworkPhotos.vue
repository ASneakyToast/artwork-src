<template>
  <img :srcset="require( `~/assets/artwork/${ artwork.slug }/photo-${ photoNumber }.jpg` ).srcSet"
        :alt="artwork.alt"
        :title="artwork.alt"
        @click="changePhoto()"/>
</template>



<script>
export default {
  props: [ 'artwork' ],
  data: () => {
    return {
      photoNumber: 1
    }
  },
  methods: {
    changePhoto: function() {
      if ( this.photoNumber >= this.artwork.photoAmount ) {
        this.photoNumber = 1;
      } else {
        this.photoNumber++;
      }
    }
  }
}
</script>
