<template>
  <main class="main">
    <!------------ Quizz container ----------->
    <div class="quizz_container">
      <!------------ Question container ----------->
      <div class="questions">
        <h2 class="question_item">{{ currentQuestion.question }}</h2>
      </div>

      <!------------ All options container ----------->
      <div class="answers">
        <div class="score">Score</div>
        <div class="score__total-score" style="color:red">{{ score }}</div>
        <!------------ Single option container ----------->
        <div class="answer_option">
          <div v-for="(choice, item) in currentQuestion.choices" :key="item">
            <div
              class="flex answer_option__item option_default_color"
              :ref="optionChosen"
              @click="onOptionClicked(choice, item)"
            >
              <div class="answer_option_id">{{ item + 1 }}</div>
              <div class="answer_option_text">{{ choice }}</div>
            </div>
          </div>
        </div>
      </div>

      <!--------- Option container ----------->
      <div class="progress-indicator text-center">
        <div class="indicator-bar h-bar w-bar"></div>
        <p class="indicator-text">
          {{ questionCounter }} / {{ questions.length }}
        </p>
      </div>
    </div>
  </main>
</template>

<script>
import { onMounted, ref } from "vue";
export default {
  setup() {
    let canClick = true;
    let questionCounter = ref(0);
    let score = ref(null);
    const currentQuestion = ref({
      question: "",
      answer: 1,
      choices: [],
    });

    // List of proposed questions
    const questions = [
      {
        question: "Qu'est ce qui est jaune et qui attend ?",
        answer: 1,
        choices: [
          "Tes erreurs warning qui attendent correction!",
          "Jonathan !",
          "Tes fichiers modifiés suivie par Git",
          "La bonne réponse",
        ],
      },
      {
        question: "What is the character limit for a Tweet ?",
        answer: 1,
        choices: [
          "120 characters",
          "280 characters",
          "320 characters",
          "160 characters",
        ],
      },
      {
        question: "Who will win the Worl Cup 2022 ?",
        answer: 3,
        choices: ["Aller Guingamp", "France", "South Korea", "Morrocco"],
      },
      {
        question: "What is Vue.js ?",
        answer: 0,
        choices: [
          "A progressive framework for JavaScript used to build web interfaces and one-page applications",
          "An organized collection of structured information stored in a computer system",
          "A mechanism that enable two software components to communicate with each other using a set of definitions and protocols",
          "An application-layer protocol for transmitting hypermedia documents",
        ],
      },
      {
        question: "What do you understand by Framework ?",
        answer: 1,
        choices: [
          "It's a beautiful frame usually hung on a wall that depicts people working",
          "It's a structure that provides a platform to create applications. It is a collection of similar types of files placed in such a way that they are configured to connect/integrate with each other internally",
          "It's a tool used in order building things for the web, WordPress for example",
          "It's an environment that encapsulates a set of technologies,  used for developing, shipping, and running applications",
        ],
      },
      {
        question:
          "What is the difference between Null, Empty, and Undefined value and how can you handle each ?",
        answer: 3,
        choices: [
          "I see no difference",
          "Null is when one element is compared with another that makes the first element null and void, undefined is when an element is declared but without a value",
          "Null is for instance in a soccer game when neither team wins, undefined is when there is a match scheduled but without the presence of spectators, players, coaches and referees  ",
          "The undefined value is a primitive value used when a variable has not been assigned a value. The null value is a primitive value that represents the null, empty, or non-existent reference.",
        ],
      },
      {
        question:
          "In JavaScript, what method is used to arrange array values into alphabetical and/or ascending order?",
        answer: 0,
        choices: ["sort()", "from()", "shift()", "unshift()"],
      },
      {
        question:
          "In JavaScript, what is the name of the technique used to extract an object's values into new variables?",
        answer: 3,
        choices: [
          "Hoisting",
          "typeof",
          "Array destructuring",
          "Object destructuring",
        ],
      },
      {
        question: "What is GitHub?",
        answer: 1,
        choices: [
          "An online IDE",
          "A hosting platform for Git repositories",
          "A popular database",
          "A subscription based platform to sell coding classes",
        ],
      },
      {
        question: "What is the DOM in JavaScript?",
        answer: 2,
        choices: [
          "A process that moves variables, functions, and classes to the top of the scope.",
          "A function that is used as an argument for another function.",
          "A programming interface to create, change, or remove elements from the document.",
          "Technique used to extract array values into new variables.",
        ],
      },
    ];
    
    // Question loader
    const loadQuestion = () => {
      canClick = true;
      // Check if there there are more questions to load
      if (questions.length > questionCounter.value) {
        // Load question
        currentQuestion.value = questions[questionCounter.value];
        console.log("Current questions: " + currentQuestion.value);
        questionCounter.value += 1;
      } else {
        console.log("No more questions to load");
      }
    };

    // Methods
    let itemsRef = [];
    const optionChosen = (element) => {
      if (element) {
        itemsRef.push(element);
      }
    };

    // Clear answer options once the user has selected (clicked) an answer and load the next question in the list after 1.5 sec
    const clearSelected = (divSelected) => {
      setTimeout(() => {
        divSelected.classList.remove("correct_answer_selected");
        divSelected.classList.remove("wrong_answer_selected");
        divSelected.classList.add("default_color_answer");
        loadQuestion();
      }, 1500);
    };

    const onOptionClicked = (choice, item) => {
      if (canClick) {
        // permit to select an option
        const divTagItem = itemsRef[item];
        const optionID = item;
        if (currentQuestion.value.answer == optionID) {
          console.log("You're correct my friend");
          score.value += 10;
          divTagItem.classList.add("correct_answer_selected");
          divTagItem.classList.remove("default_color_answer");
        } else {
          console.log("You're tremendously wrong my friend");
          divTagItem.classList.add("wrong_answer_selected");
          divTagItem.classList.remove("default_color_answer");
        }
        canClick = false;
        // Go to next question
        clearSelected(divTagItem);
        console.log(choice, item);
      } else {
        // can't select an option
        console.log("Can't select another option");
      }
    };

    // lifecycle hooks
    onMounted(() => {
      loadQuestion();
    });

    // return
    return {
      currentQuestion,
      questions,
      score,
      questionCounter,
      loadQuestion,
      onOptionClicked,
      optionChosen,
    };
  },
};
</script>

