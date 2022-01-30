---
title: "Lego - Home Alone AR"
date: "01 Nov 2021"
showDate: true
comments: false
showSocial: true
showMeta: true
categories:
  - projects
tags:
  - html
  - js
  - css
  - lego
  - ar
  - webar
  - jquery
  - microsite
  - 8thwall
  - solarflare-studio
autoThumbnailImage: false 
thumbnailImagePosition: "top"
thumbnailImage: https://i.imgur.com/ZhrrlDs.jpg
coverImage: https://i.imgur.com/26hEqfS.jpg
metaAlignment: center
---
<!--more-->
{{< toc >}}

# Project Information
This was my first client facing project in my career. I was tasked with developing the entire front end for the micro site as well as the AR games. In combination with the lead developer at SFS we pair programmed the entire experience.

The lego Home Alone experience was a project that lego wanted to be delivered with a 1-month deadline. It was tight with long hours worked but the end result was amazing.

The concept was the following. The micro site would be broken in to 4 parts. These parts are listed below:
- Find Kevin Statue WebAR Game
- Selfie With Kevin Experience
- Selfie Viewer/Share
- Micro site Itself

Another part was the competition aspect, however I wont showcase this as I was not involved with its development.

{{< image classes="fancybox center clear" src="https://i.imgur.com/thYwfVq.gif" thumbnail="https://i.imgur.com/thYwfVq.gif" group="group:legohomealone" thumbnail-width="40%" thumbnail-height="40%" title="GIF showing lego AR game" >}}

# Development:
As mentioned above, the development on this project was completed by myself and the lead developer. We of course has help from the creatives, project managers, integrated designers etc, however in regard to code alone it was done by the lead developer and I. 

The lead dev handled the backend, deployment and keeping the project running on time for the 1 month deadline. As a result I will only write about the stuff I developed below.

## AR Game
The first part of the experience I developed was the AR game. This was created in accordance with [8thwall](https://www.8thwall.com/). 8thwall have an amazing WebAR library that makes developing experiences as simple as possible.

I started by getting conceptually thinking about everything that makes up the experience. That being, movement, collection mechanics, end video etc. With this in mind I started implementing the movement of Kevin from home alone)

{{< image classes="fancybox center clear" src="https://i.imgur.com/QnUSPkH.png" thumbnail="https://i.imgur.com/QnUSPkH.png" group="group:legohomealone" thumbnail-width="40%" thumbnail-height="40%" title="Image showing the joystick movement mechanic" >}}

As seen from the image above, I was able to successfully implement the movement mechanic. This was a simple process due to the 8th wall playground having joystick movement showcases already implemented.

{{< codeblock "move-character.js" "javascript" "" "move-character.js" >}}
const offsetX = rawOffsetX / maxRawDistance
const offsetY = rawOffsetY / maxRawDistance

const forward = -Math.min(Math.max(-1, offsetY), 1)
const side = -Math.min(Math.max(-1, offsetX), 1)

const moveZ = -forward * 0.4
const moveX = -side * 0.4

// get y rot of camera
const camY = this.camera.object3D.rotation.y

let joystickRot = Math.atan2(forward, side)

joystickRot -= camY

const speed = 0.001995 //0.011025

this.el.object3D.position.z -= speed * Math.sin(joystickRot) * timeDelta
this.el.object3D.position.x -= speed * Math.cos(joystickRot) * timeDelta

this.el.object3D.rotation.y = -joystickRot - Math.PI / 2

this.el.setAttribute('animation-mixer', {
clip: 'Run',
loop: 'repeat',
crossFadeDuration: 0.4,
})
{{< /codeblock >}}

This is a part of the script that allows the character to move with the walking animation that hte lead artist at solarflare created. Having GLBs with animations baked into the models allows us to simply access them and ply them as we pleased.

The result of the joystick code above is as follows:
{{< image classes="fancybox center clear" src="https://i.imgur.com/71Wafu2.gif" thumbnail="https://i.imgur.com/71Wafu2.gif" group="group:legohomealone" thumbnail-width="40%" thumbnail-height="40%" title="GIF showing the character movement with the joystick" >}}

