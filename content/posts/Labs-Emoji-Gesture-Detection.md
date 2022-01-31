---
title: "MediaPipe - Emoji Gestures"
date: "22 Jan 2022"
showDate: true
comments: false
showSocial: true
showMeta: true
categories:
  - projects
tags:
  - c-sharp
  - unity
  - magicleap
  - ar
  - labs 
  - solarflare-studio
autoThumbnailImage: false 
thumbnailImagePosition: "top"
thumbnailImage: https://i.imgur.com/SQawQqZ.jpg
coverImage: https://i.imgur.com/IeCo5mZ.png
metaAlignment: center
---
<!--more-->
{{< toc >}}

# Project Information
This labs project, much like the nanoleaf football project, was to be a fast project with a quick delivery. Due to this, I had to sacrifice code quality for quick output.

The project was to use Googles AI (MediaPipe) to use hand tracking and produce an exciting output. In the end we developed a hand gesture recognition that displayed te emoji the use ris signing to their webcam.

{{< image classes="fancybox center clear" src="https://i.imgur.com/rjogsWb.gif" thumbnail="https://i.imgur.com/rjogsWb.gif" group="group:labs-emojigesture" thumbnail-width="70%" thumbnail-height="70%" title="GIF showing complete emoji experience" >}}

The gif above showcases the complete emoji experience.

# Development:
The project went through several creative changes. At one point we were planning to develop a system with the webcam to translate 3d rendered objects on the canvas. that being said the emoji idea was more entertaining and liked among the team.

This project was pretty much solo developed by myself, with the help of the integrated designer to create the emoji assets. This labs project had a very quick delivery.

The end plan was to produce a video showcasing the emoji system being used by multiple members of the team. We then want to comp the footage onto a video call to make it appear as if this is a featuer of gogle meet. This video will then be shared on social platforms as a thought-provoking piece for people to discuss and take inspiration from.

## Setup MediaPipe
Using MediaPipes documentation I can easily set up MediaPipe hand detection in JavaScript. To start with I implemented the sample code from the docs. From their I experimented and gradually expanded the code to fit hte needs of what we want to develop for the labs project.

{{< codeblock "index.js" "javascript" "" "index.js" >}}
const hands = new Hands({locateFile: (file) => {
    return `https://cdn.jsdelivr.net/npm/@mediapipe/hands/${file}`;
}});

hands.setOptions({
    selfieMode:true,
    maxNumHands: 1,
    modelComplexity: 1,
    minDetectionConfidence: 0.5,
    minTrackingConfidence: 0.5,
    useCpuInference: false,
});
hands.onResults(onResults);

const camera = new Camera(videoElement, {
    onFrame: async () => {
        await hands.send({image: videoElement});
    },
    width: 1280,
    height: 720
});
camera.start();
{{< /codeblock >}}
This code above is responsible for setting the hand landmarkers on the video container.

