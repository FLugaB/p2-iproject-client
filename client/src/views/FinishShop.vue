<template>
  <div class="fullpage-wrapper px-0 pb-0">
    <div class="wrapper" v-cloak v-bind:class="{'is-previous': isPreviousSlide, 'first-load': isFirstLoad}">
        <div class="slide-wrapper" 
                 v-for="(slide, index) in slides" 
                 :key="index"
                 v-bind:class="{ active: index === currentSlide }"
                 v-bind:style="{ 'z-index': (slides.length - index), 'background-image': 'url(' + slide.bgImg + ')' }">
                <div class="slide-inner">
                    <div class="slide-bg-text">
                        <p>{{ slide.headlineFirstLine }}</p>
                        <p>{{ slide.headlineSecondLine }}</p>
                    </div>
                    <div class="slide-rect-filter">
                        <div class="slide-rect" v-bind:style="{'border-image-source': 'url(' + slide.rectImg + ')'}"></div>
                    </div>
                    <div class="slide-content">
                        <h1 class="slide-content-text"><p>{{ slide.headlineFirstLine }}</p><p>{{ slide.headlineSecondLine }}</p></h1><router-link to="/"><a class="slide-content-cta">Adopt Me !</a></router-link></div>
                    <h2 class="slide-side-text"><span>{{ slide.sublineFirstLine }} / </span><span>{{ slide.sublineSecondLine }}</span></h2></div>
        </div>
        <div class="controls-container">
            <button class="controls-button" 
                    v-for="(slide, index) in slides"
                    :key="index"
                    v-bind:class="{ active: index === currentSlide }"
                    v-on:click="updateSlide(index)">{{ slide.headlineFirstLine }} {{ slide.headlineSecondLine }}</button>
        </div>
        <div class="pagination-container">
            <span class="pagination-item"
                  v-for="(slide, index) in slides"
                  :key="index"
                  v-bind:class="{ active: index === currentSlide }"
                  v-on:click="updateSlide(index)"></span>
        </div>
    </div>
  </div>
</template>

<script>
import {  mapState } from "vuex";
export default {
    data(){
        return {
            currentSlide: 0,
            isPreviousSlide: false,
            isFirstLoad: true,
            slides: [
                {
                    headlineFirstLine: "Puppy",
                    headlineSecondLine: "Pug",
                    sublineFirstLine: "Sad pug, need some warmth",
                    sublineSecondLine: "3200000",
                    bgImg: "https://images.unsplash.com/photo-1453227588063-bb302b62f50b?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=870&q=80",
                    rectImg: "https://images.unsplash.com/photo-1453227588063-bb302b62f50b?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=870&q=80"
                },
                {
                    headlineFirstLine: "Puppy",
                    headlineSecondLine: "Hidding",
                    sublineFirstLine: "Innocent dog is hiding himself.",
                    sublineSecondLine: "2200000",
                    bgImg: "https://images.unsplash.com/photo-1636263931579-026c9ff96269?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1842&q=80",
                    rectImg: "https://images.unsplash.com/photo-1636263931579-026c9ff96269?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1842&q=80"
                },
                {
                    headlineFirstLine: "White",
                    headlineSecondLine: "Bunny",
                    sublineFirstLine: "White Bunny",
                    sublineSecondLine: "1100000",
                    bgImg: "https://images.unsplash.com/photo-1511542229800-663a99ca1817?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=2070&q=80",
                    rectImg: "https://images.unsplash.com/photo-1511542229800-663a99ca1817?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=2070&q=80"
                }
            ]
        }
    },
    mounted: function () {
        var productRotatorSlide = document.getElementById("app");
        var startX = 0;
        var endX = 0;

        productRotatorSlide.addEventListener("touchstart", (event) => startX = event.touches[0].pageX);

        productRotatorSlide.addEventListener("touchmove", (event) => endX = event.touches[0].pageX);

        productRotatorSlide.addEventListener("touchend", function(event) {
            console.log(event)
            var threshold = startX - endX;

            if (threshold < 150 && 0 < this.currentSlide) {
                this.currentSlide--;
            }
            if (threshold > -150 && this.currentSlide < this.slides.length - 1) {
                this.currentSlide++;
            }
        }.bind(this));
    },
      methods: {
        updateSlide(index) {
            index < this.currentSlide ? this.isPreviousSlide = true : this.isPreviousSlide = false;
            this.currentSlide = index;
            this.isFirstLoad = false;
        }
    },
         computed: {
    
    ...mapState(["productData"]),
  
  },
}
</script>

