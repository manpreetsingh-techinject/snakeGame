<script lang="ts">
import { initializeApp } from "firebase/app";
import { getFirestore, doc, setDoc, collection, addDoc } from "firebase/firestore";

const firebaseConfig = {
  apiKey: "AIzaSyDCM6hJ0Mnx35aCcGoRSF390x93Ladf6Fo",
  authDomain: "test-app-ab77c.firebaseapp.com",
  projectId: "test-app-ab77c",
  storageBucket: "test-app-ab77c.appspot.com",
  messagingSenderId: "29254434956",
  appId: "1:29254434956:web:ac1f75664d4caca0767ba9",
  measurementId: "G-NY4D91ZSG8"
};

const app = initializeApp(firebaseConfig);
const db = getFirestore(app);

let snake = [{ x: 0, y: 0 }];
let eggX = 0;
let eggY = 0;
let direction = "right";
let score = 0;
let sPositionX=0;
let sPositionY=0;
let container:HTMLDivElement;
let display=false;
let gameOver=false;

  function updateEggPosition() {
    eggX = Math.floor(Math.random() * ((630 - 10) / 20)) * 20;
    eggY = Math.floor(Math.random() * ((630 - 10) / 20)) * 20;
  }

  function moveSnake() {

      let newHead = { x: snake[0].x, y: snake[0].y };
        if (direction == "right") {
          newHead.x += 20;
        } else if (direction == "left") {
          newHead.x -= 20;
        } else if (direction == "up") {
          newHead.y -= 20;
        } else if (direction == "down") {
          newHead.y += 20;
        }

        //check if the snake eats the egg
        if (newHead.x == eggX && newHead.y == eggY) {
          updateEggPosition();
          snake.push({x: 0, y: 0});
          // let snakeSegment = document.createElement("div");
          // // snakeSegment.className = "snake";
          // snakeSegment.style.backgroundColor = "rgb(17, 253, 63)";
          // snakeSegment.style.border = "0.3mm solid black";
          // container.appendChild( snakeSegment);
          score+=1;
          // document.getElementById("value").innerHTML = score;
        }
        
        for (let i = snake.length - 1; i > 0; i--) {
          snake[i] = { ...snake[i - 1] };
        }
        snake[0] = { ...newHead };

          // check if the snake hits the border
          if ( newHead.x < 0 || newHead.y < 0 || newHead.x >= 630 - 10 || newHead.y >= 630 - 10 ) {
            endGame();
            return;
          }
          // let snakeSegments = document.querySelectorAll(".snake");
          
          for(let i=0;i<snake.length;i++){
              sPositionX=snake[i].x;
              sPositionY=snake[i].y;
          }
          // snakeSegments.forEach((segment, index) => {
            // segment.style.left = snake[index].x + "px";
            // segment.style.top = snake[index].y + "px";
          // });
  }

  //   //set the newHead to old positions


  //   //check if the snake bites itself
  //   for (let i = 1; i < snake.length; i++) {
  //     if (newHead.x == snake[i].x && newHead.y == snake[i].y) {
  //       endGame();
  //       return;
  //     }
  //   }

  //   //Update the positions of the snake
  // }

  // //Update Snake Directions
   function handleKeydown(event:KeyboardEvent) {
    if (event.key == "ArrowRight" && direction != "left") {
      direction = "right";
    } else if (event.key == "ArrowLeft" && direction != "right") {
      direction = "left";
    } else if (event.key == "ArrowUp" && direction != "down") {
      direction = "up";
    } else if (event.key == "ArrowDown" && direction != "up") {
      direction = "down";
    }
  };
  updateEggPosition();

  let c = 1;
  let gameInterval:NodeJS.Timeout;
  let counter = setInterval(() => {
    c += 1;
  }, 1000);

  setTimeout(() => {
    clearInterval(counter);

    gameInterval = setInterval(moveSnake, 200);
        display=true;
  //   // document.querySelector("video").style.display = "block";
  //   // document.querySelector(".center").style.display = "flex";
  //   // document.querySelector(".game-container").style.display = "block";
  //   // document.querySelector(".score").style.display = "block";
  //   // document.querySelector(".game-container").style.position = "relative";
  //   // document.querySelector(".game-container").style.border = "5px dashed rgba(255, 255, 255, 0.5)";
  //   // document.querySelector(".score").style.position = "absolute";
  //   // document.querySelector(".snake").style.background = "rgb(17, 253, 63)";
  //   // document.querySelector(".snake").style.border = "0.3mm solid black";
  //   // document.querySelector(".egg").style.backgroundColor = "white";
  //   // document.querySelector(".sbody").style.display = "none";
  //   // document.querySelector(".end").style.display = "none";
  //   // document.querySelector(".cont").style.display = "flex";
  }, 3000);

  function endGame() {
    clearInterval(gameInterval);
    display=false;
    gameOver=true;
    
    // addDoc(collection(db, 'SnakeGame'), {'points' : score})

  //   // document.querySelector("video").style.display = "none";
  //   // document.querySelector(".center").style.display = "none";
  //   // document.querySelector(".game-container").style.display = "none";
  //   // document.querySelector(".score").style.display = "none";
  //   // document.querySelector(".cont").style.display = "none";
  //   // document.querySelector(".sbody").style.display = "flex";
  //   // document.querySelector(".end").style.display = "flex";
  }

</script>

