<template>
  <ion-page>
    <ion-content>
      <ion-header :translucent="true">
        <ion-toolbar color="primary">
          <ion-title>Movie Card Game</ion-title>
          <ion-buttons slot="end">
            <ion-button
              @click="showBook = !showBook"
              v-show="!gameStarted && !showModal"
              ><ion-icon slot="icon-only" :icon="book"></ion-icon
            ></ion-button>
            <ion-button
              @click="showModal = !showModal"
              v-show="!gameStarted && !showBook"
              ><ion-icon slot="icon-only" :icon="add"></ion-icon
            ></ion-button>
          </ion-buttons>
        </ion-toolbar>
      </ion-header>

      <div class="modal" v-if="showModal">
        <div class="form">
          <ion-item>
            <ion-label position="floating">Movie</ion-label>
            <ion-input v-model="title"></ion-input>
          </ion-item>
          <ion-item>
            <ion-label position="floating">Goal</ion-label>
            <ion-input v-model="goal"></ion-input>
          </ion-item>
          <ion-button expand="full" @click="AddMovie">Add Movies</ion-button>
          {{ movie }} - {{ goal }}
        </div>
      </div>
      <div class="modal" v-if="showBook == true && !gameStarted">
        <ion-card>
          <ion-item>
            <ion-icon :icon="book" slot="start"></ion-icon>
            <ion-label><h2>How To Play</h2></ion-label>
          </ion-item>

          <ion-card-content>
            <p>
              Once the game starts you will see a movie title and an objective
              <br />
              The card will reveal a movie and an objective <br />
              Have fun with voices and movie quotes.
            </p>
          </ion-card-content>
          <ion-item>
            <ion-icon :icon="book" slot="start"></ion-icon>
            <ion-label><h2>Character</h2></ion-label>
          </ion-item>
          <ion-card-content>
            <p>
              If the card contains just a character name, you must quote
              their lines and attempt to do their voices.
            </p>
          </ion-card-content>
            <ion-item>
            <ion-icon :icon="book" slot="start"></ion-icon>
            <ion-label><h2>Movie Title</h2></ion-label>
          </ion-item>
          <ion-card-content>
            <p>
              If the card objective is a movie title, you must quote as many lines as you can.
            </p>
          </ion-card-content>
        </ion-card>
      </div>
      <div id="container">
        <p v-if="!gameStarted">Click the button to start</p>
        <ion-button shape="round" @click="ToggleClass()">
          <ion-icon
            slot="icon-only"
            :icon="play"
            v-if="!gameStarted"
          ></ion-icon>
          <ion-icon slot="icon-only" :icon="playSkipForward" v-else></ion-icon
        ></ion-button>
        <div class="card">
          <div :class="`${isFlipped}`">
            <div class="card-face card-face-front">
              <h2 v-if="!gameStarted">Card</h2>
              <h2 v-else>Movie</h2>
            </div>
            <div class="card-face card-face-back">
              <ion-card>
                <ion-card-header>
                  <img
                    src="https://images.unsplash.com/photo-1489599849927-2ee91cede3ba?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=1050&q=80"
                  />
                  <ion-card-title>{{ title }}</ion-card-title>
                </ion-card-header>
                <ion-card-content>
                  <h2>Challenge: {{ goal }}</h2>
                </ion-card-content>
              </ion-card>
            </div>
          </div>
        </div>
        <ion-button shape="round" v-show="gameStarted" @click="endGame()"
          >End Game</ion-button
        >
      </div>
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import {
  IonContent,
  IonHeader,
  IonPage,
  IonButtons,
  IonTitle,
  IonCard,
  IonCardContent,
  IonCardTitle,
  IonLabel,
  IonInput,
  IonCardHeader,
  IonToolbar,
  IonButton,
} from "@ionic/vue";
import { defineComponent } from "vue";
import { play, playSkipForward, add, close, book } from "ionicons/icons";
export default defineComponent({
  name: "Home",
  components: {
    IonContent,
    IonHeader,
    IonLabel,
    IonInput,
    IonButtons,
    IonCard,
    IonCardContent,
    IonCardTitle,
    IonCardHeader,
    IonPage,
    IonButton,
    IonTitle,
    IonToolbar,
  },
  data() {
    return {
      gameStarted: false,
      isFlipped: "card_inner",
      movies: [
        { title: "Toy Story", goal: "Buzz Quotes" },
        { title: "Toy Story", goal: "Woody Quotes" },
        { title: "Toy Story", goal: "As many Quotes as possible" },
        { title: "Toy Story 2", goal: "As many Quotes as possible" },
        { title: "Cars", goal: "As many Quotes as possible" },
        { title: "Cars 2", goal: "Mater Quotes" },
      ],
      title: '',
      goal: '',
      play,
      close,
      book,
      add,
      playSkipForward,
      showModal: false,
      showBook: false,
    };
  },
  created() {
    this.GetMovies();
  },
  methods: {
    AddMovie() {
      if (this.title != "" && this.goal != "") {
        const newMovie = {
          title: this.title,
          goal: this.goal,
        };
        this.movies.push(newMovie);
        this.showModal = false;
        localStorage.setItem("movies", JSON.stringify(this.movies));
        (this.title = ""), (this.goal = "");
      }
    },
    ToggleClass() {
      if (this.isFlipped == "card_inner") {
        this.gameStarted = true;
        this.isFlipped = "card_inner is-flipped";
        this.GetAMovie();
      } else if (this.isFlipped == "card_inner is-flipped") {
        this.isFlipped = "card_inner";
      }
    },
    endGame() {
      this.gameStarted = false;
      this.isFlipped = "card_inner";
    },
    GetMovies() {
      const storedMovies = localStorage.getItem("movies");
      if (storedMovies) {
        this.movies = JSON.parse(storedMovies);
      } else {
        localStorage.setItem("movies", JSON.stringify(this.movies));
      }
    },
    GetAMovie() {
      const movie = this.movies[Math.floor(Math.random() * this.movies.length)];
      this.title = movie.title;
      this.goal = movie.goal;
    },
  },
});
</script>

<style scoped>
.card {
  margin: 50px auto 0;
  width: 400px;
  height: 400px;
}
.card_inner {
  width: 100%;
  height: 100%;
  transition: transform 1s;
  transform-style: preserve-3d;
  cursor: pointer;
  perspective: 1000;

}
.card_inner.is-flipped {
  transform: rotateY(180deg);
}
.card-face {
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  overflow: hidden;
  position: absolute;
  border-radius: 10px;
}
.card-face-front {
  background-image: linear-gradient(to bottom right, pink, purple);
  display: flex;
  color: white;
  align-items: center;
  justify-content: center;
  padding: 10px;
}
.card-face-back {
  transform: rotateY(180deg);
}
.modal {
  background-color: rgba(255, 255, 255);
  width: 100%;
  color: black;
  height: 100%;
  margin: 50px auto;
  align-items: center;
  justify-content: center;
  border-radius: 16px;
  border: 4px solid purple;
  transform: translate((-50%, -50%));
  max-width: 600px;
  max-height: 400px;
  z-index: 1;
  position: relative;
}
#container {
  text-align: center;

  position: absolute;
  left: 0;
  right: 0;
  top: 50%;
  transform: translateY(-50%);
}

#container strong {
  font-size: 20px;
  line-height: 26px;
}

#container p {
  font-size: 16px;
  line-height: 22px;
  color: #8c8c8c;
  margin: 0;
}

#container a {
  text-decoration: none;
}
</style>
