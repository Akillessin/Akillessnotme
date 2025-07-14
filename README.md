# Akillessnotme
Not made by me
// ==UserScript==
// @name         Taming.io Keystrokes
// @namespace    http://tampermonkey.net/
// @version      0.2
// @description  A simple script to see visually the keys you have pressed. (Stylish for recording, streaming, etc.)
// @author       You
// @match        https://taming.io
// @icon         https://taming.io/img/item/amber-spear.png
// @grant        none
// @downloadURL https://update.greasyfork.org/scripts/466231/Tamingio%20Keystrokes.user.js
// @updateURL https://update.greasyfork.org/scripts/466231/Tamingio%20Keystrokes.meta.js
// ==/UserScript==

class cps {
  constructor() {
    this.counter = setInterval(function () {
      seconds++;
    }, 1000);

    this.reset = function () {
      lmc = 0;
      rmc = 0;
      seconds = 0;
    };

    this.lmb = function () {
      return Math.ceil(lmc / seconds);
    };

    this.rmb = function () {
      return Math.ceil(rmc / seconds);
    };
  }
}

let seconds = 0;
let lmc = 0;
let rmc = 0;

const k = {};
const config = {
  //Use "https://keycode.info/" to find the key values
  //(Specifically "event.key" value)

  up: "w",               // -> ArrowUp
  left: "a",             // -> ArrowLeft
  down: "s",             // -> ArrowDown
  right: "d",            // -> ArrowRight
  pressColor: "#424242", // -> Color when key is pressed
  idleColor: "#000000",  // -> Color when key isn't pressed
  opacity: 0.8,          // -> Opacity of the keys (0 being invisible)
  offsetX: 0,            // -> Fine tune the position (X = movement to right)
  offsetY: 0,            // -> Fine tune the position (Y = movement down)
}

let keys = {
}

const counter = new cps();
const css = `
.keystrokes {
  width:240px;
  height: 240px;
  text-align: center;
  vertical-align: middle;
  font-family: "Lato", sans-serif;
  pointer-events: none;
  user-select: none;
  position: absolute;
  bottom: 50%;
  top: ${272.8 + config.offsetY}px;
  left: ${-30 + config.offsetX}px;
  z-index: 999;
  opacity: ${config.opacity};
  animation: rainbow 10s infinite;
  font-weight: 500;
}
.${config.up},
.${config.left},
.${config.down},
.${config.right},
.space,
.lmb,
.rmb {
  position: absolute;
  font-family: "Lato", sans-serif;
  transition: 0.3s;
  background: ${config.idleColor};
  border-radius: 5px;
}
 
.${config.up},
.${config.left},
.${config.down},
.${config.right} {
  width: 58px;
  height: 58px;
  line-height: 60px;
  font-size: 20px;
  font-weight: 1000;
}
.lmb,
.rmb {
  font-size: 14px;
  line-height: 40px;
}
.${config.up} {
  left: 100px;
  top: 15px;
}
.${config.left} {
  left: 35px;
  top: 80px;
}
.${config.down} {
  left: 100px;
  top: 80px;
}
.${config.right} {
  left: 165px;
  top: 80px;
}
.lmb {
  width: 89.5px;
  height: 40px;
  top: 145px;
  left: 34.5px;
}
.rmb {
  width: 89.5px;
  height: 40px;
  top: 145px;
  left: 132.5px;
}
.space {
  width: 187px;
  height: 40px;
  top: 193px;
  left: 35px;
}
 
@keyframes rainbow
{
0%
{
color: #ff0000;
}
 
10%
{
color: #fc7b03;
}
 
20%
{
color: #fcf803;
}
 
30%
{
color: #98fc03;
}
 
40%
{
color: #03fc28;
}
 
50%
{
color: #03fccf;
}
 
60%
{
color: #03c2fc;
}
 
70%
{
color: #033dfc;
}
 
80%
{
color: #6b03fc;
}
 
90%
{
color: #e803fc;
}
 
100%
{
color: #fc031c;
}
 
}
`;

const keystrokes = document.createElement("div");
keystrokes.className = "keystrokes";
keystrokes.innerHTML = `
<div class="${config.up}">
    W
  </div>
  <!-- A -->
  <div class="${config.left}">
    A
  </div>
  <!-- S -->
  <div class="${config.down}">
    S
  </div>
  <!-- D -->
  <div class="${config.right}">
    D
  </div>
  <!-- LMB -->
  <div class="lmb">
    <span id="lmb">LMB: 0</span>
  </div>
  <!-- RMB -->
  <div class="rmb">
   <span id="rmb">RMB: 0</span>
  </div>
  <!-- SPACE -->
  <div class="space">
    <svg xmlns="http://www.w3.org/2000/svg" width="40px" style="position: absolute; top: 0px; left: 72.5px;" viewBox="0 0 512 512">
      <polygon fill="var(--ci-primary-color, currentColor)" points="40 288 40 416 464 416 464 288 432 288 432 384 72 384 72 288 40 288" class="ci-primary" />
    </svg>
  </div>
`;
 
function visual() {
  let s = document.createElement("style");
  s.innerHTML = css;
  document.head.appendChild(s);
  document.body.appendChild(keystrokes);
}
window.addEventListener("contextmenu", function(e) {
  e.preventDefault();
}); 
 
window.addEventListener("keydown", function (e) {
  k[e.key] = e.type == "keydown";
 
  if (k[config.up]) {
    keys.up.style.backgroundColor = config.pressColor;
  }
  if (k[config.left]) {
    keys.left.style.backgroundColor = config.pressColor;
  }
  if (k[config.down]) {
    keys.down.style.backgroundColor = config.pressColor;
  }
  if (k[config.right]) {
    keys.right.style.backgroundColor = config.pressColor;
  }
  if (k[" "]) {
    keys.space.style.backgroundColor = config.pressColor;
  }
});

window.addEventListener("keyup", function (e) {
  k[e.key] = e.type == "keydown";

  if (e.key != " ") {
    document.getElementsByClassName(e.key)[0].style.background = config.idleColor;
  } else {
    keys.space.style.background = config.idleColor;
  }
});

window.addEventListener("mousedown", function(e) {
	if (e.button === 0) {
  	lmc++;
    document.getElementsByClassName("lmb")[0].style.background = config.pressColor;   
  } else if (e.button === 2) {
  	rmc++;
    document.getElementsByClassName("rmb")[0].style.background = config.pressColor;
  }  
});

window.addEventListener("mouseup", function(e) {
		if (e.button === 0) {
    	document.getElementsByClassName("lmb")[0].style.background = config.idleColor;
    } else if (e.button === 2) {
    	document.getElementsByClassName("rmb")[0].style.background = config.idleColor;
    }
})

window.onload = function() {
  visual();

  keys = {
    up: document.getElementsByClassName(config.up)[0],
    left: document.getElementsByClassName(config.left)[0],
    down: document.getElementsByClassName(config.down)[0],
    right: document.getElementsByClassName(config.right)[0],
    space: document.getElementsByClassName("space")[0],
    lmb: document.getElementsByClassName("lmb")[0],
    rmb: document.getElementsByClassName("rmb")[0]
  };

  setInterval(function () {
    if (counter.lmb) {
      document.getElementById("lmb").innerHTML = 
      "LMB: " + counter.lmb();
    }

    if (counter.rmb) {
      document.getElementById("rmb").innerHTML =
      "RMB: " + counter.rmb();
    }
    
    counter.reset();
  }, 1000);
}