<style>
html {
  box-sizing: border-box;
}

*,
*:before,
*:after {
  box-sizing: inherit;
}

.controls-button, .slide-side-text, .slide-content-cta {
  font-family: "Montserrat";
  text-transform: uppercase;
  color: #fff;
  letter-spacing: 0.12rem;
  font-size: 0.7rem;
  line-height: 1;
}

[v-cloak] {
  opacity: 0;
}

.fullpage-wrapper {
  cursor: default;
  overflow: hidden;
}
.fullpage-wrapper ::-moz-selection {
  background-color: rgba(46, 49, 52, 0.7);
  color: #f5f5f1;
}
.fullpage-wrapper ::selection {
  background-color: rgba(46, 49, 52, 0.7);
  color: #f5f5f1;
}
.fullpage-wrapper ::-moz-selection {
  background-color: rgba(46, 49, 52, 0.7);
  color: #f5f5f1;
}

.wrapper {
  height: calc(100vh - 50px);
  min-height: 36rem;
  position: relative;
}
@media (max-width: 630px) {
  .wrapper {
    height: 100vh;
    min-height: 0;
  }
}

.slide-wrapper {
  background-size: cover;
  height: 100%;
  background-position: center center;
  position: absolute;
  width: 100%;
  background-blend-mode: darken;
}
.slide-wrapper:nth-child(1) {
  background-color: rgba(115, 129, 153, 0.4);
}
.slide-wrapper:nth-child(1):before {
  background-color: rgba(115, 129, 153, 0.25);
}
.slide-wrapper:nth-child(1) .slide-content-text {
  text-shadow: 2px 5px 45px rgba(85, 96, 113, 0.25);
}
.slide-wrapper:nth-child(2) {
  background-color: rgba(144, 171, 184, 0.7);
}
.slide-wrapper:nth-child(2):before {
  background-color: rgba(144, 171, 184, 0.3);
}
.slide-wrapper:nth-child(2) .slide-content-text {
  text-shadow: 2px 5px 45px rgba(121, 142, 152, 0.2);
}
.slide-wrapper:nth-child(3) {
  background-color: rgba(86, 125, 156, 0.5);
}
.slide-wrapper:nth-child(3):before {
  background-color: rgba(86, 125, 156, 0.2);
}
.slide-wrapper:nth-child(3) .slide-content-text {
  text-shadow: 2px 5px 55px rgba(57, 83, 103, 0.4);
}
.slide-wrapper:before {
  content: "";
  position: absolute;
  width: 100%;
  height: 100%;
  z-index: 1;
}
.slide-inner {
  position: relative;
  z-index: 2;
  height: 100%;
  overflow: hidden;
}
.slide-bg-text {
  font-family: "Playfair Display";
  color: #000;
  font-size: 42vh;
  line-height: 0.8;
  opacity: 0.03;
  font-weight: 900;
  margin-top: -4rem;
  position: absolute;
  top: 50%;
  left: 5vw;
  transform: translateY(-50%);
}
.slide-bg-text > p:last-child {
  padding-left: 4rem;
}
.slide-content {
  color: #fff;
  margin-top: 5rem;
  position: absolute;
  top: 50%;
  left: calc(13vw + (.7) * 48vh);
  transform: translateY(-50%);
  display: flex;
  flex-direction: column;
}
@media (max-width: 1000px) {
  .slide-content {
    left: calc(13vw + 1rem);
  }
}
@media (max-height: 730px) {
  .slide-content {
    top: 30%;
    transform: translateY(-30%);
    left: calc(9vw + (.7) * 16vw);
  }
}
.slide-content-text {
  font-family: "Playfair Display";
  font-size: 9rem;
  letter-spacing: 0.2rem;
  line-height: 0.87;
  font-weight: 700;
  will-change: auto;
}
@media (max-height: 790px) {
  .slide-content-text {
    font-size: 7rem;
  }
}
@media (max-width: 1150px) {
  .slide-content-text {
    font-size: 7rem;
  }
}
@media (max-width: 840px) {
  .slide-content-text {
    font-size: 5.5rem;
  }
}
@media (max-width: 630px) {
  .slide-content-text {
    margin-bottom: 5.5rem;
  }
}
@media (max-width: 500px) {
  .slide-content-text {
    font-size: 3.5rem;
  }
}
.slide-content-text > p:last-child {
  padding-left: 3rem;
}
.slide-content-cta {
  cursor: pointer;
  align-self: flex-start;
  margin-top: 4.5rem;
  margin-left: calc((.3) * 48vh + 4.5rem);
  padding: 0.9rem 2.2rem;
  border-left: 1px solid #fff;
  border-right: 1px solid #fff;
  transition: 0.18s ease-in-out;
  font-weight: 700;
}
@media (max-width: 1000px) {
  .slide-content-cta {
    background-color: rgba(255, 255, 255, 0.3);
    padding-top: 1.2rem;
    padding-bottom: 1.2rem;
  }
}
@media (max-width: 630px) {
  .slide-content-cta {
    display: none;
  }
}
.slide-content-cta:hover {
  color: #000;
  background-color: #fff;
}
.slide-rect {
  height: 62vh;
  width: 48vh;
  border-image-slice: 10%;
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  left: 13vw;
  border-width: 5vh;
  border-style: solid;
  box-shadow: 2px 2px 90px 30px rgba(41, 50, 61, 0.22);
  will-change: auto;
}
@media (max-height: 790px) {
  .slide-rect {
    left: 9vw;
    height: 20vw;
    width: 16vw;
    border-width: 5vh;
  }
}
@media (max-height: 730px) {
  .slide-rect {
    top: 30%;
    transform: translateY(-30%);
  }
}
.slide-rect-filter {
  filter: brightness(110%) contrast(110%) saturate(110%);
}
.slide-side-text {
  position: absolute;
  left: calc(13vw - 3rem);
  -ms-writing-mode: tb-rl;
      writing-mode: vertical-rl;
  top: calc((50% - (62vh / 2)) + (5vh / 2));
}
@media (max-height: 790px) {
  .slide-side-text {
    left: calc(9vw - 3rem);
    top: calc((50% - (20vw / 2)) + (5vh / 2));
  }
}
@media (max-height: 730px) {
  .slide-side-text {
    top: calc((40% - (20vw / 2)) + (5vh / 2));
  }
}
.slide-side-text > span:first-child {
  font-weight: 700;
}
.slide-side-text:after {
  content: "";
  width: 1px;
  background-color: #fff;
  height: 40px;
  display: block;
  position: absolute;
  top: calc(100% + 25px);
  left: 50%;
  transform: translateX(-50%);
}

