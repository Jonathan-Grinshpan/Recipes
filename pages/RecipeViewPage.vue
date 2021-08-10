<template>
  <div class="container">
    <div v-if="recipe">
      <div class="recipe-header mt-3 mb-4">
        <h1>{{ recipe.title }}</h1>
        <img :src="recipe.image" class="center" />
      </div>
      <div class="recipe-body">
        <div class="wrapper">
          <div class="wrapped">
            <div class="mb-3">
                 
              <div><b><u>Time to make :</u></b>  {{ recipe.readyInMinutes }} minutes</div>
              <div><b><u>number of likes :</u></b> {{ recipe.aggregateLikes }} likes</div>
                 <div><b><u>is this vegan? :</u></b> {{ recipe.vegan }} </div>
                 <div><b><u>is this vegeterian? :</u></b> {{ recipe.vegan }} </div>
                  <div><b><u>is this gluten free? :</u></b> {{ recipe.gluten_free }} </div>
                
            </div>
               <span v-if="$root.store.username" class="brr">
              <div><button @click="favor">Add this to favorite recipes?</button> </div>
               
                <div><button @click="person">Add this to personal recipes?</button> </div>
                   </span>
                  <br>
            <b><u>Ingredients:</u></b>
            <ul>
              <li
                v-for="(r, index) in recipe.ingredients"
                :key="index + '_' + r.id"
              >
                {{ r.original }}
              </li>
            </ul>
          </div>
      
            
          <div class="wrapped">
           <b><u> Instructions:</u></b>
            <ol>
              <li v-for="s in recipe._instructions" :key="s.number">
                {{ s.step }}
              </li>
            </ol>
          </div>
          
        </div>
      </div>
      <!-- <pre>
      {{ $route.params }}
      {{ recipe }}
    </pre
      > -->
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      recipe: null
    };
  },
  async created() {
    try {
     
      let response;
      // response = this.$route.params.response;

      try {
        response = await this.axios.get(
          "http://localhost:3000/recipes/Information",
          {
            params: { recipe_id: this.$route.params.recipeId }
            
          }
        );

        // console.log("response.status", response.status);
        if (response.status !== 200) this.$router.replace("/NotFound");
      } catch (error) {
        console.log("error.response.status", error.response.status);
        this.$router.replace("/NotFound");
        return;
      }

      let {
         analyzedInstructions,
         instructions, 
        extendedIngredients,
        aggregateLikes,
        readyInMinutes,
        image,
        vegan,
        ingredients,
        vegeterian,
        title,
        gluten_free
      } = response.data;
  
      let _instructions = instructions
        .map((fstep) => {
          fstep.steps[0].step = fstep.name + fstep.steps[0].step;
          return fstep.steps;
        })
        .reduce((a, b) => [...a, ...b], []);

      let _recipe = {
        instructions,
        _instructions,
        analyzedInstructions,
        extendedIngredients,
        aggregateLikes,
          ingredients,
        readyInMinutes,
        image,
        vegan,
        vegeterian,
        title,
        gluten_free
      };

      this.recipe = _recipe;
    } catch (error) {
      console.log(error);
    }
  },
  methods: {
     async favor(){
        try {
           const response = await this.axios.post(
          
          "http://localhost:3000/recipes/add_favorite_recipie",{
          
            recipe_id: this.$route.params.recipeId
          
          
          }
        );
   
   this.$root.toast( "recipe added to favorites", "success");
    
    } catch (error) {
      console.log(error);
    }
  },
  async person(){
        try {
           const response = await this.axios.post(
          
          "http://localhost:3000/profile/add_personal_recipe",{
          
            recipe_id: this.$route.params.recipeId
          
          
          }
        );
   
   this.$root.toast( "recipe added to personal", "success");
    
    } catch (error) {
      console.log(error);
    }
  }
}
}
</script>

<style scoped>
.wrapper {
  display: flex;
}
.wrapped {
  width: 50%;
}
.center {
  display: block;
  margin-left: auto;
  margin-right: auto;
  width: 50%;
}
brr {
   display: block;
   margin: 10px 0;
}
/* .recipe-header{

} */
</style>
