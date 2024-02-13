<template>
  <div id="adsgoeshere" v-html="adsenseContent"></div>

  <div class="container">
    <!-- Add the score display -->
    <div class="score-circle">{{ score }}</div>

    <!-- Rest of the template code -->
  </div>
  <img class="ntclogo" src="../assets/ntclogo.png" />
  <div>
    <div v-if="!testStarted">
      <div class="quiz-rules">
        <p>
          Welcome to the NTC Non-Professional teachers quize! Here's how it
          works:
        </p>
        <ul type="none">
          <li>
            Answer: Choose the option you think is correct for each question. If
            you're unsure, take your best guess!<br />
            Passing Grade: To pass the quiz, you'll need to score 80% or
            higher.<br />
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
      <div
        v-if="currentIndex < questionsToDisplay.length"
        class="carousel-container"
      >
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
            <label :for="'option' + index" class="option-label">{{
              option
            }}</label>
          </li>
        </ul>
        <button @click="nextQuestion" class="next-button">Next</button>
        <!--Hidden div-->
        <div v-if="showCorrectAnswer" class="correct-answer">
          Correct answer:
          <span style="color: green; font-weight: bold">{{
            questionsToDisplay[currentIndex].correctAnswer
          }}</span>
        </div>
      </div>

      <div v-else>
        <!-- Scoring and Feedback Popup -->
        <div class="popup">
          <h2 v-if="score >= 17">Congratulations, {{ userName }}!</h2>
          <h2 v-else>Sorry, {{ userName }}, you didn't pass.</h2>
          <button @click="resetTest">Close</button>
        </div>
      </div>
    </div>
  </div>
  <!-- Thumbs-up icon -->
  <div v-if="showThumbsUp" class="thumbs-up">👍</div>

  <!-- Thumbs-down icon -->
  <div v-if="showThumbsDown" class="thumbs-down">👎</div>

  <div class="footer-bottom">
          <p>&copy; 2024 Maktechit. All rights reserved.</p>
          <p>Designed by <a href="#" target="_blank">Jean-Claude (.Com)</a></p>
        </div>
</template>