<style>
@media (max-width: 1024px) {
  main {
    margin-top: 1.5rem;
  }

  .question_item {
    font-size: larger;
    font-weight: bold;
  }

  .answer_option_id {
    font-size: 2rem;
  }

  .answer_option_text {
    font-size: 1rem;
  }
}

@media (min-width: 1024px) {
  .question_item {
    font-size: 1.5rem;
    font-weight: bold;
  }

  .answer_option_id {
    font-size: 2rem;
  }

  .answer_option_text {
    font-size: 1.1rem;
  }
}

.quizz_container {
  color: #fff;
  padding: 1.5rem 2rem;
  min-height: 100vh;
  display: flex;
  align-items: center;
  flex-direction: column;
  flex-wrap: wrap;
  background-color: hsla(160, 100%, 37%, 1);
  box-shadow: 0 10px 15px -3px rgb(0 0 0 / 0.1), 0 4px 6px -4px rgb(0 0 0 / 0.1);
  border-radius: 0.5rem;
}

.questions {
  width: 100%;
  height: fit-content;
  border-radius: 0.5rem;
  background-color: rgb(243 244 246);
  color: hsla(160, 100%, 37%, 1);
  padding: 0.5rem;
  text-align: center;
}

.question_item {
  background-color: #fff;
  padding: 1.25rem;
  border-radius: 0.5rem;
}

.answers {
  width: 100%;
  border-radius: 0.5rem;
  margin-top: 2rem;
  background-color: rgb(243 244 246);
}

.answer_option {
  padding: 0.75rem;
  border-radius: 0.5rem;
  padding-bottom: 2rem;
}

.answer_option__item {
  cursor: pointer;
}

.score {
  font-weight: bold;
  text-align: end;
  margin-right: 10%;
  font-size: 1.2rem;
  color: #2c3e50;
}
.score__total-score {
  font-weight: bold;
  text-align: end;
  margin-right: 10%;
  font-size: 1rem;
  color: #2c3e50;
}

.default_color_answer {
  background-color: rgb(243 244 246) !important;
}

.correct_answer_selected {
  background-color: rgba(62, 255, 68, 0.768) !important;
}

.wrong_answer_selected {
  background-color: rgba(247, 36, 12, 0.903) !important;
}

.flex {
  display: flex;
  background-color: #fff;
  padding: 0.5rem;
  border-radius: 0.5rem;
  margin-top: 1rem;
  margin-bottom: 1rem;
}

.answer_option_id {
  border-radius: 0.5rem;
  background-color: #2c3e50;
  padding: 2px 1rem;
  color: #fff;
  text-align: center;
  align-self: center;
}

.answer_option_text {
  border-radius: 0.5rem;
  color: #2c3e50;
  padding: 0.5rem 1rem;
  font-weight: bold;
}

.h-bar {
  height: 0.25rem;
}

.w-bar {
  width: 5rem;
}

.text-center {
  text-align: center;
}

.progress-indicator {
  margin-top: 2rem;
}

.indicator-bar {
  background-color: rgb(31 41 55);
  border-radius: 9999px;
  margin-bottom: 5px;
}

.indicator-text {
  color: #fff;
  font-size: 1.2rem;
  font-weight: bold;
}
</style>