{{< codeblock "index.js" "javascript" "" "index.js" >}}
/***
* Received the hand landmarks
* @param results
  */
  function onResults(results) {
  width=results.image.width;
  height=results.image.height;

  if(width!=canvasElement.width){
  canvasElement.width=width;
  canvasElement.height=height;
  }

  canvasCtx.save();
  canvasCtx.clearRect(0, 0, canvasElement.width, canvasElement.height);
  canvasCtx.drawImage(results.image, 0, 0, width, height);

  if (results.multiHandLandmarks) {
  for (const landmarks of results.multiHandLandmarks) {
  drawConnectors(canvasCtx, landmarks, HAND_CONNECTIONS, {color: '#00FF00', lineWidth: 2});

       drawLandmarks(canvasCtx, landmarks, {color: '#FF0000', lineWidth: 1, radius: 2});
       // Gets the index and thumb points
       calcLandmarks = landmarks
{{< /codeblock >}}

The code above handles drawing the landmarkers to the canvas. Additionally, they are drawn every frame(second) so they follow the users hand exactly as its moved around the window.

With this implemented the base code for hand tracking from MediaPipe is complete and integrated.

## Emoji Gestures
When it comes to recognising gestures I took the simplest approach I could think of. This was to measure the distance between multiple given landmarks on the hand. I could then have conditionals in place to judge when certain fingers are closing etc and working out the hand gesture from that.

An example is seen below:
{{< codeblock "index.js" "javascript" "" "index.js" >}}
/***
* Function responsible for -
* Defining various hand points
* Returning string value of the hand emoji user is showing
* @param landmarks
* @returns {string|null}
  */
function detectHandGesture(landmarks){
  // Creating multiple positional VARS to create hand gestures

  // Fingers distance from base of palm
  let middleFingerToPalm = calculateDistanceToBasePalm(landmarks[12]);
  let ringFingerToPalm = calculateDistanceToBasePalm(landmarks[16]);
  let indexFingerToPalm = calculateDistanceToBasePalm(landmarks[8]);
  let pinkyFingerToPalm = calculateDistanceToBasePalm(landmarks[20]);
  let thumbToPalm = calculateDistanceToBasePalm(landmarks[4])

  // Rock Sign
  if (middleFingerToPalm <=200 && ringFingerToPalm <= 200 && pinkyFingerToPalm >= 200 && indexFingerToPalm >=200){
  return "Rock"
  }
}
{{< /codeblock >}}

the code above is working out the distance from each finger to the palm of the hand. When the distance is less than or greater than 200 i can detect if the fingers are closed or open. From here I can tell when the user puts their hand in a rock n roll type gesture.

I then return the emoji being signed. This return statement is fed to a relative path to fetch and display the current emoji.

{{< image classes="fancybox center clear" src="https://i.imgur.com/LTiWnXk.png" thumbnail="https://i.imgur.com/LTiWnXk.png" group="group:labs-emojigesture" thumbnail-width="70%" thumbnail-height="70%" title="Image showing hand gesture being recognised" >}}

## Emoji Animation
In regard to the emoji animation, this was fairly simple. I am using an external script that instantiates and places the emoji off-screen relative to the users hand location.

This means that as the user moves their hand the emojis instantiate in that updates location. I then play an animation I created ni CSS that makes the emojis fall below the users view. On the animation end I destroy the emojis.

{{< codeblock "emoji.js" "javascript" "" "emoji.js" >}}
/***
* Function handles converting from pixels to VW
* @param px
* @returns {number}
  */
function convertPXToVW(px) {
  return px * (100 / document.documentElement.clientWidth);
}
let i = 0

let parentDiv
let sheet

/***
* Function handles creating parent div and configuring DOM to start displaying emojis
  */
export function setupEmojiSpawn(){
  let element = document.createElement('style');

  // Append style element to head
  document.head.appendChild(element);

  // Reference to the stylesheet
  sheet = element.sheet;
  parentDiv = document.createElement('div');

{{< /codeblock >}}
The code above starts by creating a div. This div is the parent container to all the emojis that are created.

{{< codeblock "index.js" "javascript" "" "index.js" >}}
export function emojiSpawner(baseHand, emojiToSpawnSrc) {
    let div = document.createElement('img');
    div.setAttribute("src", emojiToSpawnSrc)
    div.id = 'emoji' + i;
    div.className = 'emojiparent';

    styles += 'animation-duration: ' + (getRandomInt(4, 7) + 1) + 's;'
    styles += 'animation-timing-function: linear;'
    styles += 'animation-iteration-count: 1;'
    styles += 'animation-play-state: running;'
    styles += '}';

    sheet.insertRule(styles, i);
    i++

    // Calculate Pixels to display emojis with a given View port width
    let leftStart = baseHand.x*document.documentElement.clientWidth * 100 / 100
    div.style.setProperty('--left-start',  leftStart + 'px')
    div.style.setProperty('--left-end', leftStart + 'px')
    document.getElementById(parentDiv.id).appendChild(div)

    // Find animation, on end destroy div element
    let currentAnimation = document.getElementById(div.id)
    currentAnimation.addEventListener('animationend', () => {
        currentAnimation.remove()
    });
{{< /codeblock >}}
This code above is called every time a hand gesture is recognised. It handles creating the image and plaing it correctly relative to the users hand. 

{{< image classes="fancybox center clear" src="https://i.imgur.com/AdkVWNE.gif" thumbnail="https://i.imgur.com/AdkVWNE.gif" group="group:labs-emojigesture" thumbnail-width="70%" thumbnail-height="70%" title="GIF showing emoji animation" >}}

# Showcase:
{{< youtube 2Jld8vxRT8c >}}
{{< image classes="fancybox center fig-50" src="https://i.imgur.com/QQKYdY8.png" thumbnail="https://i.imgur.com/QQKYdY8.png" group="group:labs-emojigesture" >}}
{{< image classes="fancybox center clear fig-50" src="https://i.imgur.com/ISILTTn.png" thumbnail="https://i.imgur.com/ISILTTn.png" group="group:labs-emojigesture" >}}

# Extra Information
{{< image classes="fancybox nocaption fig-25" src="https://i.imgur.com/apl8UH2t.png" thumbnail="https://i.imgur.com/apl8UH2t.png" group="group:labs-nanoleaf-football" >}}
{{< image classes="fancybox nocaption clear fig-25" src="https://i.imgur.com/jBfEt6S.gif" thumbnail="https://i.imgur.com/jBfEt6S.gif" group="group:labs-nanoleaf-football" >}}
[Solarflare Studio](https://solarflarestudio.co.uk/)

[//]: # ([![SFS]&#40;https://i.imgur.com/apl8UH2t.png&#41;]&#40;https://solarflarestudio.co.uk/&#41;)

[//]: # ()
[//]: # ({{< image classes="fancybox left fig-20" src="https://i.imgur.com/jBfEt6S.gif" thumbnail="https://i.imgur.com/jBfEt6S.gif" group="group:labs-nanoleaf-football" >}})


[//]: # (## Repo/Download Source:)

[//]: # (GitHub: https://github.com/Orakeshi/Nanoleaf-PremierLeague)

[//]: # ()
[//]: # (Download: https://github.com/Orakeshi/Nanoleaf-PremierLeague/archive/refs/heads/main.zip)