<script>
export default {
  data() {
    return {
      aadsenseContent: "",
      consecutiveCorrectAnswers: 0, // Track consecutive correct answers
      consecutiveIncorrectAnswers: 0, // Track consecutive incorrect answers
      showThumbsUp: false,
      showThumbsDown: false,
      highlightCorrectAnswer: false,
      showCorrectAnswer: false, // Add this line
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
          {
            text: "Question 3 for Category 1?",
            options: ["Option A", "Option B", "Option C"],
            correctAnswer: "Option C",
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
      totalRounds: 3, // Define total number of rounds
    };
  },

  methods: {
    incrementScore() {
      this.score++;
    },
    // Function to increment consecutive correct answers and reset consecutive incorrect answers
    incrementConsecutiveCorrect() {
      this.consecutiveCorrectAnswers++;
      this.consecutiveIncorrectAnswers = 0;
    },

    // Function to increment consecutive incorrect answers and reset consecutive correct answers
    incrementConsecutiveIncorrect() {
      this.consecutiveIncorrectAnswers++;
      this.consecutiveCorrectAnswers = 0;
    },
    // Function to play a random audio file from the "cheer" folder
    playRandomCheerAudio() {
      const folder = "cheer";
      const audioFiles = [
        require(`../assets/sounds/${folder}/Ala Bossu.aac`),
        require(`../assets/sounds/${folder}/nosize.aac`),
      ];
      const randomIndex = Math.floor(Math.random() * audioFiles.length);
      const audio = new Audio(audioFiles[randomIndex]);
      audio.play();
    },

    // Function to play a random audio file from the "motivate" folder
    playRandomMotivateAudio() {
      const folder = "motivate";
      const audioFiles = [
        require(`../assets/sounds/${folder}/suffer.aac`),
        require(`../assets/sounds/${folder}/tire.aac`),
        require(`../assets/sounds/${folder}/try.aac`),
      ];
      const randomIndex = Math.floor(Math.random() * audioFiles.length);
      const audio = new Audio(audioFiles[randomIndex]);
      audio.play();
    },

    checkConsecutiveCorrectAnswers() {
      if (this.consecutiveCorrectAnswers === 2) {
        // Change to 2 as it starts from 0
        this.playRandomCheerAudio();
      }
    },

    // Function to check for three consecutive incorrect answers
    checkConsecutiveIncorrectAnswers() {
      if (this.consecutiveIncorrectAnswers === 2) {
        // Change to 2 as it starts from 0
        this.playRandomMotivateAudio();
      }
    },

    playCorrectSound() {
      const audio = new Audio(require("../assets/sounds/correct.mp3"));
      audio.play();
    },

    playIncorrectSound() {
      const audio = new Audio(require("../assets/sounds/incorrect.mp3"));
      audio.play();
    },
    showThumbsUpAnimation() {
      this.showThumbsUp = true;
      setTimeout(() => {
        this.showThumbsUp = false;
      }, 2000); // Adjust the duration as needed
    },

    showThumbsDownAnimation() {
      this.showThumbsDown = true;
      setTimeout(() => {
        this.showThumbsDown = false;
      }, 2000); // Adjust the duration as needed
    },
    startTest() {
      if (this.userName && this.selectedCategory) {
        // Find the selected category object by its ID
        const selectedCategory = this.categories.find(
          (category) => category.id === this.selectedCategory
        );
        if (selectedCategory) {
          // Select questions based on the chosen category name
          this.questionsToDisplay = this.questions[selectedCategory.name];
          this.testStarted = true;
        } else {
          alert("Invalid category selected. Please choose a valid category.");
        }
      } else {
        alert(
          "Please enter your name and select a category to start the test."
        );
      }
    },

    nextQuestion() {
      // Check if the selected answer is correct
      const correctAnswer =
        this.questionsToDisplay[this.currentIndex].correctAnswer;
      if (this.selectedAnswer !== correctAnswer) {
        // Play incorrect sound
        this.playIncorrectSound();

        // Show thumbs-down animation
        this.showThumbsDownAnimation();

        // Show the correct answer
        this.showCorrectAnswer = true;

        // Move to the next question after 5 seconds
        setTimeout(() => {
          // Hide thumbs-down animation and the correct answer after 5 seconds
          this.showThumbsDown = false;
          this.showCorrectAnswer = false;

          // Move to the next question
          this.currentIndex++;

          // Increment consecutive incorrect answers
          this.incrementConsecutiveIncorrect();

          // Call function to check for consecutive incorrect answers
          this.checkConsecutiveIncorrectAnswers();
        }, 5000);
      } else {
        // Increment score
        this.incrementScore();

        // Play correct sound
        this.playCorrectSound();

        // Show thumbs-up animation
        this.showThumbsUpAnimation();

        // Move to the next question if the answer is correct
        this.currentIndex++;

        // Increment consecutive correct answers
        this.incrementConsecutiveCorrect();

        // Call function to check for consecutive correct answers
        this.checkConsecutiveCorrectAnswers();
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
    shuffleQuestions(questions) {
      // Shuffle the questions array
      for (let i = questions.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [questions[i], questions[j]] = [questions[j], questions[i]];
      }
      return questions;
    },
  },
  mounted() {
    this.adsenseContent = document.getElementById(
      "adsgoeshere"
    ).innerHTML;
  },
};
</script>

<style scoped>
.container {
  position: relative;
}

.score-circle {
  position: fixed;
  top: 20px;
  right: 20px;
  width: 50px;
  height: 50px;
  background-color: green;
  color: white;
  border-radius: 50%;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 18px;
  font-weight: bold;
  animation: float 2s infinite alternate;
}

@keyframes float {
  0% {
    transform: translateY(0);
  }
  100% {
    transform: translateY(-20px);
  }
}
.correct-answer {
  margin-top: 10px; /* Adjust spacing as needed */
}

/* Thumbs-up animation */
@keyframes thumbsUp {
  0% {
    transform: translateY(0);
    opacity: 1;
  }
  50% {
    transform: translateY(-50px);
    opacity: 1;
  }
  100% {
    transform: translateY(-100px);
    opacity: 0;
  }
}

.thumbs-up {
  position: fixed;
  bottom: 20px;
  right: 20px;
  font-size: 30px;
  color: green;
  animation: thumbsUp 2s ease-out;
}

/* Thumbs-down animation */
@keyframes thumbsDown {
  0% {
    transform: translateY(0);
    opacity: 1;
  }
  50% {
    transform: translateY(-50px);
    opacity: 1;
  }
  100% {
    transform: translateY(-100px);
    opacity: 0;
  }
}

.thumbs-down {
  position: fixed;
  bottom: 20px;
  right: 20px;
  font-size: 30px;
  color: red;
  animation: thumbsDown 2s ease-out;
}
/* Component-specific styles */

.ntclogo {
  height: 15%;
  width: 15%;
}
.carousel-container {
  margin-top: 15px;
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

@media (max-width: 768px) {
  /* Adjust styles for screens smaller than 768px */
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
  margin-bottom: 30px;
}
.footer-bottom {
  margin-top: 20px;
  text-align: center;
}

.footer-bottom p {
  margin: 5px 0;
}

.footer-bottom a {
  color: rgb(72, 60, 60);
  text-decoration: none;
}
</style>
