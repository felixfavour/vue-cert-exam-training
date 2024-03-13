<script setup>
import {ref, watch} from 'vue'
const questions = [
  {
    question: "What is the capital of France?",
    options: ["London", "Berlin", "Paris", "Rome"],
    answer: "Paris",
  },
  {
    question: "Which planet is closest to the sun?",
    options: ["Earth", "Mars", "Venus", "Mercury"],
    answer: "Mercury",
  },
  // Add more questions as needed
];
const optionSelected = ref('')
const currentQuestionIndex = ref(0)
const passedQuestions = ref([])
const submittedQuestions = ref([])
const scoreScreenVisible = ref(false)

const submitQuestion = () => {
  // check if current option selected on question is correct
  const currentQuestion = questions?.at(currentQuestionIndex.value)
  if (currentQuestion.answer === optionSelected.value) {
    passedQuestions.value.push(currentQuestionIndex.value)
  }
  submittedQuestions.value.push(currentQuestionIndex.value)
}

const nextQuestion = () => {
  if (currentQuestionIndex.value === questions.length - 1) {
    scoreScreenVisible.value = true
  }
  currentQuestionIndex.value += 1
}
</script>

<template>
  <div class="quiz-ctn text-white max-w-[400px] mx-auto mt-80">
    <div v-for="(question, index) in questions" :key="question.question"  class="question-ctn">
      <template v-if="index === currentQuestionIndex">
        <div class="question text-xl border-b-2 border-gray-400">
          {{ question.question }}
        </div>
        <div class="options-ctn">
          <label v-for="option in question.options" :key="option" :for="option" class="block">
            <input type="radio" v-model="optionSelected" :value="option" :id="option" :disabled="submittedQuestions.includes(index)">
            {{ option }}
          </label>
        </div>
        <div class="flex gap-2 items-center">
          <button v-if="!submittedQuestions.includes(index)" :disabled="!optionSelected" class=" bg-blue-500 px-4 py-1 mt-6" @click="submitQuestion">Submit</button>
          <button v-else :disabled="!optionSelected" class=" bg-blue-500 px-4 py-1 mt-6" @click="nextQuestion">Next</button>

          <h3 v-show="passedQuestions?.includes(index)" class="mt-8 text-green-300">Correct</h3>
          <h3 v-show="!passedQuestions?.includes(index) && submittedQuestions.includes(index)" class="mt-8 text-red-300">Fail</h3>
        </div>
      </template>
      
    </div>
    <div v-if="scoreScreenVisible" class="score-screen text-center text-2xl text-bold">
      Your final score: {{  passedQuestions.length }} out of {{ questions.length  }}
    </div>
  </div>
</template>
