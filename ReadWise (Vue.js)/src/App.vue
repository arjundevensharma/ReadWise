<template>
  <div class="reading-test">
    <h2>{{ passage.title }}</h2>
    <div v-for="(line, index) in passage.lines" :key="index">
      <p v-if="index === currentLine">{{ line }}</p>
      <p v-else class="hidden">{{ line }}</p>
    </div>
    <button v-if="currentLine < passage.lines.length" @click="nextLine">Next</button>
    <div v-else>
      <h3>Comprehension Quiz</h3>
      <form @submit.prevent="submitQuiz">
        <div v-for="(question, index) in quiz.questions" :key="index">
          <p>{{ question.prompt }}</p>
          <div v-for="(option, index) in question.options" :key="index">
            <input type="radio" :id="`option-${index}`" :value="option" v-model="quiz.answers[index]">
            <label :for="`option-${index}`">{{ option }}</label>
          </div>
        </div>
        <button type="submit">Submit</button>
      </form>
      <p v-if="readingSpeed">Your reading speed is {{ readingSpeed }} words per minute.
      </p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      passage: {
        title: "Reading Test and Book Recommender",
        lines: [
          "Welcome to the Reading Test! After clicking the 'Next' button, you will be given lines of a passage. Read through each line carefully and click the 'Next' button to proceed to the next line of the passage. Once you have finished reading the passage, you will be presented with a comprehension quiz. Please select the best answer for each question and click the 'Submit' button to see your results. After submitting the quiz, you will be provided with your reading speed and a book recommendation based on your performance. Good luck!",
          "In the heart of the verdant forest, nestled amidst a canopy of emerald foliage, lay a hidden glade, untouched by human hands.",
          " The chirping of birds and rustling of leaves lent a melodic charm to the idyllic scene.",
          " A gentle breeze carried the fragrant scent of wildflowers, while the warm sun caressed the skin with a soft glow.",
          "Suddenly, a twig snapped, interrupting the peaceful serenade.",
          " A young doe emerged from the thicket, her large brown eyes scanning the surroundings with caution.",
          " She took a tentative step forward, her slender legs trembling 	slightly, before finally relaxing as she realized there was no danger."

        ]
      },
      currentLine: 0,
      quiz: {
        questions: [
          
          {
            prompt: "What sounds can be heard in the glade?",
            options: ["The chirping of birds", "The honking of cars", "The barking of dogs", "The roaring of planes"],
            answer: "The chirping of birds"
          },
          
          {
            prompt: "Where is the glade located?",
            options: ["On the edge of a cliff", "In the heart of a forest", "In the middle of a desert", "None of the above"],
            answer: "In the heart of a forest"
          },
{
prompt: "Which of the following best describes the author's use of descriptive language in the passage?",
options: ["Sparse and minimalistic", "Repetitive and dull", "Vivid and evocative", "Incoherent and confusing"],
answer: "Vivid and evocative"
},
          
          {
            prompt: "What happens when the young doe emerges from the thicket?",
            options: ["She starts grazing on the wildflowers", "She runs away as soon as she sees something moving",  "She attacks the source of the noise", "She takes a few cautious steps forward and then relaxes"],
            answer: "She takes a few cautious steps forward and then relaxes"
          }
        ],
        
        answers: ["In the heart of a forest","The chirping of birds","She takes a few cautious steps forward and then relaxes","Vivid and evocative"  ]
        
      },
      readingSpeed: 0,
  startTime: 0,
  endTime: 0
}
},
methods: {
nextLine() {
if (this.currentLine === 1) {
this.startTime = new Date().getTime();
} 
  this.currentLine++;
  if (this.currentLine === 7) {
    this.endTime = new Date().getTime();
  }
    },
  submitQuiz() {
		let numCorrect = 0;
    let totalWords = this.passage.lines.join(' ').split(' ').length - 91;
    let timeTaken = (this.endTime - this.startTime) / 1000;
    let multiplier = 60 / timeTaken;
    this.readingSpeed = Math.round(totalWords * multiplier);
    
    for (let i = 0; i < this.quiz.questions.length; i++) {
      if (this.quiz.answers[i] === this.quiz.questions[i].answer) {
        numCorrect++;
      }
    }

    let readingLevel = "";
    let bookRecommendation = "";

    if (numCorrect >= 4 && this.readingSpeed >= 700) {
      readingLevel = "Post-University";
      bookRecommendation = "Ulysses by James Joyce";
    } else if (numCorrect >= 4 && this.readingSpeed >= 300) {
      readingLevel = "Grade 12/University";
      bookRecommendation = "The Great Gatsby by F. Scott Fitzgerald";
    } else if (numCorrect >= 3 && this.readingSpeed >= 250) {
      readingLevel = "Grades 9-10";
      bookRecommendation = "The Great Gatsby by F. Scott Fitzgerald";
    } else if (numCorrect >= 2 && this.readingSpeed >= 200) {
      readingLevel = "Grades 7-8";
      bookRecommendation = "To Kill a Mockingbird by Harper Lee";
    } else if (numcorrect >= 2) {
      readingLevel = "Grade 5-6";
      bookRecommendation = "Harry Potter and the Philosopher's Stone by J.K. Rowling";         
    }
      else if (numcorrect >= 1) {
      readingLevel = "Grade 3-4";
      bookRecommendation = "Charlotte's Web by E.B. White";
    } else {
      readingLevel = "Grade 1-2";
      bookRecommendation = "The Cat in the Hat by Dr. Seuss";
    }

    alert(`You answered ${numCorrect} out of ${this.quiz.questions.length} questions correctly. Your reading speed is ${this.readingSpeed} words per minute. Based on your performance, we recommend reading "${bookRecommendation}" for a ${readingLevel} reader.`);
      }
  }
}
</script>

<style>
.hidden {
  display: none;
}
</style>