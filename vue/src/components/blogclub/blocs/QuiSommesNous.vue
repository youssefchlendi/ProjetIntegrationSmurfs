<template>
  <div id="quisommesnous">
    <article class="blog-post">
      <h2 class="blog-post-title">
        Qui sommes-nous ?(hedhii description détaillée lel club)
      </h2>
      <br />
      <p v-for="(item, index) in abouts" :key="index">
        {{item.longDescription}}
      </p>
    </article>
  </div>
</template>
<script>
export default {
  props: {
    id: {
      type: Number,
      required: true
    }
  },
  name: "QuiSommesNous",
  data(){
    return {
      abouts:[],
    }
  },
  created(){
    this.$http.get('http://localhost:8000/api/pc/about/'+this.id).then(response => {
      this.abouts = response.data
     
    })
 
  },


};
</script>
