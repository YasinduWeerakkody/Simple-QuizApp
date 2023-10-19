<script setup>
import { ref, computed }from 'vue'

const questions = ref([
  {
    question: "What is the VueJs ?",
    answer: 0,
    options:[
      'A Front End Framework',
      'An Ice-cream',
      'A Library'
    ],
    selected:null
  },
  {
    question: "What is the Vuex ?",
    answer: 2,
    options:[
      'Vue with an x',
      'A cheese',
      'State management Library'
    ],
    selected:null
  },
  {
    question: "What is Vue Router used for",
    answer: 1,
    options:[
      'Walking a space',
      'A routing library for Vue JS',
      'Burger Sauce',
      'Quizzes'
    ],
    selected:null
  }
])


const quizeCompleted = ref(false)
const currentQuestion = ref(0)
const score = computed(() => {
  let value = 0 
  questions.value.map(q => {
    if(q.selected == q.answer) {
      value ++
    }
  })
  return value
})

const getCurrentQuestion = computed(() => {
  let question = questions.value[currentQuestion.value]
  question.index = currentQuestion.value
  return question
})

const SetAnswer = evt => {
  questions.value[currentQuestion.value].selected = evt.target.value
  evt.target.value = null
}

const NextQuestion = () => {
  if(currentQuestion.value < questions.value.length - 1){
    currentQuestion.value++
  }else{
    quizeCompleted.value = true
  }
}
</script>

<template>
  <!-- <header>
    <img alt="Vue logo" class="logo" src="@/assets/logo.svg" width="125" height="125" />

    <div class="wrapper">
      <HelloWorld msg="You did it!" />

      <nav>
        <RouterLink to="/">Home</RouterLink>
        <RouterLink to="/about">About</RouterLink>
      </nav>
    </div>
  </header> -->

  <main class="app">
    <h1>The Quize</h1>

    <section class="quize" v-if="!quizeCompleted">
      <div class="quize-info">
        <span class="question">
          {{ getCurrentQuestion.question }}
        </span>
        <span class="score">
          Score {{ score }}/{{ questions.length }}
        </span>
      </div>

      <div class="options">
        <label v-for="(option, index) in getCurrentQuestion.options" :key="index" :class="`option ${
          getCurrentQuestion.selected == index
          ? index == getCurrentQuestion.answer
          ?'correct'
          :'wrong'
          :''

        } ${
          getCurrentQuestion.selected != null &&
          index != getCurrentQuestion.answer
          ? 'disabled'
          :''
        }`">
          <input 
            type="radio"
            :name="getCurrentQuestion.index"
            :value="index"
            v-model="getCurrentQuestion.selected"
            :disabled="getCurrentQuestion.selected"
            @change="SetAnswer">
            <span>{{ option }}</span>
      </label>
      </div>

      <button @click="NextQuestion" :disabled="!getCurrentQuestion.selected">
        {{ 
          getCurrentQuestion.index == questions.length - 1
          ? 'finish'  
          :getCurrentQuestion.selected == null
          ? 'select an option'
          : 'Next Question'
        }}
      </button>
    </section>

    <section v-else>
      <h2>You have finished the quize</h2>
      <p>Your score is {{ score }} / {{ questions.length }}</p>
    </section>
  </main>
  
  <!-- <RouterView /> -->
</template>

<style>
/* header {
  line-height: 1.5;
  max-height: 100vh;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

nav {
  width: 100%;
  font-size: 12px;
  text-align: center;
  margin-top: 2rem;
}

nav a.router-link-exact-active {
  color: var(--color-text);
}

nav a.router-link-exact-active:hover {
  background-color: transparent;
}

nav a {
  display: inline-block;
  padding: 0 1rem;
  border-left: 1px solid var(--color-border);
}

nav a:first-of-type {
  border: 0;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }

  nav {
    text-align: left;
    margin-left: -1rem;
    font-size: 1rem;

    padding: 1rem 0;
    margin-top: 1rem;
  }
} */

*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: Verdana, Geneva, Tahoma, sans-serif;
}
body{
  background-color: #271C36;
  color: #fff;
}
.app{
  display: flex;
  align-items: center;
  flex-direction: column;
  padding: 2rem;
  min-height: 100vh;
}

h1{
  font-size: 2rem;
  margin-bottom: 3rem;
  
}
.quize{
  background-color: #cfb6f3;
  padding: 1rem;
  width: 100%;
  max-width: 640px;
  border-radius: 0.5rem;
}

.quize-info{
  display: flex;
  justify-content: space-between;
  margin-bottom: 1rem;

}

.quize-info .question{
  color: #8f8f8f;
  font-size: 1.25rem;
}

.quize-info .score{
  color: rgb(255, 255, 255);
  font-size: 1rem;
}
.options{
  margin-bottom: 1rem;
}
.option{
  display: block;
  padding: 1rem;
  background-color: #664c896b;
  border-radius: 0.5rem;
  margin-bottom: 0.5rem;
  cursor: pointer;
}
.option:hover{
  background-color: #271C36;
}
.option.correct{
  background-color: #3bf077;
}
.option.wrong{
  background-color: #f65353;
}
.option:last-of-type{
  margin-bottom: 0;
}
.option.disabled{
  opacity: 0.5;
}
.option input{
  display: none;
}
button{
  appearance: none;
  border: none;
  outline: none;
  cursor: pointer;

  padding:0.5rem 1rem;
  border-radius: 0.5rem;
  color:#ffffff;
  background-color: #49f849b5;
  font-weight: 700;
  text-transform: uppercase;
  font-size: 1rem;
}
/* button:disabled{
  opacity: 0.5;
} */
h2{
  font-size:1rem;
  margin-bottom: 2rem;
  text-align: center;
}
p{
  color: #8d8b90;
  font-size: 1rem;
  text-align: center;

}
</style>

