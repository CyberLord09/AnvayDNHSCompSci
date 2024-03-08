---
toc: false
comments: false
layout: post
title: Mario Animations on Page
type: hacks
image: /images/mario_animation.png
courses: { compsci: {week: 13} }
---


<!-- Liquid:  statements -->
<!--- Concatenation of site URL to frontmatter image  --->
{% assign sprite_file = site.baseurl | append: page.image %}
<!--- Has is a list variable containing mario metadata for sprite --->
{% assign hash = site.data.mario_metadata %}  
<!--- Size width/height of Sprit images --->
{% assign pixels = 256 %} 

<!--- HTML for page contains <p> tag named "Mario" and class properties for a "sprite"  -->

<p id="mario" class="sprite"></p>
  
<!--- Embedded Cascading Style Sheet (CSS) rules, 
        define how HTML elements look 
--->
<style>

  /*CSS style rules for the id and class of the sprite...
  */
  .sprite {
    height: {{pixels}}px;
    width: {{pixels}}px;
    background-image: url('{{sprite_file}}');
    background-repeat: no-repeat;
  }

  /*background position of sprite element
  */
  #mario {
    background-position: calc({{animations[0].col}} * {{pixels}} * -1px) calc({{animations[0].row}} * {{pixels}}* -1px);
  }
  body {
    font-family: Arial, sans-serif;
    background-color: #f0f0f0;
    color: #333;
    margin: 0;
    padding: 0;
  }
  table {
    width: 110%;
    border-collapse: collapse;
    margin-top: 10px;
  }
  th, td {
    border: 1px solid #ddd;
    padding: 8px;
    text-align: left;
  }
  th {
    background-color: #4CAF50;
    color: white;
  }
  .coloredtext {
    color: rgb(75, 186, 255);
  }
</style>


<body>

<table>
  <tr>
    <th>Fact</th>
    <th>Description</th>
    <th>Fact</th>
    <th>Description</th>
  </tr>
  <tr>
    <td class="coloredtext" >1. Mario's Origins</td>
    <td>Mario made his first appearance in the 1981 arcade platformer, <em>Donkey Kong</em>, created by Shigeru Miyamoto.</td>
    <td class="coloredtext" >6. Mario's Voice Actor</td>
    <td>Charles Martinet has voiced Mario and other characters since 1995, after showing up uninvited to an audition and impressing the crew with his performance.</td>
  </tr>
  <tr>
    <td class="coloredtext" >2. Mario's Namesake</td>
    <td>Mario was named after a real estate developer named Mario Segale, who had interactions with Nintendo's American headquarters in Tukwila, Washington.</td>
    <td class="coloredtext" >7. Mario's World Records</td>
    <td>Mario holds several Guinness World Records, including being the most prolific video game character and having the longest-running video game series.</td>
  </tr>
  <tr>
    <td class="coloredtext" >3. Mario's Occupations</td>
    <td>Mario has held various occupations throughout his gaming career, including: Carpenter, Plumber, Doctor, Racer, Martial Artist, Baseball Player, Soccer Player, Basketball Player, Pro Golfer, Tennis Player, Construction Worker, Olympic Athlete</td>
    <td class="coloredtext" >8. Mario's Dark Backstory</td>
    <td>The manual for *Super Mario Bros.* reveals a darker backstory where Mario is tasked with rescuing the Mushroom People from Koopa's black magic.</td>
  </tr>
  <tr>
    <td class="coloredtext" >4. Mario as a Villain</td>
    <td>Mario was depicted as a villain in *Donkey Kong Junior*, where he captured Donkey Kong, who was trying to save his father.</td>
    <td class="coloredtext" >9. Mario's Last Name</td>
    <td>Nintendo officially states that Mario and Luigi do not have a last name, although the 1993 *Super Mario Bros.* film humorously suggests their last name is "Mario."</td>
  </tr>
  <tr>
    <td class="coloredtext" >5. Mario's Original Nemesis</td>
    <td>Miyamoto originally envisioned an ox being Mario's biggest enemy, which later became the turtle-like Koopa, Bowser.</td>
    <td class="coloredtext" >10. Mario's Extensive Gaming Career</td>
    <td>Mario has appeared in over 255 games, spanning various genres and platforms, making him one of the most iconic and prolific characters in gaming history.</td>
  </tr>
</table>

</body>



