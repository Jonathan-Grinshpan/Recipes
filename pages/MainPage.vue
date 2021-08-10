<template>
  <div class="container">
    
    <RecipePreviewList title="Explore these Recipes" class="RandomRecipes center" ref="RecipePreviewList" />
    <div>
   
    </div>
      <b-button @click="random"

        type="submit"
        variant="primary"
        style="width:100px;display:block;"
       class="mx-auto w-50"
        >get random recipes</b-button
      >
    <recipeLastSeen
      title="you have recently viewed"
      :class="{
        RandomRecipes: true,
        blur: !$root.store.username,
        center: true
      }"
      disabled
    >
    </recipeLastSeen>
    <!-- <div
      style="position: absolute;top: 70%;left: 50%;transform: translate(-50%, -50%);"
    >
      Centeredasdasdad
    </div> -->
  </div>
</template>

<script>
import RecipePreviewList from "../components/RecipePreviewList";
import recipeLastSeen from "../components/recipeLastSeen";
export default {
  components: {
    RecipePreviewList,
    recipeLastSeen

  },
  methods: {
    async random() {
     
      try {
       this.$refs.RecipePreviewList.updateRecipes();
      } catch (error) {
        console.log(error);
      }
    }
  }
};
</script>

<style lang="scss" scoped>
.RandomRecipes {
  margin: 10px 0 10px;
}
.blur {
  -webkit-filter: blur(5px); /* Safari 6.0 - 9.0 */
  filter: blur(2px);
}
::v-deep .blur .recipe-preview {
  pointer-events: none;
  cursor: default;
}
</style>
