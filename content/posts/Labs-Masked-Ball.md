---
title: "AR Masked Ball"
date: "13 Aug 2021"
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
thumbnailImage: https://i.imgur.com/umCN9mj.jpg
coverImage: https://i.imgur.com/Vvag1wu.png
metaAlignment: center
syntaxHighlighter: highlight.js
---
<!--more-->
{{< toc >}}

# Project Information
The masked ball project was an in-house labs project in accordance with artist [Charlotte Dillon](https://www.instagram.com/headofhouse_designs/). Myself and Priya (Intern from Goldsmiths university developed the AR application).

The project/application was to create an AR application for the magic leap to show off [Charlotte's](https://www.instagram.com/headofhouse_designs/) masks that she had designed. We were creating a proof of concept for a multi networked masked ball event. The POC application was to showcase two magic leaps syncing content being able to see each other over a network.

We wanted to be able to have a user select a mask from a number of options and have that mask track to their head. The other user wearing a magic leap device would then be able to look at the other person in the space with a mask over their head. In description this may sound simple, in practice there were many challenges, and it was a fun & difficult project.

{{< image classes="fancybox center clear" src="https://i.imgur.com/0uLtQsf.gif" thumbnail="https://i.imgur.com/0uLtQsf.gif" group="group:labs-maskedball" thumbnail-width="70%" thumbnail-height="70%" title="GIF showing the application working" >}}

# Development:
The development for this project was contained to unity & magic leap dev tools. Using C# in accordance to unity we were able to build the app for magic leap.

## Tiltbrush Workflow
[Charlotte](https://www.instagram.com/headofhouse_designs/) was using an oculus quest with tiltbrush in order to create the masks for unity. With this, we need a way to get [Charlotte's](https://www.instagram.com/headofhouse_designs/) masks into unity.
{{< image classes="fancybox center clear" src="https://i.imgur.com/eNghbTV.gif" thumbnail="https://i.imgur.com/eNghbTV.gif" group="group:labs-maskedball" thumbnail-width="70%" thumbnail-height="70%" title="GIF showing Charlotte creating a mask in tiltbrush" >}}

To achieve this, Priya found a unity plugin on GitHub that could convert tilt files from tiltbrush into fully textured models within unity. Plugin [HERE](https://github.com/googlevr/tilt-brush-toolkit)

The image below shows the mask in unity with the plugin enabled, working as intended (With all colours, textures etc)
{{< image classes="fancybox center clear" src="https://i.imgur.com/Eg3ZFC2.png" thumbnail="https://i.imgur.com/Eg3ZFC2.png" group="group:labs-maskedball" thumbnail-width="70%" thumbnail-height="70%" title="Image showing the mask in the unity scene" >}}

## Translating Masks Over Network
We needed a way to have the masks translate and follow the magic leap device over the network. It wasn't as simple as updating the masks transform on each frame. We needed to send its transform to the other magic leap device.
{{< codeblock "MasksSpawning.cs" "csharp" "" "MasksSpawning.cs" >}}
namespace Solarflare.MaskedBall.Masks
{
    public class MasksSpawning : MonoBehaviour, ISelectable
    {
        public List<GameObject> maskTransforms = new List<GameObject>();

        private List<TransmissionObject> _maskTransmissionGroup = new List<TransmissionObject>();
        private TransmissionObject _maskTransmissionObject;

        private Transform _mainCamera;

        private void Start()
        {
            _mainCamera = GameObject.Find("Main Camera").GetComponent<Transform>(); //Getting main camera component
            int maskName = Random.Range(0, maskTransforms.Count);
            InstantiateItem(maskTransforms[maskName].name);
        }

        public void InstantiateItem(string maskName)
        {
            if (_maskTransmissionGroup.Count != 0)
            {
                GameObject[] masks = GameObject.FindGameObjectsWithTag("Mask");
                foreach(GameObject mask in masks)
                {
                    TransmissionObject maskTrans = mask.GetComponent<TransmissionObject>();
                    maskTrans.Despawn();
                }
                _maskTransmissionGroup.Clear();
            }

            Vector3 tempScale = new Vector3(1, 1, 1); //Temporary scale of Cube
            for (int i = 0; i < maskTransforms.Count; i++)
            {
                if (maskTransforms[i].name == maskName)
                {
                    tempScale = maskTransforms[i].transform.localScale;
                }

            }
            _maskTransmissionObject = Transmission.Spawn(maskName, _mainCamera.transform.position, this.transform.rotation, tempScale); //Instantiate Mask to be shown across LAN
            _maskTransmissionObject.name = maskName;
            _maskTransmissionGroup.Add(_maskTransmissionObject);
            SetMaskPosition();
        }

        private void SetMaskPosition()
        {
            _maskTransmissionObject.motionSource = _mainCamera.transform; //Put Cube on ML main camera and check if synced over LAN
            _maskTransmissionObject.transform.position = _mainCamera.position; // Set transmission Object to same transform as mainCamera on every frame
        }
    }
}
{{< /codeblock >}}

The above code handles everything we need. It instantiates the mask when the user clicks the mask UI button. This will then instantiate a GameObject with the TransmissionObject script from magic leap. When the script is attached to a gameobject, that object will update the network with its transform. The other magic leap can then know where it is via PCFs (Persistent coordinate frames)

# Challenges & Solutions:
There were 2 main challenges when working on this project. They were the following:
- Working with MagicLeap device
- Syncing PCFs

The first challenge ("Working with MagicLeap device") couldn't be solved so easily. The documentation is lacking and the device has so many issues. All I could do was accept this was the case and just move on with development.

As for the second issue this effected the quality of the application and had to be resolved.
## Syncing PCFs
The issue was syncing PCFs (Persistent coordinate frames). Magic leaps can show objects over the network due to PCFs. The device is constantly scanning its environment. It will then store this mapped data to know where objects are relative to one another. The issue I was having is that the magic leap devices couldn't see each other's masks. This was because the PCFs were not syncing properly.

### Object Marker Tracking
I attempted to resolve this issue via having the headsets start syncing from a shared point. This was the Object Marker Tracking approach. Have both headsets start there PCFs from a shared marker. MagicLeap had a code sample to implement this into code which looks like the following:
{{< codeblock "ImageTrackingSystem.cs" "csharp" "" "ImageTrackingSystem.cs" >}}
public void StartImageTracking()
    {
        // Only start Image Tracking if privilege wasn't denied
        if (CurrentStatus != ImageTrackingStatus.PrivilegeDenied)
        {
            // Is not already started, and failed to start correctly, this is likely due to the camera already being in use:
            if (!MLImageTracker.IsStarted && !MLImageTracker.Start().IsOk)
            {
                Debug.LogError("Image Tracker Could Not Start");
                UpdateImageTrackingStatus(ImageTrackingStatus.CameraUnavailable);
                return;
            }

            // MLImageTracker would have been started by previous If statement at this point, so enable it. 
            if (MLImageTracker.Enable().IsOk)
            {
                // Add the target image to the tracker and set the callback
                _imageTarget = MLImageTracker.AddTarget(TargetInfo.Name, TargetInfo.Image,
                TargetInfo.LongerDimension, HandleImageTracked);
                UpdateImageTrackingStatus(ImageTrackingStatus.ImageTrackingActive);
            }
            else
            {
                Debug.LogError("Image Tracker Could Not Start");
                UpdateImageTrackingStatus(ImageTrackingStatus.CameraUnavailable);
                return;
            }
        }
    }
{{< /codeblock >}}
This image tracking test didn't work. There were a mass amount of errors that we didn't have time to look into. We decided to scrap this approach.

### Cloud Anchor Fix
After a mass amount of time researching I finally found a solution. It is a flaky fix that doesn't always work, but it works enough for us to capture media for our proof of concept.

This is not a code solution, but much rather a device fix. I realised that the second magic leap device didn't have its PCFs syncing over the cloud. Instead, it was kept on local. Enabling this setting & then having the user stand still whilst opening the app meant they shared the same PCF data and could visibly see each others masks.
# Showcase:
{{< youtube U6uxoHR1dWE >}}

Application:
{{< image classes="fancybox right fig-75" src="https://i.imgur.com/aMzbljy.gif" thumbnail="https://i.imgur.com/aMzbljy.gif" group="group:labs-maskedball" >}}
{{< image classes="fancybox right fig-25" src="https://i.imgur.com/Oqnfduw.png?2" thumbnail="https://i.imgur.com/Oqnfduw.png?2" group="group:labs-maskedball" >}}
{{< image classes="fancybox right fig-25" src="https://i.imgur.com/lgRl37N.png" thumbnail="https://i.imgur.com/lgRl37N.png" group="group:labs-maskedball" >}}
{{< image classes="fancybox right fig-25 clear" src="https://i.imgur.com/TzG0jjZ.png?1" thumbnail="https://i.imgur.com/TzG0jjZ.png?1" group="group:labs-maskedball" >}}

# Extra Information
{{< image classes="fancybox nocaption clear fig-25" src="https://i.imgur.com/apl8UH2t.png" thumbnail="https://i.imgur.com/apl8UH2t.png" group="group:labs-maskedball" >}}
[Solarflare Studio](https://solarflarestudio.co.uk/)

[//]: # ({{< image classes="fancybox nocaption clear fig-25" src="https://i.imgur.com/jBfEt6S.gif" thumbnail="https://i.imgur.com/jBfEt6S.gif" group="group:labs-maskedball" >}})


[//]: # ([![SFS]&#40;https://i.imgur.com/apl8UH2t.png&#41;]&#40;https://solarflarestudio.co.uk/&#41;)

[//]: # ()
[//]: # ({{< image classes="fancybox left fig-20" src="https://i.imgur.com/jBfEt6S.gif" thumbnail="https://i.imgur.com/jBfEt6S.gif" group="group:labs-nanoleaf-football" >}})


[//]: # (## Repo/Download Source:)

[//]: # (GitHub: https://github.com/Orakeshi/Nanoleaf-PremierLeague)

[//]: # ()
[//]: # (Download: https://github.com/Orakeshi/Nanoleaf-PremierLeague/archive/refs/heads/main.zip)


