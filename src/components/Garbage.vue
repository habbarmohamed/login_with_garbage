<template>
  <div class="hello">

    <div class="bell-container" v-if="bell">
      <h3 class="warning-bell">
        Be Ready the truck it will arrive in seconds
      </h3>
    </div>


    <div class="group">
      <label for="username">Username:</label>
      <input 
        type="text" 
        class="form-control username" 
        id="username" 
        autocomplete="off" 
        v-model="username" 
        @input="handleInput"
        ref="username"
      />
      <code class="warning-message">
        The username must not exceed the input width.
      </code>
    </div>
    <div class="group">
      <label for="password">Password:</label>
      <input 
        type="password" 
        class="form-control" 
        id="password" 
        autocomplete="off" 
      />
    </div>
  </div>

 

  <div class="trash" @click="overflowChars.length > 0 ? animateTrash() : null">
    <img 
      src="../assets/trash.png" 
      alt="trash" 
      class="trash-img" 
      :class="{ 'trash-fall': trashFalling }"
      >
    <div class="overflow-container">
      <span 
        v-for="(char, index) in overflowChars" 
        :key="index" 
        class="overflow-char"
        :class="{ 'overflow-falling': trashFalling }"
      >
        {{ char }}
      </span>
    </div>

    <div v-if="charsFalltoCamion">
      <span 
        v-for="(char, index) in overflowChars" 
        :key="index" 
        class="fall-char"
        :style="{ animationDelay: `${index * 0.1}s` }"
      >
        {{ char }}
      </span>
    </div>

    <div v-if="camion && overflowChars.length > 0" class="camion">
      <img 
        src="../assets/camion.png" 
        alt="camion" 
        class="camion-img" 
        >
    </div>
    <div v-if="overflowChars.length == 0 && charsInGarbageCar.length > 0" class="camion-back">
      <img 
        src="../assets/camion.png" 
        alt="camion" 
        class="camion-back-img" 
        >
    </div>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },
  data() {
    return {
      username: "",
      overflowChars: [],
      trashFalling: false, 
      trashReset: false,
      camion: false,
      bell: false,
      countdown: 10,
      charsFalltoCamion: false,
      charsInGarbageCar: []
    };
  },
  
  methods: {
    handleInput() {
      const input = this.$refs.username;
      const maxWidth = input.getBoundingClientRect().width - 53; // Get the input width

      const tempSpan = document.createElement("span");
      document.body.appendChild(tempSpan);
      tempSpan.style.position = "absolute";
      tempSpan.style.visibility = "hidden";
      tempSpan.style.whiteSpace = "nowrap";
      tempSpan.textContent = this.username; 

      const textWidth = tempSpan.getBoundingClientRect().width - 10;
      document.body.removeChild(tempSpan);

      if (textWidth > maxWidth) {
        const overflow = this.username.slice(this.username.length - 1); 
        this.username = this.username.slice(0, this.username.length - 1); 
        this.animateOverflow(overflow);
        
      }

      // Show Camion 
      
      if (this.overflowChars.length > 0) {
        setTimeout(() => {
          this.bell = true;
        }, 2000);
        setTimeout(() => {
          this.camion = true;
        }, 4000);
        this.bell = false;
        this.countdown = 0; 
      }
      else {
        this.bell = false;
        this.camion = false;
      }

    },
    animateOverflow(overflow) {
      const chars = overflow.split("");
      this.overflowChars.push(...chars);

      setTimeout(() => {
        // this.overflowChars = [];
      }, 2000); 
    },
    animateTrash() {
      if (this.trashFalling) {
        this.trashFalling = false;
      } else {
        this.trashFalling = true;
        setTimeout(() => {
          this.charsFalltoCamion = true;
        }, 900);
        setTimeout(() => {
            this.charsInGarbageCar = [...this.overflowChars];
            this.overflowChars.length = 0;
            this.bell = false;
        }, 2000);
        setTimeout(() => {
          this.trashFalling = false;
        }, 2000);
      }

      
    },
    // toggleTrashAnimation() {
    //   if (this.trashFalling) {
    //     this.trashReset = true; 
    //     this.trashFalling = false;

    //     setTimeout(() => {
    //       this.trashReset = false;
    //     }, 300);
    //   } else {
    //     this.trashFalling = true;
    //   }
    // },
    toggleTrashAnimation() {
      if (this.trashFalling) {
        this.trashFalling = false;
        this.trashReset = true;

        // Pause for 3 seconds before resetting
        setTimeout(() => {
          this.trashReset = false;
        }, 3000);
      } else {
        this.trashFalling = true;

        // Automatically stop falling after 1.5s, then pause for 3 seconds
        setTimeout(() => {
          this.trashFalling = false;
          this.trashReset = true;

          setTimeout(() => {
            this.trashReset = false;
          }, 3000); 
        }, 1500);
      }
}

  
  },
};
</script>