The next part of the AR game was collecting objects. We wanted to have the objects picked up and displayed on the UI at the bottom so the user could visually see how many items left they have to collect.

{{< codeblock "object-handler.js" "javascript" "" "object-handler.js" >}}
// When called, instantiate collectable item with box collider
window.spawnItem = function (){
setTimeout(()=> {
const collectable = document.getElementById(items[currentCollectableItem]+"-item") // Get Item objects
const collider = document.getElementById(items[currentCollectableItem]+"-collider") // Get Item Colliders

// Create random coordinates relative to the character spawn point
let currentCord = coords[currentCollectableItem];

collectable.object3D.position.set(currentCord.x, 0, currentCord.z)
collider.object3D.position.set(currentCord.x, 0, currentCord.z)

collectable.setAttribute('animation-mixer', {
  clip: 'Idle',
})

// Make all the collectables visible
collectable.setAttribute('visible', true)
collider.setAttribute('visible', true)
}, 100)

}

spawnItem();
{{< /codeblock >}}

The code above is responsible for instantiating the object for kevin to collect. As they spawn sequentially one at a time, this method is called after Kevins collider has collided and collected an existing object in the scene.

{{< codeblock "collected-item.js" "javascript" "" "collected-item.js" >}}
// Display UI card for collected item
function displayUI(itemName) {
    const message = document.getElementById(itemName + "-message")
    message.style.display = 'block';
    entity.setAttribute('character-move', '')
    currentCollectableItem++;
    window.spawnItem();

    // Remove card etc after 3 seconds
    setTimeout(() => {

        // If the item is aftershave -> Display iconic Kevin face
        if (itemName === "aftershave") {
            let kevin = entity.getObject3D('mesh').getObjectByName("LegoKevin")
            let texture = THREE.ImageUtils.loadTexture(smileSrc)
            texture.flipY = false
            kevin.material.map = texture
        }
        groupNotCollected[currentItem].setAttribute('src', "./assets/ui/" + itemName + "-on-CollectItem.png");
        currentItem++;
        message.style.display = 'none';
        if (totalCollected === totalCollectables) {
            checkCollected();
        }
    }, 1500)
}
{{< /codeblock >}}
This module of the code above is responsible for showing the correct UI card and updating the bottom panels ui of the item collected. With this code in place the collection mechanic is now complete.

{{< image classes="fancybox center clear" src="https://i.imgur.com/rgkuU8e.gif" thumbnail="https://i.imgur.com/rgkuU8e.gif" group="group:legohomealone" thumbnail-width="40%" thumbnail-height="40%" title="GIF showing the collection mechanic" >}}

The final aspect left of the game was to implement the door, allow the suer to walk through it and have a video play & end the game

{{< codeblock "end-game.js" "javascript" "" "end-game.js" >}}
door.addEventListener("hitstart", function(event) {
    const character = document.getElementById("character");
    character.removeAttribute('character-move');
    analytics.sendEvent(analytics.events.arGame.category, analytics.events.arGame.finishedGame);

    videoContainer.style.display = "block";
    video.play();
    
    video.addEventListener('ended', () => {
        body.style.opacity = "0";
        window.location.href='./transition.html';
    })
})
{{< /codeblock >}}

That is now everything finished in regard to the AR game.

{{< image classes="fancybox center clear" src="https://i.imgur.com/3HNROaM.gif" thumbnail="https://i.imgur.com/3HNROaM.gif" group="group:legohomealone" thumbnail-width="40%" thumbnail-height="40%" title="GIF showcasing door spawn and end video" >}}

## Selfie Experience
The selfie experience was a lot simpler than the AR game. Not as much was required to complete the development. The only challenging aspect was using the HTML2Cavnas library to allow me to screenshot the body tag whilst hiding specific elements.

There was only 2 main aspects of the selfie experience. They are recognising face gestures and playing an animation. These are both combined into 1 action, so I completed the development together.

