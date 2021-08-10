<template>
  <div class="container">
    <h1 class="title">Search for recipes</h1>
    <b-form  @reset.prevent="onReset">
    
      <b-form-group
        id="input-group-cuisine"
        label-cols-sm="3"
        label="cuisine:"
        label-for="cuisine"
      >
        <b-form-select
          id="cuisine"
          v-model="$v.form.cuisine.$model"
          :options="Cuisine"
          value="Italian"
        :state="validateState('cuisine')"
        ></b-form-select>
        <b-form-invalid-feedback>
          cuisine is required
        </b-form-invalid-feedback>
      </b-form-group>

       <b-form-group
        id="input-group-diet"
        label-cols-sm="3"
        label="diet:"
        label-for="diet"
      >
        <b-form-select
          id="diet"
          v-model="$v.form.diet.$model"
          :options="Diet"
        :state="validateState('diet')"
        ></b-form-select>
        <b-form-invalid-feedback>
          diet is required
        </b-form-invalid-feedback>
      </b-form-group>

       <b-form-group
        id="input-group-into"
        label-cols-sm="3"
        label="Intolerance:"
        label-for="into"
      >
        <b-form-select
          id="into"
          v-model="$v.form.into.$model"
          :options="Intolerances"
        :state="validateState('into')"
        ></b-form-select>
        <b-form-invalid-feedback>
          Intolerance is required
        </b-form-invalid-feedback>
      </b-form-group>


 <b-form-group
        id="input-group-food"
        label-cols-sm="3"
        label="food:"
        label-for="food"
      >
        <input
          id="food"
      
        value="Pizza"
        required = true
        >
 </b-form-group>
       

       <select id="options">
  <option value="5">5</option>
  <option value="10" >10</option>
  <option value="15">15</option>
</select>

     
      <b-button
      @click="Search"
        type="submit"
        variant="primary"
        style="width:250px;"
        class="ml-5 w-75"
        >Search</b-button
      >
     
    </b-form>
   <b-row cols-lg="5">
     
      <b-col v-for="r in recipes" :key="r.id" >
        <RecipePreview class="recipePreview" :recipe="r" />
        
      </b-col>
    </b-row>
    

   
    <!-- <b-card class="mt-3 md-3" header="Form Data Result">
      <pre class="m-0"><strong>form:</strong> {{ form }}</pre>
      <pre class="m-0"><strong>$v.form:</strong> {{ $v.form }}</pre>
    </b-card> -->
  </div>
</template>

<script>
import RecipePreview from "../components/RecipePreview.vue";
import Diet from "../assets/Diet"
import Cuisine from "../assets/Cuisine";
import Intolerances from "../assets/Intolerances"
import {
  required,
  minLength,
  maxLength,
  alpha,
  sameAs,
  email
} from "vuelidate/lib/validators";

export default {
  name: "Search",
  components: {
    RecipePreview
  },
  data() {
    return {
      form: {
        
        cuisine: "Italian",
        diet: "Vegetarian",
        into: "Soy",
       
      },
       recipes: [],
      Intolerances: [{ value: null, text: "", disabled: true }],
      Diet: [{ value: null, text: "", disabled: true }],
      Cuisine: [{ value: null, text: "", disabled: true }],
      errors: [],
      validated: false
    };
  },
  validations: {
    form: {
    
      cuisine: {
        required
      },
      diet: {
        required
      },
      into: {
        required
      },
      food: {
        required
      },
    
    }
  },
  mounted() {
    // console.log("mounted");
    this.Cuisine.push(...Cuisine);
    this.Diet.push(...Diet);
    this.Intolerances.push(...Intolerances);
    // console.log($v);
  },
  methods: {
    validateState(param) {
      const { $dirty, $error } = this.$v.form[param];
      return $dirty ? !$error : null;
    },
    async Search() {
      
       try {
      
        const response = await this.axios.get(
            `http://localhost:3000/recipes/search?food=${food.value}&cuisine=${cuisine.value}&amount=${options.value}&intolerence=${into.value}&diet=${diet.value}`,
     
        );

        // console.log(response);
        const recipes = response.data.ans;
      
        this.recipes = [];
        this.recipes.push(...recipes);
        // console.log(this.recipes);
      } catch (error) {
        console.log(error);
      }
    },
    
    onReset() {
      this.form = {
        
        cuisine: null,
        diet: null,
        into: null,
       
      };
      this.$nextTick(() => {
        this.$v.$reset();
      });
    }
  }
};
</script>
<style lang="scss" scoped>
.container {
 
  margin-top: 50px;
}
</style>