<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
body {
    margin: 0;
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 17px;
    overflow-x: hidden;
  }

  .hello {
    display: block;
    gap: 16px; 
    align-items: center;
    padding: 20px;
    border-radius: 8px;
    justify-content: center;
    width: 300px;
    margin:auto
  }

  .group {
    display: flex;
    flex-direction: column;
    margin-bottom: 1em;
  }

  .group label {
    margin-bottom: 8px; 
    font-size: 14px;
    color: #333;
    text-align:left
  }

  .form-control {
    padding: 8px 12px;
    font-size: 14px;
    border: 1px solid #ccc;
    border-radius: 1px;
    outline: none;
    transition: border-color 0.2s ease-in-out;
    width: 150px;
    font-size: 21px;
    
  }

  .form-control.username {
    border-right: none;
    width: 170px; 
    overflow: hidden; 
    text-overflow: ellipsis; 
    white-space: nowrap; 
    padding-right: 0;
  
  }



.overflow-container {
  position: relative;
  position: relative;
    top: -132px;
    left: 30px;
}

.overflow-char {
  position: absolute;
  animation: moveDown 2.5s ease-in-out forwards;
  font-size: 18px;
  color: black;
}
code {
  width: 60%;
}
@keyframes moveDown {
  0% {
    transform: translateY(0);
    transform: translateX(0);
    opacity: 1;
  }
  10% {
    transform: translateY(40px) translateX(1px);
    opacity: 1;
  }
  20% {
    transform: translateY(80px) translateX(2px);
    opacity: 1;
  }
  30% {
    transform: translateY(120px) translateX(4px);

    opacity: 1;
  }
  40% {
    transform: translateY(160px) translateX(5px);

    opacity: 1;
  }
  50% {
    transform: translateY(200px) translateX(7px);

    opacity: 1;
  }
  60% {
    transform: translateY(240px) translateX(8px);

    opacity: 1;
  }
  70% {
    transform: translateY(280px) translateX(10px);

    opacity: 1;
  }
  80% {
    transform: translateY(320px) translateX(11px);

    opacity: 1;
  }
  90% {
    transform: translateY(360px) translateX(13px);

    opacity: 1;
  }
  100% {
    transform: translateY(400px) translateX(14px);

    opacity: 0;
  }
}

.trash {
  height: 200px;
  position: relative;
  cursor: pointer; 

}

.trash-img {
  width: 50%;
    position: absolute;
    right: 37em;
    top: -7em;
    cursor: pointer;
    transition: transform .5s ease-in-out;
}

.camion {
  position: relative;
  height: 200px;
  width: 850px;
  position: relative;
  cursor: pointer; 

}
.camion-img {
  width: 100%;
  position: absolute;
    left: -5em;
    top: 2em;
    animation: moveRight 3s linear forwards;
}

@keyframes moveRight {
  0% {
    transform: translateX(-100%);
  }
  100% {
    transform: translateX(5vw);
  }
}
.camion-back {
  position: relative;
  height: 200px;
  width: 850px;
  position: relative;
  cursor: pointer; 

}
.camion-back-img {
  width: 100%;
  position: absolute;
    left: 2em;
    top: 2em;
    animation: moveBack 3s linear forwards;
    animation-delay: 2s;
}

@keyframes moveBack {
  0% {
    transform: translateX(2.7vw);
  }
  100% {
    transform: translateX(-150%);
  }
}

.trash-fall {
  animation: fall 4s ease-in-out forwards, fall-reverse 4s ease-in-out 4s forwards;
}


@keyframes fall {
  0% {
    transform: rotateZ(0) translateY(0);
  }
  10% {
    transform: rotateZ(-150deg) translateY(100px); 
  }
  30%, 35% {
    transform: rotateZ(-155deg) translateY(100px); 
  }
  40%, 45% {
    transform: rotateZ(-145deg) translateY(100px); 
  }
  50% {
    transform: rotateZ(-150deg) translateY(100px);
  }
  100% {
    transform: rotateZ(-150deg) translateY(100px);
  }
}


@keyframes fall-reverse {
  0% {
    transform: rotateZ(-150deg) translateY(100px);
  }
  50% {
    transform: rotateZ(-145deg) translateY(100px);
  }
  60%, 65% {
    transform: rotateZ(-155deg) translateY(100px);
  }
  90% {
    transform: rotateZ(-150deg) translateY(100px);
  }
  100% {
    transform: rotateZ(0) translateY(0);
  }
}

/*
.trash-fall {
  animation: fall 2s ease forwards; 
}

@keyframes fall {
  
  to {
    -webkit-transform: rotateZ(-150deg);
            transform: rotateZ(-150deg);
  }
} */

.bell-container {
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
}
.warning-bell {
  position: absolute;
  top: -2em;
  right: -36em;
  background: #ffeded;
  border: 2px solid #ff4d4d;
  border-radius: 8px;
  padding: 10px 15px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  animation: shakeWarning 1.5s ease-in-out infinite;
}
@keyframes shakeWarning {
  0%, 100% {
    transform: translateX(0);
  }
  25% {
    transform: translateX(-5px);
  }
  50% {
    transform: translateX(5px);
  }
  75% {
    transform: translateX(-5px);
  }
}


.overflow-falling {
  animation: moveDown 2.5s ease-in-out forwards; /* Add falling animation */
}

.fall-char {
  position: absolute;
  top: 2em;
  left: 45em;
  animation: moveDownToCamion 2.5s ease-in-out forwards;
  font-size: 18px;
  color: black;
}
@keyframes moveDownToCamion {
  0% {
    transform: translateY(-190) translateX(-400px);
    opacity: 1;
  }
  100% {
    transform: translateY(300px) translateX(-250px);

    opacity: 0;
  }
}
</style>