{{< codeblock "face-events.js" "javascript" "" "face-events.js" >}}
AFRAME.registerComponent('face-events', {
    schema: {
    mouthOpen: {type: 'number', default: 0},
    faceFound: {type: 'bool', default: false},
    },

    init() {
        const scene = this.el.sceneEl;

        // Create attributes required for face mesh detection
        this.lower = document.createElement('xrextras-face-attachment')
        this.lower.setAttribute('alignToSurface', 'true')
        this.lower.setAttribute('point', 'lowerLip')
        this.el.appendChild(this.lower)

        const show = ({detail}) => {
            this.data.faceFound = true
        }
        const hide = ({detail}) => {
            this.data.faceFound = false
        }
        // Dispatch various functions when events occur
        scene.addEventListener('xrfacefound', show)
        scene.addEventListener('xrfaceupdated', show)
        scene.addEventListener('xrfacelost', hide)
{{< /codeblock >}}
This code above is repsonsible for registering the component and creating a scheme for when the mouth is open/closed.

{{< codeblock "face-events.js" "javascript" "" "face-events.js" >}}
setInterval(() => {
    // normalize lower lip distance
    const blendAmt = function (value, min, max) {
        return (value - min) / (max - min)
        }
        this.data.mouthOpen = blendAmt(this.lower.object3D.position.y, -0.5, -0.7)

        let mouth = this.data.mouthOpen;
        let face = this.data.faceFound;

        if (face === true) {
            // Mouth is open
            if(mouth > 0.9) {
                mouthOpenNow = true
                AnimateNow();
            }
            // Mouth is closed
            else{
                mouthOpenNow = false
                AnimateNow();
            }

        } else {
            // Not tracking the users face
            StopAnimation()
        }
    }, 85)
{{< /codeblock >}}
The final aspect is the code above. This handles playing the Kevin sequence animation when the persons mouth opens. This works by taking a set of still PNG images and cycling up & down through them to give the impression kevin is moving/animating.

{{< image classes="fancybox center fig-50" src="https://i.imgur.com/8EIHQyN.png" thumbnail="https://i.imgur.com/8EIHQyN.png" group="group:legohomealone" >}}
{{< image classes="fancybox center clear fig-50" src="https://i.imgur.com/UML6VYv.gif" thumbnail="https://i.imgur.com/UML6VYv.gif" group="group:legohomealone" >}}

## Prize Draw
The next part of the experience I worked on was the Prize Draw page. This was the simplest of everything I worked on. All I had to do was develop the front end design & take te input from the text fields + post them to our strapi backend.

{{< codeblock "prizeDraw.js" "javascript" "" "prizeDraw.js" >}}
// If t&c check box left empty
if(!tcCheck.checked){
    console.log("Error: T&C must be checked")
    //errorText.innerHTML="T&Cs box must be checked";
    tcText.style.border = "red 2px solid";
}
/* If form passes validation try and send data to backend */
else{
    let email = document.getElementById("email").value;
    let fullName = document.getElementById("fullname").value;
    let age = document.getElementById("age").value;
    let phone = document.getElementById("phone").value;
    let address = document.getElementById("address").value;

    console.log(email)
    try {
        const entryDataId = await prizeDrawEntry.savePrizeDrawEntry({
            email: email,
            fullName: fullName,
            age: age,
            phone: phone,
            address: address,
        });
        console.log("Entry sent with id " + entryDataId + ".");
        analytics.sendEvent(analytics.events.prizeDraw.category, analytics.events.prizeDraw.joinedPrizeDraw);
    }
    catch (error) {
        errorText.innerHTML="ERROR: Please try again later!";
        console.log("ERROR: " + error)
    }
    /* Display Thank you page */
    form.style.display="none";
    thanksWindow.style.display="block";
}
{{< /codeblock >}}

This code above is responsible for doing exactly that.

{{< youtube g2SmzVJOoJY >}}

## Microsite
The last aspect of the experience to be developed is the overall micro site (that being animations, design, buttons, socials etc)

### Desktop View
One part of the experiecne that has been defined in the requirements is that the destop version of the site should be different to the mobile/tablet view.

Any desktop user should not be able to click on the AR or selfie experience. Additionally, they should only be able to access the prize draw and scan the QR code to open the complete experience on mobile.

To achieve this I simply used javaScript to check the nav user agent to see what device they were on.

{{< codeblock "detectDevice.js" "javascript" "" "detectDevice.js" >}}
/**
* Function that checks the userAgent data to see if the user is on a desktop computer
* If the user is on a desktop they should not be able to access parts of the site
* @returns {string}
*/
  export function detectDevice() {
     let isIOS = /iPad|iPhone|iPod/.test(navigator.platform) ||
         (navigator.platform === 'MacIntel' && navigator.maxTouchPoints > 1)

     const ua = navigator.userAgent;

  if(isIOS){
     return "tablet";
  }

  if (/(tablet|ipad|playbook|silk)|(android(?!.*mobi))/i.test(ua)) {
     return "tablet";
  }
  else if (/Mobile|Android|iP(hone|od)|IEMobile|BlackBerry|Kindle|Silk-Accelerated|(hpw|web)OS|Opera M(obi|ini)/.test(ua)) {
     return "mobile";
  }
     return "desktop";
  };
{{< /codeblock >}}

The above code now detects the device. From there I have conditionals in place to display the desktop or mobile view dependent on the value returned from this function.

{{< image classes="fancybox center clear" src="https://i.imgur.com/nPDFACM.png" thumbnail="https://i.imgur.com/nPDFACM.png" group="group:legohomealone" thumbnail-width="40%" thumbnail-height="40%" title="Image showcasing mobile view of site" >}}

{{< image classes="fancybox center clear" src="https://i.imgur.com/hDiwsem.png" thumbnail="https://i.imgur.com/hDiwsem.png" group="group:legohomealone" thumbnail-width="100%" thumbnail-height="100%" title="Image showing desktop view of site" >}}

# Showcase:
## Desktop Site

{{< youtube 0f15tGyc-UA >}}
{{< image classes="fancybox fig-33" src="https://i.imgur.com/v1PrP15.png" thumbnail="https://i.imgur.com/v1PrP15.png" group="group:legohomealone" >}}
{{< image classes="fancybox fig-33" src="https://i.imgur.com/M4PXjNc.png" thumbnail="https://i.imgur.com/M4PXjNc.png" group="group:legohomealone" >}}
{{< image classes="fancybox clear fig-33" src="https://i.imgur.com/tF5x5cE.png" thumbnail="https://i.imgur.com/tF5x5cE.png" group="group:legohomealone" >}}

## Mobile Site

{{< image classes="fancybox center fig-50" src="https://i.imgur.com/a4plmaw.png?1" thumbnail="https://i.imgur.com/a4plmaw.png?1" group="group:legohomealone" >}}
{{< image classes="fancybox center clear fig-50" src="https://i.imgur.com/wh9qc7z.png?1" thumbnail="https://i.imgur.com/wh9qc7z.png?1" group="group:legohomealone" >}}
{{< image classes="fancybox fig-33" src="https://i.imgur.com/14EdGvR.png?1" thumbnail="https://i.imgur.com/14EdGvR.png?1" group="group:legohomealone" >}}
{{< image classes="fancybox fig-33" src="https://i.imgur.com/h8WIuEX.png?1" thumbnail="https://i.imgur.com/h8WIuEX.png?1" group="group:legohomealone" >}}
{{< image classes="fancybox clear fig-33" src="https://i.imgur.com/bOEAtXm.png?1" thumbnail="https://i.imgur.com/bOEAtXm.png?1" group="group:legohomealone" >}}

## Complete Showcase

{{< youtube FUaHVIvapkY >}}
{{< image classes="fancybox center fig-75" src="https://i.imgur.com/Hz6LUvO.gif" thumbnail="https://i.imgur.com/Hz6LUvO.gif" group="group:legohomealone" >}}
{{< image classes="fancybox center fig-25" src="https://i.imgur.com/DOWui0O.gif" thumbnail="https://i.imgur.com/DOWui0O.gif" group="group:legohomealone" >}}
{{< image classes="fancybox center fig-25" src="https://i.imgur.com/vXH5DZe.png" thumbnail="https://i.imgur.com/vXH5DZe.png" group="group:legohomealone" >}}
{{< image classes="fancybox center clear fig-25" src="https://i.imgur.com/OfcvFK1.jpg" thumbnail="https://i.imgur.com/OfcvFK1.jpg" group="group:legohomealone" >}}


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


