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
  <footer class="footer">
    <div class="container">
      <div class="footer-content">
        <div class="footer-social">
          <ul>
            <li>
              <a href="https://web.facebook.com/jean.amekudzi.5">
                <font-awesome-icon :icon="['fab', 'facebook-f']" />
              </a>
            </li>
            <!-- <li><a href="#"><font-awesome-icon icon="fab fa-twitter"></font-awesome-icon></a></li> -->
            <li>
              <a href="https://www.instagram.com/__.ccom">
                <font-awesome-icon :icon="['fab', 'instagram']" />
              </a>
            </li>
            <li>
              <a href="https://www.linkedin.com/in/jean-claude-amekudzi/">
                <font-awesome-icon :icon="['fab', 'linkedin-in']" />
              </a>
            </li>
          </ul>
        </div>
      </div>
      <div class="footer-bottom">
        <p>&copy; 2024 Your Company. All rights reserved.</p>
        <p>
          Designed by
          <a
            href="https://www.linkedin.com/in/jean-claude-amekudzi/"
            target="_blank"
            >Jean-Claude (.com)</a
          >
        </p>
      </div>
    </div>
  </footer>
</template>

<script>
// Import the necessary Font Awesome files

import { library } from "@fortawesome/fontawesome-svg-core";
import { fab } from "@fortawesome/free-brands-svg-icons";
import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome";

// Add Font Awesome icons to the library
library.add(fab);

