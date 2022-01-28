---
title: "ApocalypseZ"
date: "08 Sep 2021"
showDate: true
comments: false
showSocial: true
showMeta: true
categories:
  - projects
tags:
  - c-sharp
  - unity
  - game-dev
  - vr
  - personal
autoThumbnailImage: false 
thumbnailImagePosition: "top"
thumbnailImage: https://i.imgur.com/S0zaljJ.jpg
coverImage: https://i.imgur.com/IJJwJMH.png
metaAlignment: center
---
<!--more-->
{{< toc >}}

# Project Information
Apocalypse-Z was my first attempt at not only a VR game but also working alone on a large scale project. I had previously gained mass knowledge from doing the majority of development alone, as seen from [Wishing Tree](https://orakeshi.github.io/post/digital-wishing-tree/) & [Masked Ball](https://orakeshi.github.io/post/digital-wishing-tree/).

That being said I had help in the form of people to consult with, designers to create the art and assets etc. With this project I was alone in both development, plan, design, deployment etc. In the end I succeeded in what I set out to achieve.

I wanted to create a VR zombie shooter game for the oculus quest 2. This was both a passion & fun project and a great learning experience for me. As I typically work with XR I thought this would give me a great opportunity to learn about unity and its development process for VR in greater detail.

{{< image classes="fancybox center clear" src="https://i.imgur.com/cHIXbwT.png?1" thumbnail="https://i.imgur.com/cHIXbwT.png?1" group="group:apocalypsez" thumbnail-width="70%" thumbnail-height="70%" title="Image showing the spawn point for the player (Vr Rig)" >}}

# Development:
There is not much to mention in regard to development. Whilst this project ended up being relatively large (200 commits on GitHub) It was not particularly challenging. Everything went smooth in regard to dev work.

## OpenXR
As for OpenXR, this is the unity library I used. This library allows me to build for any VR device I want. I dont have to define specific controls or target a specific platform. The game can be buuilt for Oculus rift, quest go etc aswell as Steam devices such as the index etc.

More effort is required at the initial stages to implement all hte controller model prefabs for the various devices, however from that point on its very simple to start working with.

{{< codeblock "HandPresence.cs" "csharp" "" "HandPresence.cs" >}}
    private void TryInitialize()
    {
        List<InputDevice> devices = new List<InputDevice>();

        InputDevices.GetDevicesWithCharacteristics(controllerCharacteristics, devices);

        foreach (var item in devices)
        {
            Debug.Log(item.name + item.characteristics);
        }


        if (devices.Count > 0)
        {
            targetDevice = devices[0];
            GameObject prefab = controllerPrefabs.Find(controller => controller.name == targetDevice.name);
            if (prefab)
            {
                spawnedController = Instantiate(prefab, transform);
            }
            else
            {
                Debug.Log("Did not find the correct controller model");
                spawnedController = Instantiate(controllerPrefabs[0], transform);
            }

            spawnedHandModel = Instantiate(handModelPrefab, transform);
            handAnimator = spawnedHandModel.GetComponent<Animator>();
        }
    }
{{< /codeblock >}}

The code above is responsible for getting the device characteristics and instantiating the correct controller prefabs for the device being played on. This script also handles spawning hand models when controllers are not being rendered in the scene.

## URP
This part of development dent require much to be written about. There is no code snippets. Much rather this is the aspect of the game in which I focused on the look and style of the game. I decided to use URP (Universal Render Pipeline) to have access to many effects such as bloom etc.

I chose URP over HDRP, simply due to my main device to target was the quest which will struggle to render HDRP textures which are typically alot higher quality.

That being said, with access to URP, I stylised the game to have heavy fog with an orange tint. 

{{< image classes="fancybox center clear" src="https://i.imgur.com/aBWv5os.png?1" thumbnail="https://i.imgur.com/aBWv5os.png?1" group="group:apocalypsez" thumbnail-width="70%" thumbnail-height="70%" title="Image showing the URP effects applied on the map" >}}

# Showcase:
{{< youtube McyWwkK61H8 >}}
{{< image classes="fancybox center fig-33" src="https://i.imgur.com/crsGp23.jpg" thumbnail="https://i.imgur.com/crsGp23.jpg" group="group:apocalypsez" >}}
{{< image classes="fancybox center fig-33" src="https://i.imgur.com/nnrubON.jpg" thumbnail="https://i.imgur.com/nnrubON.jpg" group="group:apocalypsez" >}}
{{< image classes="fancybox center fig-33" src="https://i.imgur.com/hH7rcD9.jpg" thumbnail="https://i.imgur.com/hH7rcD9.jpg" group="group:apocalypsez" >}}
{{< image classes="fancybox center clear" src="https://i.imgur.com/fzLSpZy.gif" thumbnail="https://i.imgur.com/fzLSpZy.gif" group="group:apocalypsez" thumbnail-width="100%" thumbnail-height="100%" >}}
# Extra Information

[//]: # ({{< image classes="fancybox nocaption fig-25" src="https://i.imgur.com/apl8UH2t.png" thumbnail="https://i.imgur.com/apl8UH2t.png" group="group:labs-nanoleaf-football" >}})
{{< image classes="fancybox nocaption clear fig-25" src="https://i.imgur.com/jBfEt6S.gif" thumbnail="https://i.imgur.com/jBfEt6S.gif" group="group:labs-nanoleaf-football" >}}

[//]: # ([Solarflare Studio]&#40;https://solarflarestudio.co.uk/&#41;)

[//]: # ([![SFS]&#40;https://i.imgur.com/apl8UH2t.png&#41;]&#40;https://solarflarestudio.co.uk/&#41;)

[//]: # ()
[//]: # ({{< image classes="fancybox left fig-20" src="https://i.imgur.com/jBfEt6S.gif" thumbnail="https://i.imgur.com/jBfEt6S.gif" group="group:labs-nanoleaf-football" >}})


## Repo/Download Source:

GitHub: https://github.com/Orakeshi/ApocalypseZ


Download: https://github.com/Orakeshi/ApocalypseZ/archive/refs/heads/main.zip


