<template>
  <img src="../assets/ntclogo.png" />
  <div>
    <div v-if="!testStarted">
      <div class="quiz-rules">
        <p>Welcome to our quiz! Here's how it works:</p>
        <ul type="none">
          <li>
            Start: To begin, enter your name and select a category of questions
            you'd like to be tested on.
          </li>
          <li>
            Test: Once you start the test, you'll be presented with a series of
            multiple-choice questions.
          </li>
          <li>
            Answer: Choose the option you think is correct for each question. If
            you're unsure, take your best guess!
          </li>
          <li>
            Feedback: If you select the correct answer, well done! If not, don't
            worry. The correct answer will be revealed before moving to the next
            question.
          </li>
          <li>
            Scoring: Your score will be calculated based on the number of
            correct answers.
          </li>
          <li>
            Passing Grade: To pass the quiz, you'll need to score 80% or higher.
          </li>
          <li>
            Result: After answering all questions, you'll receive immediate
            feedback on whether you passed or need to try again.
          </li>
          <li>
            Good Luck!: Best of luck with the quiz. Have fun and test your
            knowledge!
          </li>
        </ul>
      </div>

      <!-- Initial Popup -->
      <div class="popup">
        <h2>Welcome to the Test</h2>
        <input type="text" placeholder="Enter your name" v-model="userName" />
        <select v-model="selectedCategory">
          <option value="">Select a category</option>
          <option
            v-for="category in categories"
            :key="category.id"
            :value="category.id"
          >
            {{ category.name }}
          </option>
        </select>
        <button @click="startTest">Start Test</button>
      </div>
    </div>
    <div v-else>
      <!-- Question Carousel -->
      <div v-if="currentIndex < questions.length">
        <h2>{{ currentIndex + 1 }}. {{ questions[currentIndex].text }}</h2>
        <ul>
          <li
            v-for="(option, index) in questions[currentIndex].options"
            :key="index"
          >
            <input
              type="radio"
              :id="'option' + index"
              :value="option"
              v-model="selectedAnswer"
            />
            <label :for="'option' + index">{{ option }}</label>
          </li>
        </ul>
        <button @click="nextQuestion">Next</button>
      </div>
      <div v-else>
        <!-- Scoring and Feedback Popup -->
        <div class="popup">
          <h2 v-if="score >= 80">Congratulations, {{ userName }}!</h2>
          <h2 v-else>Sorry, {{ userName }}, you didn't pass.</h2>
          <button @click="resetTest">Close</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      userName: "",
      selectedCategory: "",
      testStarted: false,
      currentIndex: 0,
      selectedAnswer: null,
      score: 0,
      categories: [
        { id: 1, name: "Category 1" },
        { id: 2, name: "Category 2" },
        // Add more categories here
      ],
      questions: {
        // Define questions for each category
        category1: [
          {
            text: "Question 1 for Category 1?",
            options: ["Option A", "Option B", "Option C"],
            correctAnswer: "Option A",
          },
          // Add more questions for Category 1 here
        ],
        category2: [
          {
            text: "Question 1 for Category 2?",
            options: ["Option A", "Option B", "Option C"],
            correctAnswer: "Option B",
          },
          // Add more questions for Category 2 here
        ],
        // Add more categories and questions here
      },
    };
  },
  methods: {
    startTest() {
      if (this.userName && this.selectedCategory) {
        // Select questions based on the chosen category
        this.questionsToDisplay = this.questions[this.selectedCategory];
        this.testStarted = true;
      } else {
        alert(
          "Please enter your name and select a category to start the test."
        );
      }
    },
    nextQuestion() {
      if (
        this.selectedAnswer ===
        this.questionsToDisplay[this.currentIndex].correctAnswer
      ) {
        this.score++;
      }
      this.currentIndex++;
    },
    resetTest() {
      this.userName = "";
      this.selectedCategory = "";
      this.testStarted = false;
      this.currentIndex = 0;
      this.selectedAnswer = null;
      this.score = 0;
      this.questionsToDisplay = [];
    },
  },
};
</script>

<style scoped>
/* Component-specific styles */
.popup {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background: #fff;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 5px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}
.quiz-rules {
  color: red;
  font-size: 11px;
}
</style>