.controls-container {
  position: absolute;
  z-index: 200;
  display: flex;
  bottom: 0;
  right: 0;
  align-items: flex-end;
}
@media (max-width: 630px) {
  .controls-container {
    display: none;
  }
}
.controls-button {
  cursor: pointer;
  background-color: rgba(208, 206, 204, 0.32);
  border: 0;
  padding: 1.6rem 2.2rem;
  flex-basis: 0;
  flex-grow: 1;
  min-width: 15rem;
  transition: 0.25s ease-in-out;
  outline: 0;
}
@media (max-width: 730px) {
  .controls-button {
    padding: 1.2rem 1.4rem;
    min-width: 13rem;
  }
}
.controls-button:not(.active):hover {
  color: #000;
  background-color: #fff;
}
.controls-button.active {
  cursor: default;
  font-weight: 700;
  background-color: #3b3e45;
  padding-top: 1.9rem;
  padding-bottom: 1.9rem;
  margin-bottom: -0.3rem;
  position: relative;
}
@media (max-width: 730px) {
  .controls-button.active {
    padding-top: 1.4rem;
    padding-bottom: 1.4rem;
    margin-bottom: -0.15rem;
  }
}
.controls-button.active:after {
  content: "";
  background-color: #e3e3e3;
  height: 5px;
  width: calc(100% - 8px);
  position: absolute;
  top: 100%;
  left: 4px;
}
.controls-button:not(.active) + .controls-button {
  border-left: 1px solid rgba(255, 255, 255, 0.2);
}

