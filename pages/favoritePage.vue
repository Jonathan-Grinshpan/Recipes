<template>
  <b-container>
    <h3>
         <h2 class="title">Favorite recipes </h2>
 
      <slot></slot>
    </h3>
   
    <b-row cols-lg="5">
      <b-col v-for="r in recipes" :key="r.id">
        <RecipePreview class="recipePreview" :recipe="r" />
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
import RecipePreview from "../components/RecipePreview.vue";
export default {
  name: "favoritePage",
  components: {
    RecipePreview
  },
 
  data() {
    return {
      recipes: []
    };
  },
  mounted() {
    this.updateRecipes();
  },
  methods: {
    async updateRecipes() {
      try {
        const response = await this.axios.get(
          "http://localhost:3000/recipes/get_favorite_recipie"
        );

        // console.log(response);
        const recipes = response.data.ans;
        this.recipes = [];
        this.recipes.push(...recipes);
        // console.log(this.recipes);
      } catch (error) {
        console.log(error);
      }
    }
  }
};
</script>

<style lang="scss" scoped>
.container {
  min-height: 400px;
   
}
</style>