export default {
  components: {
    FontAwesomeIcon,
  },
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
        { id: 1, name: "ESSENTIAL_PEDAGOGIES_AND_PROFESSIONAL_PRACTICE" },
        { id: 2, name: "PROFESSIONAL_ATTITUDES_VALUES_AND_ETHICS_IN_TEACHING" },
        { id: 3, name: "USE_OF_ICTs_IN_EDUCATION" },
        // Add more categories here
      ],
      questions: {
        // Define questions for each category
        ESSENTIAL_PEDAGOGIES_AND_PROFESSIONAL_PRACTICE: [
          {
            text: "Question 1 for Category 1?",
            options: ["Option A", "Option B", "Option C"],
            correctAnswer: "Option A",
          },
          {
            text: "What are the two main types of curricula implemented in Ghana?",
            options: [
              "Standards-Based Curriculum and Objective-Based Curriculum",
              "Behaviorism and Constructivism",
              "Basic School Curriculum and Professional Practice",
            ],
            correctAnswer:
              "Standards-Based Curriculum and Objective-Based Curriculum",
          },
          {
            text: "What is the purpose of the Standards-Based Curriculum?",
            options: [
              "To specify a series of skills, values, and competencies at specified stages of learning",
              "To promote global citizenship",
              "To guide the development of standards-based curricula for tertiary education",
            ],
            correctAnswer:
              "To specify a series of skills, values, and competencies at specified stages of learning",
          },
          {
            text: "What is the main characteristic of the Objective-Based Curriculum?",
            options: [
              "Sequentially appropriate",
              "Accountability oriented",
              "Developmentally appropriate",
            ],
            correctAnswer: "Developmentally appropriate",
          },
          {
            text: "According to Tanner and Tanner (1980), how is curriculum defined?",
            options: [
              "As the planned and guided learning experiences formulated through the systematic reconstruction of knowledge",
              "As the process of teaching and learning",
              "As the assessment of learners' continuous growth",
            ],
            correctAnswer:
              "As the planned and guided learning experiences formulated through the systematic reconstruction of knowledge",
          },
          {
            text: "What is the teaching philosophy of the Standards-Based Curriculum?",
            options: [
              "To encourage passive learning",
              "To use innovative, creative, and practical ways of teaching school subjects",
              "To focus on memorization of facts",
            ],
            correctAnswer:
              "To use innovative, creative, and practical ways of teaching school subjects",
          },
          {
            text: "What is emphasized in the learning philosophy of the Standards-Based Curriculum?",
            options: [
              "Passive acquisition of knowledge",
              "Constructing knowledge based on learners' experiences",
              "Memorization of information without context",
            ],
            correctAnswer:
              "Constructing knowledge based on learners' experiences",
          },
          {
            text: "What are the components of the Basic School Curriculum?",
            options: [
              "Philosophy, Vision, and Lesson Plans",
              "Rationale, Aim, and Teaching Strategies",
              "Strand number, sub-strand number, and content strand number",
            ],
            correctAnswer:
              "Strand number, sub-strand number, and content strand number",
          },
          {
            text: "What is the main purpose of the Basic School Curriculum?",
            options: [
              "To guide the development of standards-based curricula for tertiary education",
              "To promote and achieve quality teaching and learning",
              "To create learning opportunities for learners",
            ],
            correctAnswer:
              "To promote and achieve quality teaching and learning",
          },
          {
            text: "Which curriculum is competency-based?",
            options: [
              "Standards-Based Curriculum",
              "Objective-Based Curriculum",
              "Basic School Curriculum",
            ],
            correctAnswer: "Basic School Curriculum",
          },
          {
            text: "What is the role of teachers in the teaching philosophy of the Standards-Based Curriculum?",
            options: [
              "To create passive learning environments",
              "To motivate learners to develop critical thinking and problem-solving skills",
              "To focus solely on delivering content",
            ],
            correctAnswer:
              "To motivate learners to develop critical thinking and problem-solving skills",
          },
          {
            text: "According to Tanner and Tanner (1980), what are the intended learning outcomes of the curriculum?",
            options: [
              "To create passive learners",
              "To foster continuous and wilful growth in personal social competence",
              "To prioritize memorization over understanding",
            ],
            correctAnswer:
              "To foster continuous and wilful growth in personal social competence",
          },
          {
            text: "What type of learning experiences does the curriculum aim to provide?",
            options: [
              "Passive and rote memorization",
              "Active and contextualized construction of knowledge",
              "Isolated and unrelated facts",
            ],
            correctAnswer:
              "Active and contextualized construction of knowledge",
          },
          {
            text: "What is the main focus of the Objective-Based Curriculum?",
            options: [
              "To specify a series of skills, values, and competencies at specified stages of learning",
              "To guide the development of standards-based curricula for tertiary education",
              "To create learning opportunities for learners",
            ],
            correctAnswer:
              "To specify a series of skills, values, and competencies at specified stages of learning",
          },
          {
            text: "What is emphasized in the teaching philosophy of the Standards-Based Curriculum?",
            options: [
              "To encourage rote memorization",
              "To use innovative, creative, and practical ways of teaching school subjects",
              "To discourage active participation from learners",
            ],
            correctAnswer:
              "To use innovative, creative, and practical ways of teaching school subjects",
          },
          {
            text: "What is the role of learners in the learning philosophy of the Standards-Based Curriculum?",
            options: [
              "To passively receive information",
              "To construct knowledge based on their experiences",
              "To solely rely on teachers for knowledge acquisition",
            ],
            correctAnswer: "To construct knowledge based on their experiences",
          },
          {
            text: "What is the primary objective of the Ghanaian school curriculum?",
            options: [
              "To promote global citizenship",
              "To guide the development of standards-based curricula for tertiary education",
              "To equip learners with relevant knowledge, skills, and competencies",
            ],
            correctAnswer:
              "To equip learners with relevant knowledge, skills, and competencies",
          },
          {
            text: "What is the significance of the teaching and learning approaches in the curriculum?",
            options: [
              "To hinder learners' critical thinking abilities",
              "To promote passive learning",
              "To facilitate active participation and critical thinking",
            ],
            correctAnswer:
              "To facilitate active participation and critical thinking",
          },
          {
            text: "Which type of curriculum specifies behaviors learners are expected to exhibit at the end of a learning session?",
            options: [
              "Standards-Based Curriculum",
              "Objective-Based Curriculum",
              "Basic School Curriculum",
            ],
            correctAnswer: "Objective-Based Curriculum",
          },
          {
            text: "What is the main focus of the Rationale for the Basic School Curriculum?",
            options: [
              "To promote and achieve quality teaching and learning",
              "To encourage passive learning",
              "To guide the development of standards-based curricula for tertiary education",
            ],
            correctAnswer:
              "To promote and achieve quality teaching and learning",
          },
          {
            text: "What is the role of teachers in the learning philosophy of the curriculum?",
            options: [
              "To create passive learning environments",
              "To facilitate the construction of learners' knowledge",
              "To discourage critical thinking",
            ],
            correctAnswer:
              "To facilitate the construction of learners' knowledge",
          },
          {
            text: "What is the primary emphasis of the Standards-Based Curriculum?",
            options: [
              "Memorization of facts",
              "Development of critical thinking skills",
              "Promotion of passive learning",
            ],
            correctAnswer: "Development of critical thinking skills",
          },
          {
            text: "Which curriculum emphasizes learners' active participation in the learning process?",
            options: [
              "Standards-Based Curriculum",
              "Objective-Based Curriculum",
              "Basic School Curriculum",
            ],
            correctAnswer: "Standards-Based Curriculum",
          },
          {
            text: "What is the role of teachers according to the teaching philosophy of the Standards-Based Curriculum?",
            options: [
              "To promote passive learning",
              "To discourage learners' participation",
              "To create learning opportunities and motivate learners",
            ],
            correctAnswer:
              "To create learning opportunities and motivate learners",
          },
          {
            text: "What does the curriculum aim to develop in learners?",
            options: [
              "Rote memorization",
              "Critical thinking and problem-solving skills",
              "Passive learning habits",
            ],
            correctAnswer: "Critical thinking and problem-solving skills",
          },
          {
            text: "What is the primary focus of the teaching philosophy of the Standards-Based Curriculum?",
            options: [
              "To use traditional teaching methods",
              "To encourage innovative teaching approaches",
              "To focus on passive learning",
            ],
            correctAnswer: "To encourage innovative teaching approaches",
          },
          {
            text: "What is the primary role of learners in the learning philosophy of the Standards-Based Curriculum?",
            options: [
              "To rely solely on teachers for knowledge acquisition",
              "To construct knowledge based on their experiences",
              "To passively receive information",
            ],
            correctAnswer: "To construct knowledge based on their experiences",
          },
          {
            text: "What is the main characteristic of the Objective-Based Curriculum?",
            options: [
              "Accountability oriented",
              "Sequentially appropriate",
              "Developmentally appropriate",
            ],
            correctAnswer: "Developmentally appropriate",
          },
          {
            text: "What is the main emphasis of the Rationale for the Basic School Curriculum?",
            options: [
              "To guide the development of standards-based curricula for tertiary education",
              "To promote and achieve quality teaching and learning",
              "To encourage passive learning",
            ],
            correctAnswer:
              "To promote and achieve quality teaching and learning",
          },
          {
            text: "What is the primary role of teachers according to the learning philosophy of the curriculum?",
            options: [
              "To promote passive learning",
              "To create learning opportunities and motivate learners",
              "To discourage critical thinking",
            ],
            correctAnswer:
              "To create learning opportunities and motivate learners",
          },
          {
            text: "What is the main focus of the teaching philosophy of the curriculum?",
            options: [
              "To encourage rote memorization",
              "To use innovative, creative, and practical ways of teaching school subjects",
              "To discourage active participation from learners",
            ],
            correctAnswer:
              "To use innovative, creative, and practical ways of teaching school subjects",
          },
          {
            text: "What is the primary goal of the Ghanaian school curriculum?",
            options: [
              "To promote global citizenship",
              "To guide the development of standards-based curricula for tertiary education",
              "To equip learners with relevant knowledge, skills, and competencies",
            ],
            correctAnswer:
              "To equip learners with relevant knowledge, skills, and competencies",
          },
          {
            text: "What is emphasized in the learning philosophy of the curriculum?",
            options: [
              "Passive acquisition of knowledge",
              "Constructing knowledge based on learners' experiences",
              "Memorization of information without context",
            ],
            correctAnswer:
              "Constructing knowledge based on learners' experiences",
          },
          {
            text: "What is the main focus of the Objective-Based Curriculum?",
            options: [
              "To specify a series of skills, values, and competencies at specified stages of learning",
              "To guide the development of standards-based curricula for tertiary education",
              "To create learning opportunities for learners",
            ],
            correctAnswer:
              "To specify a series of skills, values, and competencies at specified stages of learning",
          },
          {
            text: "What is emphasized in the teaching philosophy of the curriculum?",
            options: [
              "To encourage passive learning",
              "To use innovative, creative, and practical ways of teaching school subjects",
              "To focus on memorization of facts",
            ],
            correctAnswer:
              "To use innovative, creative, and practical ways of teaching school subjects",
          },
          {
            text: "What is the role of learners in the learning philosophy of the curriculum?",
            options: [
              "To passively receive information",
              "To construct knowledge based on their experiences",
              "To solely rely on teachers for knowledge acquisition",
            ],
            correctAnswer: "To construct knowledge based on their experiences",
          },
          {
            text: "What is the primary objective of the Ghanaian school curriculum?",
            options: [
              "To promote global citizenship",
              "To guide the development of standards-based curricula for tertiary education",
              "To equip learners with relevant knowledge, skills, and competencies",
            ],
            correctAnswer:
              "To equip learners with relevant knowledge, skills, and competencies",
          },
          {
            text: "What is the significance of the teaching and learning approaches in the curriculum?",
            options: [
              "To hinder learners' critical thinking abilities",
              "To promote passive learning",
              "To facilitate active participation and critical thinking",
            ],
            correctAnswer:
              "To facilitate active participation and critical thinking",
          },
          {
            text: "Which type of curriculum specifies behaviors learners are expected to exhibit at the end of a learning session?",
            options: [
              "Standards-Based Curriculum",
              "Objective-Based Curriculum",
              "Basic School Curriculum",
            ],
            correctAnswer: "Objective-Based Curriculum",
          },
          {
            text: "What is the main focus of the Rationale for the Basic School Curriculum?",
            options: [
              "To promote and achieve quality teaching and learning",
              "To encourage passive learning",
              "To guide the development of standards-based curricula for tertiary education",
            ],
            correctAnswer:
              "To promote and achieve quality teaching and learning",
          },
          {
            text: "What is the role of teachers in the learning philosophy of the curriculum?",
            options: [
              "To create passive learning environments",
              "To facilitate the construction of learners' knowledge",
              "To discourage critical thinking",
            ],
            correctAnswer:
              "To facilitate the construction of learners' knowledge",
          },
          {
            text: "What is the primary emphasis of the Standards-Based Curriculum?",
            options: [
              "Memorization of facts",
              "Development of critical thinking skills",
              "Promotion of passive learning",
            ],
            correctAnswer: "Development of critical thinking skills",
          },
          {
            text: "Which curriculum emphasizes learners' active participation in the learning process?",
            options: [
              "Standards-Based Curriculum",
              "Objective-Based Curriculum",
              "Basic School Curriculum",
            ],
            correctAnswer: "Standards-Based Curriculum",
          },
          {
            text: "What is the role of teachers according to the teaching philosophy of the Standards-Based Curriculum?",
            options: [
              "To promote passive learning",
              "To discourage learners' participation",
              "To create learning opportunities and motivate learners",
            ],
            correctAnswer:
              "To create learning opportunities and motivate learners",
          },
          {
            text: "What does the curriculum aim to develop in learners?",
            options: [
              "Rote memorization",
              "Critical thinking and problem-solving skills",
              "Passive learning habits",
            ],
            correctAnswer: "Critical thinking and problem-solving skills",
          },
          {
            text: "What is the primary focus of the teaching philosophy of the Standards-Based Curriculum?",
            options: [
              "To use traditional teaching methods",
              "To encourage innovative teaching approaches",
              "To focus on passive learning",
            ],
            correctAnswer: "To encourage innovative teaching approaches",
          },
          {
            text: "What is the primary role of learners in the learning philosophy of the Standards-Based Curriculum?",
            options: [
              "To rely solely on teachers for knowledge acquisition",
              "To construct knowledge based on their experiences",
              "To passively receive information",
            ],
            correctAnswer: "To construct knowledge based on their experiences",
          },
          {
            text: "What is the relationship between a scheme of work and a lesson plan?",
            options: [
              "A scheme of work guides a lesson plan",
              "A lesson plan guides a scheme of work",
              "A scheme of work and a lesson plan are unrelated",
            ],
            correctAnswer: "A scheme of work guides a lesson plan",
          },
          {
            text: "Who is responsible for planning the syllabus in Ghana?",
            options: [
              "The classroom teachers",
              "NaCCA",
              "The Ministry of Education",
            ],
            correctAnswer: "NaCCA",
          },
          {
            text: "Which condition is necessary for effective lesson plan preparation?",
            options: [
              "Good revision of the subject matter",
              "Memorization of the syllabus",
              "Limited knowledge of the topic",
            ],
            correctAnswer: "Good revision of the subject matter",
          },
          {
            text: "What should teachers consider when planning a lesson?",
            options: [
              "Only the subject matter",
              "The ages and abilities of the learners",
              "Their personal preferences",
            ],
            correctAnswer: "The ages and abilities of the learners",
          },
          {
            text: "What is the purpose of considering the choice of communication channel in lesson planning?",
            options: [
              "To make the lesson more confusing",
              "To ensure effective communication with learners",
              "To limit communication with learners",
            ],
            correctAnswer: "To ensure effective communication with learners",
          },
          {
            text: "What is essential for enhancing the teaching and learning process?",
            options: [
              "Limited and irrelevant teaching-learning resources",
              "Adequate, relevant, quality teaching-learning resources",
              "Absence of teaching-learning resources",
            ],
            correctAnswer:
              "Adequate, relevant, quality teaching-learning resources",
          },
          {
            text: "What do lesson plans help teachers to specify?",
            options: [
              "The weather forecast",
              "The instructional objectives",
              "The class seating arrangement",
            ],
            correctAnswer: "The instructional objectives",
          },
          {
            text: "What is the purpose of organising complex materials into smaller parts in lesson planning?",
            options: [
              "To make the lesson more confusing",
              "To facilitate the presentation of the lesson",
              "To complicate the learning process",
            ],
            correctAnswer: "To facilitate the presentation of the lesson",
          },
          {
            text: "What do lesson plans help establish regarding learners' lessons?",
            options: [
              "Their favorite subject",
              "Their hobbies",
              "How lessons will be assessed or evaluated",
            ],
            correctAnswer: "How lessons will be assessed or evaluated",
          },
          {
            text: "What are some of the core competencies mentioned?",
            options: [
              "Cooking skills",
              "Financial management",
              "Critical thinking and Problem-solving",
            ],
            correctAnswer: "Critical thinking and Problem-solving",
          },
          {
            text: "What is the primary role of a starter in lesson planning?",
            options: [
              "To stimulate curiosity and open-mindedness",
              "To put learners to sleep",
              "To confuse learners",
            ],
            correctAnswer: "To stimulate curiosity and open-mindedness",
          },
          {
            text: "What activities are included in the main phase of a lesson?",
            options: [
              "Napping",
              "Exploring new learning content",
              "Ignoring the teacher",
            ],
            correctAnswer: "Exploring new learning content",
          },
          {
            text: "What is the purpose of the plenary/reflections phase in lesson planning?",
            options: [
              "To entertain learners",
              "To waste time",
              "To reflect on and consolidate the learning",
            ],
            correctAnswer: "To reflect on and consolidate the learning",
          },
          {
            text: "What is the primary purpose of learning theories?",
            options: [
              "To confuse teachers",
              "To provide models for teaching and learning",
              "To complicate the learning process",
            ],
            correctAnswer: "To provide models for teaching and learning",
          },
          {
            text: "What do behaviourism learning theories focus on?",
            options: ["External factors", "Internal factors", "The weather"],
            correctAnswer: "External factors",
          },
          {
            text: "Who are some notable behaviourism theorists mentioned?",
            options: ["Albert Einstein", "Ivan Pavlov", "Shakespeare"],
            correctAnswer: "Ivan Pavlov",
          },
          {
            text: "What is the primary idea behind social learning theory?",
            options: [
              "Learning occurs in isolation",
              "People learn by observing others",
              "People learn best alone",
            ],
            correctAnswer: "People learn by observing others",
          },
          {
            text: "What does cognitive learning theory focus on?",
            options: [
              "External factors",
              "Internal factors",
              "The teacher's mood",
            ],
            correctAnswer: "Internal factors",
          },
          {
            text: "Who developed the cognitive constructivism view of learning?",
            options: ["Albert Einstein", "Lee Vygotsky", "Jean Piaget"],
            correctAnswer: "Jean Piaget",
          },
          {
            text: "What is the main idea behind constructivism learning theory?",
            options: [
              "Learning is passive",
              "Learners construct knowledge",
              "Learners memorize information",
            ],
            correctAnswer: "Learners construct knowledge",
          },
          {
            text: "What do constructivists believe influences learning?",
            options: [
              "The weather",
              "Prior knowledge and experiences",
              "The time of day",
            ],
            correctAnswer: "Prior knowledge and experiences",
          },
          {
            text: "What is the primary goal of inclusive education?",
            options: [
              "To exclude certain learners",
              "To accommodate all children",
              "To limit access to education",
            ],
            correctAnswer: "To accommodate all children",
          },
          {
            text: "What are some areas of learner diversity mentioned?",
            options: [
              "Hair color",
              "Socio-economic backgrounds",
              "Favorite food",
            ],
            correctAnswer: "Socio-economic backgrounds",
          },
          {
            text: "What does inclusive education take into consideration regarding gender issues?",
            options: [
              "It promotes unfair treatment of females",
              "It perpetuates gender stereotypes",
              "It addresses unfair treatment based on false assumptions",
            ],
            correctAnswer:
              "It addresses unfair treatment based on false assumptions",
          },
          {
            text: "What are some reasons for implementing UDL in schools?",
            options: [
              "To make learning difficult for students",
              "To limit access to education",
              "To ensure all learners receive relevant, appropriate, and targeted provision",
            ],
            correctAnswer:
              "To ensure all learners receive relevant, appropriate, and targeted provision",
          },
          {
            text: "What is a guiding principle of inclusive education?",
            options: [
              "Exclusion of certain children",
              "Accommodation of all children",
              "Favoritism towards certain children",
            ],
            correctAnswer: "Accommodation of all children",
          },
          {
            text: "What do activities promoting inclusive education involve?",
            options: [
              "Segregation of learners",
              "Flexibility in assessment",
              "Limited support for learners",
            ],
            correctAnswer: "Flexibility in assessment",
          },
          {
            text: "What is the purpose of Universal Design for Learning (UDL)?",
            options: [
              "To complicate lesson planning",
              "To anticipate and plan for all learners",
              "To exclude certain learners",
            ],
            correctAnswer: "To anticipate and plan for all learners",
          },
          {
            text: "What is one benefit of using UDL in teaching?",
            options: [
              "It makes learning inaccessible",
              "It ensures only certain learners can access learning",
              "It helps ensure the greatest range of learners can access and engage in learning",
            ],
            correctAnswer:
              "It helps ensure the greatest range of learners can access and engage in learning",
          },
          {
            text: "What are the basic principles of Universal Design for Learning?",
            options: [
              "The where, when, and why of learning",
              "The why, what, and how of learning",
              "The who, what, and where of learning",
            ],
            correctAnswer: "The why, what, and how of learning",
          },
          {
            text: "What is the purpose of social-emotional learning (SEL)?",
            options: [
              "To ignore learners' emotions",
              "To develop self-awareness and interpersonal skills",
              "To limit learners' social interactions",
            ],
            correctAnswer: "To develop self-awareness and interpersonal skills",
          },
          {
            text: "What does SEL support learners to recognize?",
            options: [
              "That feelings are irrelevant",
              "That emotions are experienced similarly by everyone",
              "That feelings are valid",
            ],
            correctAnswer: "That feelings are valid",
          },
          {
            text: "What are the core areas of developing skills in SEL?",
            options: ["Three", "Four", "Five"],
            correctAnswer: "Five",
          },
          {
            text: "What is an activity that promotes conflict resolution in SEL?",
            options: [
              "Encouraging learners to fight",
              "Having learners ignore conflicts",
              "Discussing conflicts and suggesting solutions respectfully",
            ],
            correctAnswer:
              "Discussing conflicts and suggesting solutions respectfully",
          },
          {
            text: "What is an example of an activity promoting self-awareness in SEL?",
            options: [
              "Ignoring learners' emotions",
              "Encouraging learners to express their emotions",
              "Making learners feel ashamed of their emotions",
            ],
            correctAnswer: "Encouraging learners to express their emotions",
          },
          {
            text: "How many minutes does the teacher spend on meditation/visualization in Activity 2?",
            options: ["2 minutes", "3 minutes", "5 minutes"],
            correctAnswer: "3 minutes",
          },
          {
            text: "What is an activity that promotes self-management in SEL?",
            options: [
              "Having learners act impulsively",
              "Teaching learners to manage their emotions and behaviors effectively",
              "Ignoring learners' emotions and behaviors",
            ],
            correctAnswer:
              "Teaching learners to manage their emotions and behaviors effectively",
          },
          {
            text: "What is the relationship between UDL and SEL?",
            options: [
              "They are unrelated",
              "They promote exclusion in education",
              "They both aim to ensure all learners can access and engage in learning",
            ],
            correctAnswer:
              "They both aim to ensure all learners can access and engage in learning",
          },
          {
            text: "How would you help promote inclusive education in your classroom?",
            options: [
              "By excluding certain learners",
              "By fostering a supportive environment for all learners",
              "By ignoring diversity among learners",
            ],
            correctAnswer:
              "By fostering a supportive environment for all learners",
          },
          {
            text: "What practices would you adopt to foster achieving both UDL and SEL in the classroom?",
            options: [
              "Ignoring the needs of diverse learners",
              "Promoting flexibility in teaching and learning",
              "Limiting access to education",
            ],
            correctAnswer: "Promoting flexibility in teaching and learning",
          },
          // Add more questions for Category 1 here
        ],
        PROFESSIONAL_ATTITUDES_VALUES_AND_ETHICS_IN_TEACHING: [
          {
            text: "Question 1 for Category 2?",
            options: ["Option A", "Option B", "Option C"],
            correctAnswer: "Option B",
          },
          {
            text: "What is the focus of this unit?",
            options: [
              "Coding techniques",
              "Teacher professionalism and ethics",
              "Physics concepts",
            ],
            correctAnswer: "Teacher professionalism and ethics",
          },
          {
            text: "What are the learning outcomes of this unit?",
            options: [
              "To understand calculus",
              "To explain teacher professionalism",
              "To learn about oceanography",
            ],
            correctAnswer: "To explain teacher professionalism",
          },
          {
            text: "What is the duration of this unit?",
            options: ["One hour", "Two credit hours", "Four hours"],
            correctAnswer: "Two credit hours",
          },
          {
            text: "What is the primary goal of Session One?",
            options: [
              "To discuss teaching methods",
              "To introduce teacher professionalism",
              "To teach mathematics",
            ],
            correctAnswer: "To introduce teacher professionalism",
          },
          {
            text: "What are some qualities needed to be an effective teacher?",
            options: [
              "Laziness and disorganization",
              "Adequate knowledge, skills, and professional practices",
              "Indifference towards students",
            ],
            correctAnswer:
              "Adequate knowledge, skills, and professional practices",
          },
          {
            text: "What is the role of a teacher beyond teaching?",
            options: [
              "To do nothing else",
              "To motivate learning through a friendly environment",
              "To isolate from the school community",
            ],
            correctAnswer:
              "To motivate learning through a friendly environment",
          },
          {
            text: "Why is teaching classified as a profession?",
            options: [
              "Because it requires no formal training",
              "Because it is not a career",
              "Because it requires formal training over a long period and certification",
            ],
            correctAnswer:
              "Because it requires formal training over a long period and certification",
          },
          {
            text: "What regulates the teaching profession in Ghana?",
            options: [
              "The Ministry of Health",
              "The National Teaching Council",
              "The Department of Agriculture",
            ],
            correctAnswer: "The National Teaching Council",
          },
          {
            text: "What does the National Teaching Council do?",
            options: [
              "Issues driver's licenses",
              "Conducts teacher licensure examinations",
              "Sells textbooks",
            ],
            correctAnswer: "Conducts teacher licensure examinations",
          },
          {
            text: "What does a code of conduct entail?",
            options: [
              "A set of written guidelines detailing ethical norms and professional standards",
              "A list of movies teachers should watch",
              "A compilation of recipes",
            ],
            correctAnswer:
              "A set of written guidelines detailing ethical norms and professional standards",
          },
          {
            text: "What is the objective of the Code of Conduct for Staff of the Ghana Education Service?",
            options: [
              "To lower standards of competence",
              "To achieve high standards of competence and good behavior",
              "To encourage misconduct",
            ],
            correctAnswer:
              "To achieve high standards of competence and good behavior",
          },
          {
            text: "What are ethics in teaching?",
            options: [
              "Moral standards that don't influence behavior",
              "Moral standards and practices that influence behavior",
              "Moral standards irrelevant to teaching",
            ],
            correctAnswer:
              "Moral standards and practices that influence behavior",
          },
          {
            text: "What character traits should teachers model according to professional ethics?",
            options: [
              "Dishonesty and disrespect",
              "Perseverance and honesty",
              "Laziness and impatience",
            ],
            correctAnswer: "Perseverance and honesty",
          },
          {
            text: "What is the importance of developing professional ethics for teachers?",
            options: [
              "To discourage professionalism",
              "To foster unethical behavior",
              "To demonstrate moral and ethical values",
            ],
            correctAnswer: "To demonstrate moral and ethical values",
          },
          {
            text: "What are some tips to help develop professional ethics?",
            options: [
              "Engage in unethical practices",
              "Attend workshops on dishonesty",
              "Demonstrate moral and ethical values",
            ],
            correctAnswer: "Demonstrate moral and ethical values",
          },
          {
            text: "What is the focus of Session Two?",
            options: [
              "Quantum physics",
              "Codes of conduct in teaching",
              "Historical events",
            ],
            correctAnswer: "Codes of conduct in teaching",
          },
          {
            text: "What are the learning outcomes of Session Two?",
            options: [
              "To play sports",
              "To discuss the history of art",
              "To explain codes of conduct and ethics in teaching",
            ],
            correctAnswer: "To explain codes of conduct and ethics in teaching",
          },
          {
            text: "What does a code of conduct detail?",
            options: [
              "Ethical norms and professional standards",
              "Recipes for cooking",
              "Fashion trends",
            ],
            correctAnswer: "Ethical norms and professional standards",
          },
          {
            text: "Who regulates the teaching profession in Ghana?",
            options: [
              "The National Teaching Council",
              "The Department of Transportation",
              "The Ministry of Tourism",
            ],
            correctAnswer: "The National Teaching Council",
          },
          {
            text: "What is the role of the National Teaching Council?",
            options: [
              "Conducts teacher licensure examinations",
              "Sells groceries",
              "Manages public transportation",
            ],
            correctAnswer: "Conducts teacher licensure examinations",
          },
          {
            text: "What is the purpose of the Code of Conduct for Staff of the Ghana Education Service?",
            options: [
              "To lower standards of competence",
              "To achieve high standards of competence and good behavior",
              "To promote misconduct",
            ],
            correctAnswer:
              "To achieve high standards of competence and good behavior",
          },
          {
            text: "What are ethics in teaching?",
            options: [
              "Moral standards irrelevant to teaching",
              "Moral standards that don't influence behavior",
              "Moral standards and practices that influence behavior",
            ],
            correctAnswer:
              "Moral standards and practices that influence behavior",
          },
          {
            text: "What character traits should teachers model according to professional ethics?",
            options: [
              "Dishonesty and disrespect",
              "Perseverance and honesty",
              "Laziness and impatience",
            ],
            correctAnswer: "Perseverance and honesty",
          },
          {
            text: "What is the importance of developing professional ethics for teachers?",
            options: [
              "To discourage professionalism",
              "To foster unethical behavior",
              "To demonstrate moral and ethical values",
            ],
            correctAnswer: "To demonstrate moral and ethical values",
          },
          {
            text: "What are some tips to help develop professional ethics?",
            options: [
              "Engage in unethical practices",
              "Attend workshops on dishonesty",
              "Demonstrate moral and ethical values",
            ],
            correctAnswer: "Demonstrate moral and ethical values",
          },

          // Add more questions for Category 2 here
        ],
        USE_OF_ICTs_IN_EDUCATION: [
          {
            text: "Question 1 for Category 3?",
            options: ["Option A", "Option B", "Option C"],
            correctAnswer: "Option B",
          },
          {
            text: "What is object-oriented programming (OOP)?",
            options: [
              "A programming paradigm based on objects",
              "A programming language",
              "A programming method",
            ],
            correctAnswer: "A programming paradigm based on objects",
          },
          {
            text: "What is a class in OOP?",
            options: [
              "A function",
              "A blueprint for creating objects",
              "A data structure",
            ],
            correctAnswer: "A blueprint for creating objects",
          },
          {
            text: "What is inheritance in OOP?",
            options: [
              "A way to create multiple instances of a class",
              "The process of defining a new class based on an existing class",
              "A way to restrict access to certain class members",
            ],
            correctAnswer:
              "The process of defining a new class based on an existing class",
          },
          {
            text: "What is polymorphism in OOP?",
            options: [
              "The ability of a function to take on different forms",
              "The ability to inherit multiple classes",
              "The process of creating multiple objects",
            ],
            correctAnswer:
              "The ability of a function to take on different forms",
          },
          {
            text: "What is encapsulation in OOP?",
            options: [
              "The process of combining data and functions into a single unit",
              "The process of creating multiple instances of a class",
              "The process of creating subclasses",
            ],
            correctAnswer:
              "The process of combining data and functions into a single unit",
          },
          {
            text: "What is a constructor in OOP?",
            options: [
              "A method that is called when an object is created",
              "A method that is called when an object is destroyed",
              "A method that is called when an object is modified",
            ],
            correctAnswer: "A method that is called when an object is created",
          },
          {
            text: "What is a destructor in OOP?",
            options: [
              "A method that is called when an object is created",
              "A method that is called when an object is destroyed",
              "A method that is called when an object is modified",
            ],
            correctAnswer:
              "A method that is called when an object is destroyed",
          },
          {
            text: "What is method overloading in OOP?",
            options: [
              "The process of defining multiple methods with the same name but different parameters",
              "The process of defining multiple methods with different names but the same parameters",
              "The process of defining multiple methods with the same name and parameters",
            ],
            correctAnswer:
              "The process of defining multiple methods with the same name but different parameters",
          },
          {
            text: "What is method overriding in OOP?",
            options: [
              "The process of defining multiple methods with the same name but different parameters",
              "The process of defining multiple methods with different names but the same parameters",
              "The process of defining a method in a subclass that already exists in the superclass",
            ],
            correctAnswer:
              "The process of defining a method in a subclass that already exists in the superclass",
          },
          {
            text: "What is a static method in OOP?",
            options: [
              "A method that can be called without creating an instance of the class",
              "A method that can only be called within the class",
              "A method that is automatically called when an object is created",
            ],
            correctAnswer:
              "A method that can be called without creating an instance of the class",
          },
          {
            text: "What is a static variable in OOP?",
            options: [
              "A variable that can only be accessed within the class",
              "A variable that is automatically initialized when an object is created",
              "A variable that is shared among all instances of a class",
            ],
            correctAnswer:
              "A variable that is shared among all instances of a class",
          },
          {
            text: "What is a final variable in OOP?",
            options: [
              "A variable that cannot be changed after initialization",
              "A variable that is automatically initialized when an object is created",
              "A variable that is shared among all instances of a class",
            ],
            correctAnswer:
              "A variable that cannot be changed after initialization",
          },
          {
            text: "What is a final method in OOP?",
            options: [
              "A method that cannot be overridden in a subclass",
              "A method that cannot be called without creating an instance of the class",
              "A method that cannot be accessed outside of the class",
            ],
            correctAnswer: "A method that cannot be overridden in a subclass",
          },
          {
            text: "What is a package in Java?",
            options: [
              "A collection of classes and interfaces",
              "A single class",
              "A single file",
            ],
            correctAnswer: "A collection of classes and interfaces",
          },
          {
            text: "What is an interface in Java?",
            options: [
              "A class that cannot be instantiated",
              "A class that cannot have methods",
              "A blueprint of a class that defines a set of methods",
            ],
            correctAnswer:
              "A blueprint of a class that defines a set of methods",
          },
          {
            text: "What is a constructor chaining in Java?",
            options: [
              "Calling one constructor from another within the same class",
              "Calling constructors from different classes",
              "Creating multiple instances of a class",
            ],
            correctAnswer:
              "Calling one constructor from another within the same class",
          },
          {
            text: "What is method chaining in Java?",
            options: [
              "Calling multiple methods in sequence on the same object",
              "Calling methods from different classes",
              "Creating multiple instances of a class",
            ],
            correctAnswer:
              "Calling multiple methods in sequence on the same object",
          },
          {
            text: "What is the difference between an abstract class and an interface in Java?",
            options: [
              "An abstract class can have concrete methods, while an interface cannot",
              "An interface can have fields, while an abstract class cannot",
              "An abstract class can be instantiated, while an interface cannot",
            ],
            correctAnswer:
              "An abstract class can have concrete methods, while an interface cannot",
          },
          {
            text: "What is the purpose of the 'super' keyword in Java?",
            options: [
              "To call the constructor of the superclass",
              "To call a static method of the superclass",
              "To call a method of the subclass",
            ],
            correctAnswer: "To call the constructor of the superclass",
          },
          {
            text: "What is method hiding in Java?",
            options: [
              "Hiding a method implementation in a superclass",
              "Hiding a method implementation in a subclass",
              "Hiding a method implementation in a static method",
            ],
            correctAnswer: "Hiding a method implementation in a subclass",
          },
          {
            text: "What is method signature in Java?",
            options: [
              "The name and return type of a method",
              "The parameters and return type of a method",
              "The access modifiers and return type of a method",
            ],
            correctAnswer: "The name and return type of a method",
          },
          {
            text: "What is method visibility in Java?",
            options: [
              "The ability to see the method within the same class",
              "The ability to see the method within the same package",
              "The ability to see the method from any class",
            ],
            correctAnswer: "The ability to see the method from any class",
          },
          {
            text: "What is method abstraction in Java?",
            options: [
              "Hiding the implementation details of a method",
              "Exposing the implementation details of a method",
              "Removing a method from a class",
            ],
            correctAnswer: "Hiding the implementation details of a method",
          },
          {
            text: "What is a lambda expression in Java?",
            options: [
              "A way to represent anonymous functions",
              "A way to represent named functions",
              "A way to represent global functions",
            ],
            correctAnswer: "A way to represent anonymous functions",
          },
          {
            text: "What is the purpose of the 'this' keyword in Java?",
            options: [
              "To refer to the current object",
              "To refer to the superclass",
              "To refer to the subclass",
            ],
            correctAnswer: "To refer to the current object",
          },
          {
            text: "What is a generic class in Java?",
            options: [
              "A class that can take any type as a parameter",
              "A class that can only take primitive types as parameters",
              "A class that cannot take any parameters",
            ],
            correctAnswer: "A class that can take any type as a parameter",
          },
          {
            text: "What is method chaining in JavaScript?",
            options: [
              "Calling multiple methods in sequence on the same object",
              "Calling methods from different objects",
              "Creating multiple instances of an object",
            ],
            correctAnswer:
              "Calling multiple methods in sequence on the same object",
          },
          {
            text: "What is prototypal inheritance in JavaScript?",
            options: [
              "The ability of an object to inherit properties and methods from another object",
              "The process of creating multiple instances of a class",
              "The process of creating a new class based on an existing class",
            ],
            correctAnswer:
              "The ability of an object to inherit properties and methods from another object",
          },
          {
            text: "What is a closure in JavaScript?",
            options: [
              "A function that has access to its own scope, plus the scope of its parent function",
              "A function that has access to the global scope",
              "A function that has access to the scope of its child function",
            ],
            correctAnswer:
              "A function that has access to its own scope, plus the scope of its parent function",
          },
          {
            text: "What is the purpose of the 'new' keyword in JavaScript?",
            options: [
              "To create a new instance of an object",
              "To call a constructor function",
              "To create a new class",
            ],
            correctAnswer: "To create a new instance of an object",
          },
          {
            text: "What is a callback function in JavaScript?",
            options: [
              "A function that is passed as an argument to another function",
              "A function that calls another function",
              "A function that is executed immediately",
            ],
            correctAnswer:
              "A function that is passed as an argument to another function",
          },
          {
            text: "What is event delegation in JavaScript?",
            options: [
              "Attaching event listeners to parent elements instead of individual child elements",
              "Attaching event listeners to child elements instead of parent elements",
              "Removing event listeners from elements",
            ],
            correctAnswer:
              "Attaching event listeners to parent elements instead of individual child elements",
          },
          {
            text: "What is a promise in JavaScript?",
            options: [
              "An object representing the eventual completion or failure of an asynchronous operation",
              "A function that returns a value",
              "An object representing a synchronous operation",
            ],
            correctAnswer:
              "An object representing the eventual completion or failure of an asynchronous operation",
          },
          {
            text: "What is asynchronous programming in JavaScript?",
            options: [
              "A programming paradigm that allows operations to be performed concurrently",
              "A programming paradigm that allows operations to be performed sequentially",
              "A programming paradigm that does not allow operations to be performed",
            ],
            correctAnswer:
              "A programming paradigm that allows operations to be performed concurrently",
          },
          {
            text: "What is the purpose of the 'await' keyword in JavaScript?",
            options: [
              "To wait for a promise to be resolved or rejected",
              "To execute a function asynchronously",
              "To handle errors in asynchronous code",
            ],
            correctAnswer: "To wait for a promise to be resolved or rejected",
          },
          {
            text: "What is the purpose of the 'fetch' API in JavaScript?",
            options: [
              "To make HTTP requests",
              "To create new objects",
              "To manipulate the DOM",
            ],
            correctAnswer: "To make HTTP requests",
          },
          {
            text: "What is a closure in Python?",
            options: [
              "A function that has access to its own scope, plus the scope of its parent function",
              "A function that has access to the global scope",
              "A function that has access to the scope of its child function",
            ],
            correctAnswer:
              "A function that has access to its own scope, plus the scope of its parent function",
          },
          {
            text: "What is the purpose of the 'yield' keyword in Python?",
            options: [
              "To return a value from a generator function",
              "To call a function",
              "To create a new class",
            ],
            correctAnswer: "To return a value from a generator function",
          },
          {
            text: "What is a decorator in Python?",
            options: [
              "A function that modifies the behavior of another function",
              "A function that returns another function",
              "A function that returns an iterator",
            ],
            correctAnswer: "A function that returns an iterator",
          },
          {
            text: "What is the purpose of the 'map' function in Python?",
            options: [
              "To apply a function to every item in an iterable",
              "To create a new list",
              "To concatenate multiple lists",
            ],
            correctAnswer: "To apply a function to every item in an iterable",
          },
          {
            text: "What is the purpose of the 'filter' function in Python?",
            options: [
              "To filter elements from an iterable based on a function",
              "To create a new list",
              "To concatenate multiple lists",
            ],
            correctAnswer:
              "To filter elements from an iterable based on a function",
          },
          {
            text: "What is the purpose of the 'reduce' function in Python?",
            options: [
              "To apply a function to pairs of elements and reduce them to a single value",
              "To create a new list",
              "To concatenate multiple lists",
            ],
            correctAnswer:
              "To apply a function to pairs of elements and reduce them to a single value",
          },
          {
            text: "What is a set in Python?",
            options: [
              "An unordered collection of unique elements",
              "An ordered collection of elements",
              "A mutable collection of elements",
            ],
            correctAnswer: "An unordered collection of unique elements",
          },
          {
            text: "What is a dictionary in Python?",
            options: [
              "A collection of key-value pairs",
              "An ordered collection of elements",
              "A mutable collection of elements",
            ],
            correctAnswer: "A collection of key-value pairs",
          },
          {
            text: "What is the purpose of the 'zip' function in Python?",
            options: [
              "To combine multiple iterables into a single iterable",
              "To create a new list",
              "To concatenate multiple lists",
            ],
            correctAnswer:
              "To combine multiple iterables into a single iterable",
          },
          {
            text: "What is a module in Python?",
            options: ["A file containing Python code", "A class", "A function"],
            correctAnswer: "A file containing Python code",
          },
          {
            text: "What is a package in Python?",
            options: [
              "A directory containing Python modules",
              "A single Python file",
              "A class",
            ],
            correctAnswer: "A directory containing Python modules",
          },
          {
            text: "What is the purpose of the '__init__' method in Python classes?",
            options: [
              "To initialize an object's attributes",
              "To define class methods",
              "To create a new instance of a class",
            ],
            correctAnswer: "To initialize an object's attributes",
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
        //Clear Selected answer
        this.selectedAnswer = null;
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
    this.adsenseContent = document.getElementById("adsgoeshere").innerHTML;
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
.footer {
  margin-top: 30px;
  background-color: #e6ecef;
  color: #929090;
  padding: 40px 0;
}

.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
}

.footer-content {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.footer-logo img {
  width: 150px;
}

.footer-links ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

.footer-links ul li {
  display: inline-block;
  margin-right: 20px;
}

.footer-links ul li a {
  color: #bdd4fb;
  text-decoration: none;
}

.footer-social ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

.footer-social ul li {
  display: inline-block;
  margin-right: 10px;
}

.footer-social ul li a {
  color: #fff;
  font-size: 18px;
  text-decoration: none;
}
.footer-social {
  text-align: center; /* Align icons to the center */
}

.icon {
  color: black;
}
</style>