.pagination-container {
  position: absolute;
  z-index: 200;
  display: flex;
  flex-direction: column;
  align-items: flex-end;
  right: 2rem;
  top: 50%;
  transform: translateY(-50%);
}
@media (max-width: 920px) {
  .pagination-container {
    display: none;
  }
}
.pagination-item {
  width: 30px;
  height: 1px;
  background-color: rgba(255, 255, 255, 0.6);
  transition: 0.18s ease-in-out;
}
.pagination-item + .pagination-item {
  margin-top: 1rem;
}
.pagination-item.active {
  background-color: #fff;
  position: relative;
  transform: translateX(-0.6rem);
  width: 35px;
}
.pagination-item.active:after {
  content: "";
  height: 4px;
  width: 2px;
  border-radius: 35%;
  background-color: #fff;
  display: inline-block;
  position: absolute;
  right: 0;
  top: 50%;
  transform: translateX(0.6rem) translateY(-50%);
}
.pagination-item:not(.active) {
  cursor: pointer;
}
.pagination-item:not(.active):hover {
  background-color: #fff;
  width: 35px;
}

@-webkit-keyframes slideLeft {
  from {
    transform: translateX(0);
  }
  to {
    transform: translateX(-100%);
  }
}

@keyframes slideLeft {
  from {
    transform: translateX(0);
  }
  to {
    transform: translateX(-100%);
  }
}
@-webkit-keyframes slideRight {
  from {
    transform: translateX(-100%);
  }
  to {
    transform: translateX(0);
  }
}
@keyframes slideRight {
  from {
    transform: translateX(-100%);
  }
  to {
    transform: translateX(0);
  }
}
@-webkit-keyframes cutTextUp {
  from {
    -webkit-clip-path: inset(0 0 -10% 0);
            clip-path: inset(0 0 -10% 0);
  }
  to {
    -webkit-clip-path: inset(0 0 100% 0);
            clip-path: inset(0 0 100% 0);
  }
}
@keyframes cutTextUp {
  from {
    -webkit-clip-path: inset(0 0 -10% 0);
            clip-path: inset(0 0 -10% 0);
  }
  to {
    -webkit-clip-path: inset(0 0 100% 0);
            clip-path: inset(0 0 100% 0);
  }
}
@-webkit-keyframes cutTextDown {
  from {
    -webkit-clip-path: inset(100% 0 0 0);
            clip-path: inset(100% 0 0 0);
  }
  to {
    -webkit-clip-path: inset(-10% 0 -20% 0);
            clip-path: inset(-10% 0 -20% 0);
    opacity: 1;
  }
}
@keyframes cutTextDown {
  from {
    -webkit-clip-path: inset(100% 0 0 0);
            clip-path: inset(100% 0 0 0);
  }
  to {
    -webkit-clip-path: inset(-10% 0 -20% 0);
            clip-path: inset(-10% 0 -20% 0);
    opacity: 1;
  }
}
@-webkit-keyframes cutTextDownFromTop {
  from {
    -webkit-clip-path: inset(0 0 100% 0);
            clip-path: inset(0 0 100% 0);
  }
  to {
    -webkit-clip-path: inset(0 0 -30% 0);
            clip-path: inset(0 0 -30% 0);
    opacity: 1;
  }
}
@keyframes cutTextDownFromTop {
  from {
    -webkit-clip-path: inset(0 0 100% 0);
            clip-path: inset(0 0 100% 0);
  }
  to {
    -webkit-clip-path: inset(0 0 -30% 0);
            clip-path: inset(0 0 -30% 0);
    opacity: 1;
  }
}
@-webkit-keyframes rectMovement {
  0% {
    transform: translateX(0) rotate(0) translateY(-50%);
  }
  60% {
    opacity: 1;
  }
  100% {
    transform: translateX(calc(-48vh + -13vw)) rotate(12deg) translateY(-50%);
    opacity: 0;
  }
}
@keyframes rectMovement {
  0% {
    transform: translateX(0) rotate(0) translateY(-50%);
  }
  60% {
    opacity: 1;
  }
  100% {
    transform: translateX(calc(-48vh + -13vw)) rotate(12deg) translateY(-50%);
    opacity: 0;
  }
}
@media (max-height: 730px) {
  @-webkit-keyframes rectMovement {
    0% {
      transform: translateX(0) rotate(0) translateY(-30%);
    }
    60% {
      opacity: 1;
    }
    100% {
      transform: translateX(calc(-48vh + -13vw)) rotate(12deg) translateY(-30%);
      opacity: 0;
    }
  }
  @keyframes rectMovement {
    0% {
      transform: translateX(0) rotate(0) translateY(-30%);
    }
    60% {
      opacity: 1;
    }
    100% {
      transform: translateX(calc(-48vh + -13vw)) rotate(12deg) translateY(-30%);
      opacity: 0;
    }
  }
}
@-webkit-keyframes rectMovementFromRight {
  0% {
    transform: translateX(calc(48vh)) rotate(12deg) translateY(-50%);
    opacity: 0;
  }
  60% {
    opacity: 1;
  }
  100% {
    transform: translateX(0) rotate(0) translateY(-50%);
    opacity: 1;
    @media (max-height: 730px) {
      transform: translateX(0) rotate(0) translateY(-30%);
    }
  }
}
@keyframes rectMovementFromRight {
  0% {
    transform: translateX(calc(48vh)) rotate(12deg) translateY(-50%);
    opacity: 0;
  }
  60% {
    opacity: 1;
  }
  100% {
    transform: translateX(0) rotate(0) translateY(-50%);
    opacity: 1;
    @media (max-height: 730px) {
      transform: translateX(0) rotate(0) translateY(-30%);
    }
  }
}
@media (max-height: 730px) {
  @-webkit-keyframes rectMovementFromRight {
    0% {
      transform: translateX(calc(48vh)) rotate(12deg) translateY(-30%);
      opacity: 0;
    }
    60% {
      opacity: 1;
    }
    100% {
      transform: translateX(0) rotate(0) translateY(-30%);
      opacity: 1;
    }
  }
  @keyframes rectMovementFromRight {
    0% {
      transform: translateX(calc(48vh)) rotate(12deg) translateY(-30%);
      opacity: 0;
    }
    60% {
      opacity: 1;
    }
    100% {
      transform: translateX(0) rotate(0) translateY(-30%);
      opacity: 1;
    }
  }
}
@-webkit-keyframes rectMovementRight {
  0% {
    transform: translateX(calc(-48vh + -13vw)) rotate(12deg) translateY(-50%);
  }
  40% {
    opacity: 1;
  }
  100% {
    transform: translateX(0) rotate(0) translateY(-50%);
    opacity: 1;
    @media (max-height: 730px) {
      transform: translateX(0) rotate(0) translateY(-30%);
    }
  }
}
@keyframes rectMovementRight {
  0% {
    transform: translateX(calc(-48vh + -13vw)) rotate(12deg) translateY(-50%);
  }
  40% {
    opacity: 1;
  }
  100% {
    transform: translateX(0) rotate(0) translateY(-50%);
    opacity: 1;
    @media (max-height: 730px) {
      transform: translateX(0) rotate(0) translateY(-30%);
    }
  }
}
@media (max-height: 730px) {
  @-webkit-keyframes rectMovementRight {
    0% {
      transform: translateX(calc(-48vh + -13vw)) rotate(12deg) translateY(-30%);
    }
    40% {
      opacity: 1;
    }
    100% {
      transform: translateX(0) rotate(0) translateY(-30%);
      opacity: 1;
    }
  }
  @keyframes rectMovementRight {
    0% {
      transform: translateX(calc(-48vh + -13vw)) rotate(12deg) translateY(-30%);
    }
    40% {
      opacity: 1;
    }
    100% {
      transform: translateX(0) rotate(0) translateY(-30%);
      opacity: 1;
    }
  }
}
@-webkit-keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}
@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}
.slide-wrapper {
  opacity: 0;
  transition-delay: 1.4s;
  transition-duration: 0s;
  transition-property: opacity;
  will-change: opacity, transform;
}
.slide-wrapper:not(.active) {
  -webkit-animation-delay: 0.5s;
          animation-delay: 0.5s;
  -webkit-animation-name: slideLeft;
          animation-name: slideLeft;
  -webkit-animation-duration: 0.9s;
          animation-duration: 0.9s;
  -webkit-animation-timing-function: cubic-bezier(0.18, 0.54, 0.52, 0.93);
          animation-timing-function: cubic-bezier(0.18, 0.54, 0.52, 0.93);
  pointer-events: none;
}
.slide-wrapper:not(.active) .slide-content-text > p,
.slide-wrapper:not(.active) .slide-side-text {
  -webkit-animation-name: cutTextUp;
          animation-name: cutTextUp;
  -webkit-animation-duration: 0.5s;
          animation-duration: 0.5s;
  -webkit-animation-timing-function: ease-out;
          animation-timing-function: ease-out;
}
.slide-wrapper:not(.active) .slide-rect {
  -webkit-animation-name: rectMovement;
          animation-name: rectMovement;
  -webkit-animation-duration: 0.5s;
          animation-duration: 0.5s;
  -webkit-animation-timing-function: ease;
          animation-timing-function: ease;
  -webkit-animation-fill-mode: forwards;
          animation-fill-mode: forwards;
}
.slide-wrapper.active {
  transition-delay: 0s;
  opacity: 1;
}
.slide-wrapper.active .slide-content-text > p {
  opacity: 0;
  -webkit-animation-delay: 0.8s;
          animation-delay: 0.8s;
  -webkit-animation-name: cutTextDown;
          animation-name: cutTextDown;
  -webkit-animation-duration: 0.5s;
          animation-duration: 0.5s;
  -webkit-animation-timing-function: ease;
          animation-timing-function: ease;
  -webkit-animation-fill-mode: forwards;
          animation-fill-mode: forwards;
}
.slide-wrapper.active .slide-rect {
  opacity: 0;
  -webkit-animation-name: rectMovementFromRight;
          animation-name: rectMovementFromRight;
  -webkit-animation-duration: 0.45s;
          animation-duration: 0.45s;
  -webkit-animation-timing-function: ease;
          animation-timing-function: ease;
  -webkit-animation-fill-mode: forwards;
          animation-fill-mode: forwards;
  -webkit-animation-delay: 0.9s;
          animation-delay: 0.9s;
}
.is-previous .slide-wrapper:not(.active) {
  -webkit-animation: none;
          animation: none;
}
.is-previous .slide-wrapper:not(.active) .slide-rect {
  -webkit-animation: none;
          animation: none;
}
.is-previous .slide-wrapper.active {
  transform: translateX(-100%);
  -webkit-animation-fill-mode: forwards;
          animation-fill-mode: forwards;
  -webkit-animation-delay: 0.5s;
          animation-delay: 0.5s;
  -webkit-animation-name: slideRight;
          animation-name: slideRight;
  -webkit-animation-duration: 0.8s;
          animation-duration: 0.8s;
  -webkit-animation-timing-function: cubic-bezier(0.18, 0.54, 0.52, 0.93);
          animation-timing-function: cubic-bezier(0.18, 0.54, 0.52, 0.93);
}
.is-previous .slide-wrapper.active .slide-rect {
  opacity: 0;
  -webkit-animation-name: rectMovementRight;
          animation-name: rectMovementRight;
  -webkit-animation-duration: 0.5s;
          animation-duration: 0.5s;
  -webkit-animation-timing-function: ease-out;
          animation-timing-function: ease-out;
  -webkit-animation-fill-mode: forwards;
          animation-fill-mode: forwards;
  -webkit-animation-delay: 0.9s;
          animation-delay: 0.9s;
}

.first-load .slide-wrapper.active .slide-side-text,
.first-load .slide-wrapper.active .slide-content-cta,
.first-load .slide-wrapper.active .slide-rect,
.first-load .controls-container {
  opacity: 0;
  -webkit-animation-name: fadeIn;
          animation-name: fadeIn;
  -webkit-animation-delay: 0.3s;
          animation-delay: 0.3s;
  -webkit-animation-duration: 0.3s;
          animation-duration: 0.3s;
  -webkit-animation-fill-mode: forwards;
          animation-fill-mode: forwards;
  -webkit-animation-timing-function: ease-in;
          animation-timing-function: ease-in;
}
.first-load .slide-wrapper.active .slide-content-text > p {
  -webkit-animation-name: fadeIn;
          animation-name: fadeIn;
  -webkit-animation-delay: 0.5s;
          animation-delay: 0.5s;
  -webkit-animation-duration: 0.7s;
          animation-duration: 0.7s;
}
</style>