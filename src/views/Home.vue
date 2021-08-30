<template>
  <div class="home">
    <img alt="Skyrim Logo" src="../assets/skyrim.png" height="200" width="400"><span>kind of...</span>
    <iframe id="wickedMusic" width="200" height="200" class="ms-5 mt-5" src="https://www.youtube.com/embed/AVy7YPNP_zI?autoplay=1&controls=1&loop=1&mute=0" title="YouTube video player" frameborder="0" allow="autoplay;"></iframe>

    <div v-if="gameNotStarted">
      <h3>Are You Ready to Face Almost Certain Death?</h3>
      <button @click="startGame()" class="btn btn-primary">Attempt to Slay the Dragon!!!</button>
    </div>

    <div v-if="youWon">
      <h1>Congrats on not being a noob =]</h1>
    </div>
    <div v-if="youLost">
      <h1>You are a disgrace to both your loved ones and closest friends. :[ </h1>
    </div>

    <div v-show="!gameNotStarted">
        <div @click="moveSprites($event)" style="border: 1px solid blue; min-height: 50rem;" id="gameBoard">

        <div class="row" style="min-height: 100vh;">
          <div class="col">
            <!-- <div id="dragonHealthBar">

            </div>-->
            <img v-if="!landed" ref="flying_dragon" alt="dragon" src="../assets/flying_dragon.png" style="height: 300px; width: 200px; position: absolute; bottom: -25rem;">
            <div v-if="landed">
              <div>
                {{dragon_health_left + '/' + dragon_health}}
              </div>
              <div class="progress text-center" style="width: 85%;">
                <div ref="dragonProgressBar" class="progress-bar" role="progressbar" style="width: 100%" aria-valuenow="1000" aria-valuemin="0" aria-valuemax="1000"></div>
              </div>
              <div class="row">
                <div class="col">
                  <img id="landed_dragon" alt="dragon_landed" src="../assets/attack_dragon.png" style="left: 0rem;">
                </div>
                <div class="col">
                  <img v-if="dragon_is_attacking" alt="fire" src="../assets/dragon_fire.png" style="left: 0px; margin-top: 100px; width: 300px; height: 150px;">
                </div>
              </div>


            </div>
        
          </div>
      
          <div class="col">
            <!-- <div id="characterHealthBar">

            </div> -->
            <div>
              {{character_health_left + '/' + character_health}}
            </div>
            <div class="progress">
                <div ref="characterProgressBar" class="progress-bar" role="progressbar" style="width: 100%; background-color: green;" aria-valuenow="100" aria-valuemin="0" aria-valuemax="100"></div>
            </div>

            <div class="row">
              <div v-if="character_is_attacking" class="col">
                <img v-if="weaponChoice === 'arrow'" src="../assets/bow.png" style="width: 200px; height: 200px;">
                <img v-if="weaponChoice === 'sword'" src="../assets/sword.png" style="width: 200px; height: 200px;">
                <img v-if="weaponChoice === 'axe'" src="../assets/axe.png" style="width: 200px; height: 200px;">
                <div v-if="weaponChoice === 'shield'">
                  <p>legend of zelda is better :P</p>
                  <img src="../assets/shield.png" style="width: 200px; height: 200px;">
                </div>
              </div>
              <div class="col mt-5">
                <img id="dragonborn" alt="character" src="../assets/warrior.png" height="75" width="75">
              </div>
            </div>
            

            <div v-if="landed" style="margin-top: 25rem;">
              <ul class="list-group">
                <li class="list-group-item">
                  'A' - shoot arrow
                </li>
                <li>
                  'S' - use shield
                </li>
                <li class="list-group-item">
                  'X' - use axe
                </li>
                <li class="list-group-item">
                  'W' - use sword
                </li>
              </ul>
            </div>
           
          </div>
        </div>
      </div>
    </div> 
  </div>


</template>

<script>
// @ is an alias to /src


// var flying_dragon = document.getElementById('flying_dragon');

// setInterval(function () {
//   console.log('hello world');


//   flying_dragon.style.left += 1;

// }, 2000);

export default {
  name: 'Home',
  components: {
  },
  data() {
    return {
      gameNotStarted: true,

      //dragon
      flying: false,
      landed: false,
      dragon_startingX: 75,
      dragon_startingY: 500,

      dragon_health: 1000,
      dragon_health_left: 1000,
      dragon_is_attacking: false,
      attack_rate: 5,
      attack_counter: 0,

      character_health: 100,
      character_health_left: 100,
      character_is_attacking: false,
      weaponChoice: null,

      truck_health: 5,

      dragon_position: -25,

      youWon: false,
      youLost: false,
      randomInterval: 6000

      
    }
  },
  methods: {
    moveSprites(e) {
      console.log('triggered');
      console.log('event: ', e);
      console.log('coords: ', e.clientX + '    ' + e.clientY);

    },
    startGame() { 
      this.gameNotStarted = false;
      this.flying = true;

      console.log('refs: ', this.$refs);


      var flyingDragon = this.$refs['flying_dragon'];

      var dragonMover = setInterval(() => {
        console.log('position: ', this.dragon_position);
        if (this.dragon_position === 20) {
          this.flying = false;
          this.landed = true;
          clearInterval(dragonMover);
        }
        flyingDragon.style.bottom = (this.dragon_position + 5) + 'rem';
        console.log('fly style: ', flyingDragon.style.bottom);
        this.dragon_position += 5;
      }, 2000);
      
    }
  },
  created() {


  },
  mounted() {

    console.log('random interval: ', this.randomInterval);

    window.addEventListener("keypress", e => {
      this.character_is_attacking = false;
      console.log(String.fromCharCode(e.keyCode));

      switch (String.fromCharCode(e.keyCode)) {
        case 'a':
          console.log('arrow');
          if (this.dragon_is_attacking) break;

          this.character_is_attacking = true;
          this.weaponChoice = 'arrow';

          this.dragon_health_left -= 5;
          this.$refs.dragonProgressBar.style.width = (this.dragon_health_left/10)+"%";

          if (this.dragon_health_left <= 0) {
            this.youWon = true;
          }
        
          break;
        case 'x':
          console.log('axe');
          if (this.dragon_is_attacking) break;

          this.character_is_attacking = true;
          this.weaponChoice = 'axe';

          this.dragon_health_left -= 20;
          this.$refs.dragonProgressBar.style.width = (this.dragon_health_left/10)+"%";

          if (this.dragon_health_left <= 0) {
            this.youWon = true;
          }

          break;
        case 's':
          console.log('shield');
          if (this.dragon_is_attacking) break;

          this.character_is_attacking = true;
          this.weaponChoice = 'shield';
          
          break;
        case 'w':
          console.log('sword');
          if (this.dragon_is_attacking) break;

          this.character_is_attacking = true;
          this.weaponChoice = 'sword';

          this.dragon_health_left -= 12;
          this.$refs.dragonProgressBar.style.width = (this.dragon_health_left/10)+"%";

          if (this.dragon_health_left <= 0) {
            this.youWon = true;
          }
          break;
        default:
          console.log('do nothing on this key');
          this.character_is_attacking = false;
          this.weaponChoice = null;
      }
    });

  

    setInterval(() => {

      if (this.landed === true && this.attack_rate % this.attack_counter === 0) {
        this.dragon_is_attacking = true;
        console.log('progress bar: ', this.$refs)
        if (this.weaponChoice !== 'shield') {
          this.character_health_left -= 10;
          this.$refs.characterProgressBar.style.width = this.character_health_left+"%";
          if (this.character_health_left <= 0) {
            this.youLost = true;
          }
        }
      } else if (this.landed === true && this.attack_rate % this.attack_counter !== 0) {
        this.dragon_is_attacking = false;
      }

      if (this.attack_counter === 5) {
        this.attack_counter = 0;
      } else {
        this.attack_counter++;
      }
    }, 6000)
  }


}
</script>
