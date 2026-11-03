<template>
  <div id="app" class="container">

<h1>Random Quote Generator</h1>

<!--Display the quote or a loading message-->

<p v-if="loading">Loading...</p>

<p v-else>"{{ quote }}"</p>

<p class="author">—{{ author }}</p>

<!--The button that triggers the API call-->

<button @click="getQuote">Get New Quote</button>

</div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'App',
  data() {
    return {
      quote: '',    // stores the quote text
      author: '',   // stores the author text
      loading: false // tracks loading status
    }
  },
  methods: {
    async getQuote() {
      this.loading = true
      try {
        // Send a GET request to the Quotes API
        const response = await axios.get('https://quotes15.p.rapidapi.com/quotes/random/?language_code=en', {
          headers: {
            'x-rapidapi-host': 'quotes15.p.rapidapi.com',
            'x-rapidapi-key': '[YOUR X-RapidAPI-Key from Step 2 above]' 
          }
        })
        
        // Update the quote with the response data
        this.quote = response.data.content
        this.author = response.data.originator.name
      } catch (error) {
        console.error('Error fetching quote:', error)
        this.quote = 'Oops! Something went wrong.'
        this.author = ''
      } finally {
        this.loading = false
      }
    }
  },
  mounted() {
    // Load a random quote when the app first starts
    this.getQuote()
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.container {
  text-align: center;
  font-family: Arial, sans-serif;
  margin-top: 50px;
}
button {
  margin-top: 20px;
  padding: 10px 20px;
  font-size: 16px;
}
.author {
  font-style: italic;
  color: #555;
  margin-top: 5px;
}

</style>