<script>
  ////////// convert YML hash to javascript key:value objects /////////

  var mario_metadata = {}; //key, value object
  {% for key in hash %}  
  
  var key = "{{key | first}}"  //key
  var values = {} //values object
  values["row"] = {{key.row}}
  values["col"] = {{key.col}}
  values["frames"] = {{key.frames}}
  mario_metadata[key] = values; //key with values added

  {% endfor %}

  ////////// game object for player /////////

  class Mario {
    constructor(meta_data) {
      this.tID = null;  //capture setInterval() task ID
      this.positionX = 0;  // current position of sprite in X direction
      this.currentSpeed = 0;
      this.marioElement = document.getElementById("mario"); //HTML element of sprite
      this.pixels = {{pixels}}; //pixel offset of images in the sprite, set by liquid constant
      this.interval = 100; //animation time interval
      this.obj = meta_data;
      this.marioElement.style.position = "absolute";
    }

    animate(obj, speed) {
      let frame = 0;
      const row = obj.row * this.pixels;
      this.currentSpeed = speed;

      this.tID = setInterval(() => {
        const col = (frame + obj.col) * this.pixels;
        this.marioElement.style.backgroundPosition = `-${col}px -${row}px`;
        this.marioElement.style.left = `${this.positionX}px`;

        this.positionX += speed;
        frame = (frame + 1) % obj.frames;

        const viewportWidth = window.innerWidth;
        if (this.positionX > viewportWidth - this.pixels) {
          document.documentElement.scrollLeft = this.positionX - viewportWidth + this.pixels;
        }
      }, this.interval);
    }

    startWalking() {
      this.stopAnimate();
      this.animate(this.obj["Walk"], 3);
    }
    startWalkingLeft() {
      this.stopAnimate();
      this.animate(this.obj["WalkL"], -3);
    }

    startRunning() {
      this.stopAnimate();
      this.animate(this.obj["Run1"], 6);
    }

    startRunningLeft() {
      this.stopAnimate();
      this.animate(this.obj["Run1L"], -6);
    }

    startPuffing() {
      this.stopAnimate();
      this.animate(this.obj["Puff"], 0);
    }

    startCheering() {
      this.stopAnimate();
      this.animate(this.obj["Cheer"], 0);
    }

    startCheeringL() {
      this.stopAnimate();
      this.animate(this.obj["CheerL"], 0);
    }

    startFlipping() {
      this.stopAnimate();
      this.animate(this.obj["Flip"], 0);
    }

    startFlippingL() {
      this.stopAnimate();
      this.animate(this.obj["FlipL"], 0);
    }

    startResting() {
      this.stopAnimate();
      this.animate(this.obj["Rest"], 0);
    }

    stopAnimate() {
      clearInterval(this.tID);
    }
  }

  const mario = new Mario(mario_metadata);

  ////////// event control /////////

  window.addEventListener("keydown", (event) => {
    if (event.key === "ArrowRight" || event.key==="d") {   //here I changed it to be right arrow key OR D key, same applied for other keys like down and left
      event.preventDefault();
      if (event.repeat) {
        mario.startCheering();
      } else {
        if (mario.currentSpeed === 0) {
          mario.startWalking();
        } else if (mario.currentSpeed === 3) {
          mario.startRunning();
        }  else if (mario.currentSpeed === -3) {
          mario.startWalking();
        } else if (mario.currentSpeed === -6) {
          mario.startWalking();
        }
      }
    } else if (event.key === "s"||event.key==="ArrowDown") {
      event.preventDefault();
      if (event.repeat) {
        mario.stopAnimate();
      } else {
        mario.startFlippingL();
      }
    } else if (event.key === "w"||event.key==="ArrowUp") {
      event.preventDefault();
      if (event.repeat) {
        mario.stopAnimate();
      } else {
        mario.startFlipping();
      }
    } else if (event.key === "Space" || event.key===" ") {
      event.preventDefault();
      if (event.repeat) {
        mario.stopAnimate();
      } else {
        mario.startPuffing();
      }
    } else if (event.key === "ArrowLeft" || event.key==="a") {
      event.preventDefault();
      if (event.repeat) {
        mario.startCheeringL();
      } else {      // what happens in this giant if and else if statment is that based on the current speed of the sprite (like if its stationary, walking in the correct direction, or walking/running in the opposite directoion) it determines whether it will walk or run in the left direction
        if (mario.currentSpeed === 0) {
          mario.startWalkingLeft();
        } else if (mario.currentSpeed === -3) {
          mario.startRunningLeft();
        } else if (mario.currentSpeed === 3) {
          mario.startWalkingLeft();
        } else if (mario.currentSpeed === 6) {
          mario.startWalkingLeft();
        }
      }
    }
  });

  //touch events that enable animations
  window.addEventListener("touchstart", (event) => {
    event.preventDefault(); // prevent default browser action
    if (event.touches[0].clientX > window.innerWidth / 2) {
      // move right
      if (currentSpeed === 0) { // if at rest, go to walking
        mario.startWalking();
      } else if (currentSpeed === 3) { // if walking, go to running
        mario.startRunning();
      }
    } else {
      // move left
      mario.startPuffing();
    }
  });

  //stop animation on window blur
  window.addEventListener("blur", () => {
    mario.stopAnimate();
  });

  //start animation on window focus
  window.addEventListener("focus", () => {
     mario.startFlipping();
  });

  //start animation on page load or page refresh
  document.addEventListener("DOMContentLoaded", () => {
    // adjust sprite size for high pixel density devices
    const scale = window.devicePixelRatio;
    const sprite = document.querySelector(".sprite");
    sprite.style.transform = `scale(${0.4*scale})`;
    sprite.style.zIndex = "2";
    mario.startResting();
  });
</script>

