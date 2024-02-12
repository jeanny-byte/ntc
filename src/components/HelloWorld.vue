<template>
  <img class="ntclogo" src="../assets/ntclogo.png" />
  <div>
    <div v-if="!testStarted">
      <div class="quiz-rules">
        <p>Welcome to our quiz! Here's how it works:</p>
        <ul type="none">
          <li>
            Answer: Choose the option you think is correct for each question. If
            you're unsure, take your best guess!<br>
            Passing Grade: To pass the quiz, you'll need to score 80% or higher.<br>
  
          </li>
        </ul>
      </div>

      <div class="popup">
        <h2>Welcome to NTC Training Module Test</h2>
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
      <div v-if="currentIndex < questionsToDisplay.length" class="carousel-container">
  <h2 class="question-title">
    {{ currentIndex + 1 }}. {{ questionsToDisplay[currentIndex].text }}
  </h2>
  <ul class="options-list">
    <li
      v-for="(option, index) in questionsToDisplay[currentIndex].options"
      :key="index"
      class="option-item"
    >
      <input
        type="radio"
        :id="'option' + index"
        :value="option"
        v-model="selectedAnswer"
        class="option-input"
      />
      <label :for="'option' + index" class="option-label">{{ option }}</label>
    </li>
  </ul>
  <button @click="nextQuestion" class="next-button">Next</button>
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
      highlightCorrectAnswer: false,
      userName: "",
      selectedCategory: "",
      testStarted: false,
      currentIndex: 0,
      selectedAnswer: null,
      score: 0,
      categories: [
        { id: 1, name: "category1" },
        { id: 2, name: "category2" },
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
          {
            text: "Question 2 for Category 1?",
            options: ["Option A", "Option B", "Option C"],
            correctAnswer: "Option B",
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
      },
      questionsToDisplay: [], // Initialize questionsToDisplay as an empty array
    };
  },
  methods: {
    playCorrectSound() {
      const audio = new Audio(require('../assets/correct.mp3'));
      audio.play();
    },
    
    playIncorrectSound() {
      const audio = new Audio(require('../assets/incorrect.mp3'));
      audio.play();
    },
    startTest() {
  if (this.userName && this.selectedCategory) {
    // Find the selected category object by its ID
    const selectedCategory = this.categories.find(category => category.id === this.selectedCategory);
    if (selectedCategory) {
      // Select questions based on the chosen category name
      this.questionsToDisplay = this.questions[selectedCategory.name];
      this.testStarted = true;
    } else {
      alert("Invalid category selected. Please choose a valid category.");
    }
  } else {
    alert("Please enter your name and select a category to start the test.");
  }
},


nextQuestion() {
  // Check if the selected answer is correct
  const correctAnswer = this.questionsToDisplay[this.currentIndex].correctAnswer;
  if (this.selectedAnswer !== correctAnswer) {
    // Play incorrect sound
    this.playIncorrectSound();

    // Set highlightCorrectAnswer to true to indicate the correct answer should be highlighted
    this.highlightCorrectAnswer = true;
    // Move to the next question after 5 seconds
    setTimeout(() => {
      this.currentIndex++;
      // Reset highlightCorrectAnswer to false after moving to the next question
      this.highlightCorrectAnswer = false;
    }, 5000);
  } else {
    // Play correct sound
    this.playCorrectSound();
    
    // Move to the next question if the answer is correct
    this.currentIndex++;
  }
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

.ntclogo{
  height: 15%;
  width: 15%;
}
.carousel-container {
  background-color: #f8f9fa;
  border-radius: 10px;
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.1);
  padding: 20px;
  max-width: 600px;
  margin: 0 auto;
  text-align: center;
}

.question-title {
  margin-bottom: 20px;
  font-size: 24px;
  color: #333;
}

.options-list {
  list-style: none;
  padding: 0;
}

.option-item {
  margin-bottom: 10px;
}

.option-input {
  margin-right: 10px;
}

.option-label {
  font-size: 16px;
}

.next-button {
  background-color: #007bff;
  color: #fff;
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.next-button:hover {
  background-color: #0056b3;
}

@media (max-width: 768px) { /* Adjust styles for screens smaller than 768px */
  .carousel-container {
    padding: 10px; /* Reduce padding */
  }
  
  .question-title {
    font-size: 20px; /* Decrease font size */
  }

  .option-label {
    font-size: 14px; /* Decrease font size */
  }
}

.popup {
  background-color: #f8f9fa;
  border-radius: 10px;
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.1);
  padding: 20px;
  max-width: 400px;
  margin: 0 auto;
  text-align: center;
}

.popup h2 {
  margin-bottom: 20px;
  font-size: 24px;
  color: #333;
}

.popup input[type="text"],
.popup select {
  width: 100%;
  padding: 10px;
  margin-bottom: 20px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

.popup button {
  background-color: #007bff;
  color: #fff;
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.popup button:hover {
  background-color: #0056b3;
}
.quiz-rules {
  color: red;
  font-size: 11px;

}
</style>