<svelte:window on:keydown={handleKeydown} />
<video autoplay loop muted >
  <source src="video.mp4" type="video/mp4" />
</video>

{#if !display}
<div class="sbody">
  {#if !gameOver}
  <div class="cont">
    <div class="spinner"></div>
    <h1 class="number">{c}</h1>
  </div>
  {:else}
    <div class="end">
      <h1>game over</h1>
    </div>
  {/if}
</div>
  {/if}

{#if display}
<div class="main">
    
  <div class="center">
    <div class="game-container">
      {#each snake as snakebox}
    <div class="snake" style="left:{sPositionX}px; top:{sPositionY}px;"></div>
    {/each}
    <div class="egg" style="left:{eggX}px; top:{eggY}px;"></div>
    <svg>
      <circle style="margin-left: 20px; background-color:aliceblue"></circle>
    </svg>
  </div>
  <div class="score">
    SCORE
    <span id="value">{score}</span>
  </div>
</div>
</div>

{/if}
<style>

  .main {
    width: 100%;
    height: 100vh;
    margin: 0;
    padding: 0;
    background: black;
  }
  video {
    position: absolute;
    height: 100vh;
    object-fit: cover;
    width: 100%;
  }
  .center {
    /* display: none; */
    width: 100%;
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .game-container {
    /* display: none; */
    position: relative;
    width: calc(100vh - 10px);
    height: calc(100vh - 10px);
    margin: auto;
    border: 5px dashed rgba(255, 255, 255, 0.5);
    box-sizing: border-box;
  }
  .snake {
    position:absolute;
    background-color: rgba(0, 0, 0, 0);
    background: rgb(17, 253, 63);
    width: 20px;
    height: 20px;
    border: 0.3mm solid black;
  }
  .snake:nth-child(1)::before,
  .snake:nth-child(1)::after {
    position: absolute;
    content: "";
    width: 5px;
    height: 5px;
    background: black;
    top: 3px;
    left: 3px;
    border-radius: 50%;
  }
  .snake:nth-child(1)::after {
    left: auto;
    right: 3px;
  }
  .egg {
    position: absolute;
    background: white;
    width: 20px;
    height: 20px;
    border-radius: 50%;
    animation: blink 0.5s infinite;
  }
  @keyframes blink {
    from {
      transform: scale(1);
    }
    to {
      transform: scale(0.9);
    }
  }
  .score {
    /* display: none; */
    position: absolute;
    top: 10px;
    left: 30px;
    font-size: 18px;
    font-family: "segoe UI";
    color: white;
    display: flex;
    align-items: center;
    flex-direction: column;
  }
  .score span {
    display: block;
    font-size: 24px;
  }

  .sbody {
    position: absolute;
    background-color: #777;
    height: 100vh;
    width: 100%;
    background-image: linear-gradient(
        black,
        transparent 10%,
        transparent calc(50% - 1px),
        #aaa calc(50% - 1px),
        #aaa calc(50% + 1px),
        transparent calc(50% + 1px),
        transparent 90%,
        black
      ),
      linear-gradient(
        90deg,
        black,
        transparent 10%,
        transparent calc(50% - 1px),
        #aaa calc(50% - 1px),
        #aaa calc(50% + 1px),
        transparent calc(50% + 1px),
        transparent 90%,
        black
      );
  }
  .end {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100%;
    width: 100%;
    font-size: 50px;
  }
  .cont {
    width: 250px;
    height: 250px;
    position: fixed;
    text-align: center;
    font-weight: bold;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    border: 2px solid #aaa;
    border-radius: 50%;
    overflow: hidden;

    &::after {
      content: "";
      width: 200px;
      height: 200px;
      position: absolute;
      top: 23px;
      left: 23px;
      border: 1px solid #aaa;
      border-radius: 50%;
      background: #777;
      background-image: linear-gradient(
          transparent calc(50% - 1px),
          #aaa calc(50% - 1px),
          #aaa calc(50% + 1px),
          transparent calc(50% + 1px)
        ),
        linear-gradient(
          90deg,
          transparent calc(50% - 1px),
          #aaa calc(50% - 1px),
          #aaa calc(50% + 1px),
          transparent calc(50% + 1px)
        );
    }
  }

  .spinner {
    position: absolute;
    top: 50%;
    left: 50%;
    width: 125px;
    height: 100px;
    transform-origin: 0 0;
    background-image: linear-gradient(black, transparent);
    animation: spin 1s infinite linear;

    @keyframes spin {
      from {
        transform: rotate(0deg);
      }
      to {
        transform: rotate(-360deg);
      }
    }
  } 

   .number {
    font-size: 100px;
    position: absolute;
    top: 50%;
    left: 50%;
    z-index: 2;
    transform: translate(-50%, -50%);
    width: 250px;
    height: 250px;

    &::after {
      content: "";
      width: 250px;
      height: 250px;
      position: absolute;
      top: 0;
      left: 0;
      font-size: 150px;
      line-height: 250px;
      animation: count 10s infinite steps(10, start);

      @keyframes count {
        0%,
        100% {
          content: "0";
        }
        10% {
          content: "9";
        }
        20% {
          content: "8";
        }
        30% {
          content: "7";
        }
        40% {
          content: "6";
        }
        50% {
          content: "5";
        }
        60% {
          content: "4";
        }
        70% {
          content: "3";
        }
        80% {
          content: "2";
        }
        90% {
          content: "1";
        }
      }
    }
  }
</style>
