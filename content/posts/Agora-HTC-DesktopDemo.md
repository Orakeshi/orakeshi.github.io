---
title: "Agora - HTC Demo"
date: "11 Nov 2021"
showDate: true
comments: false
showSocial: true
showMeta: true
categories:
  - projects
tags:
  - c-sharp
  - unity
  - agora
  - vr
  - solarflare-studio
autoThumbnailImage: false
thumbnailImagePosition: "top"
thumbnailImage: https://i.imgur.com/fkwOrtf.jpg
coverImage: https://i.imgur.com/mQlUSgi.png
metaAlignment: center
---
<!--more-->
{{< toc >}}

# Project Information
this project took place straight after the competition of the lego project, with an equally tight deadline. The output was the following. Two application. One was a VR application in which users could open the app, walk around the scene and see people on the second application (Desktop app).

If a user has there webcam enabled on the desktop app, the VR user should be able to see them in the scene. VR users can also view desktop users screenshare. Finally, VR users can spawn and grab VR objects into the unity scene.

Essentially, this project was to showcase the agora realtime messaging system. Using their SDK we created a way for desktop and VR users to be in the same environment and communicate with each other

This project, I was not heavily involved in. I only participated some code for the desktop application. Most/all of the code was carried out by Solarflares lead developer, who managed an extraordinary amount of work in such a small deadline.

{{< image classes="fancybox center clear" src="https://i.imgur.com/p6TCKSS.jpg" thumbnail="https://i.imgur.com/p6TCKSS.jpg" group="group:agorahtcdemo" thumbnail-width="70%" thumbnail-height="70%" title="Image showing complete agora demo" >}}

# Development:
As mentioned, Solarflares lead developer carried out the majority of development. As a result I only participated in a fraction of the overall work. Due to this, I will only write briefly about the development process with what I worked on.

{{< codeblock "MicButton.cs" "csharp" "" "MicButton.cs" >}}
using Solarflare.HTCDemo.Framework.App;
using Solarflare.HTCDemo.Framework.Rtc;
using UnityEngine;
using UnityEngine.UI;

namespace Solarflare.HTCDemo.DesktopApp.UI
{
    public class MicButton : MonoBehaviour
    {
        private IRtcSystem rtcSystem;

        private void Awake()
        {
            rtcSystem = HTCDemoFrameworkApp.Instance.RtcSystem;
            
            GetComponent<Button>().onClick.AddListener(OnClicked);
        }

        private void OnClicked()
        {
            rtcSystem.IsMuted = !rtcSystem.IsMuted;
        }
    }
}
{{< /codeblock >}}
The above code is the typical code I was writing. In unity I implemented the UI for the desktop application. I then added scripts onto each button to control both the button state and the functionality when the button was pressed.

Due to Basar (Lead developer) writing very clean and high quality code, I simply used the interface he created (RtcSystem) and updated the values from the interface. This in hand affected both applications.

{{< codeblock "CameraStateView.cs" "csharp" "" "CameraStateView.cs" >}}
using Solarflare.HTCDemo.Framework.App;
using Solarflare.HTCDemo.Framework.Rtc;
using UnityEngine;
using UnityEngine.UI;

namespace Solarflare.HTCDemo.DesktopApp.UI
{
    [RequireComponent(typeof(Image))]
    public class CameraStateView : MonoBehaviour
    {
        [SerializeField] private Sprite camOnSprite;
        [SerializeField] private Sprite camOffSprite;

        private Image image;
        private IRtcSystem rtcSystem;

        private void Awake()
        {
            image = GetComponent<Image>();
            rtcSystem = HTCDemoFrameworkApp.Instance.RtcSystem;
        }

        private void Update()
        {
            image.sprite = rtcSystem.LocalVideoEnabled ? camOnSprite : camOffSprite;
        }
    }
}
{{< /codeblock >}}

Above is a typical stateview script I wrote to change the UI view of the buttons when they were selected/unselected.

{{< codeblock "StreamVirtualCamera" "csharp" "" "StreamVirtualCamera" >}}
using System;
using Solarflare.HTCDemo.Framework.App;
using Solarflare.HTCDemo.Framework.Rtc;
using Solarflare.HTCDemo.Framework.Sessions;
using UnityEngine;
using UnityEngine.UI;

namespace Solarflare.HTCDemo.DesktopApp.Rtc
{
    public class StreamVirtualCamera : MonoBehaviour
    {
        [SerializeField]
        private RawImage remoteVideo;
        [SerializeField] private bool streamVirtualCameraView;
        [SerializeField] private Camera renderTextureCamera;

        private IRtcSystem rtcSystem;
        private ISession session;
        private bool streamingVirtualCamera;

        private void Awake()
        {
            rtcSystem = HTCDemoFrameworkApp.Instance.RtcSystem;
            session = HTCDemoFrameworkApp.Instance.Session;
        }

        private async void Start()
        {
            await rtcSystem.JoinChannel($"{session.SessionId}-Rtc", VideoMode.VirtualCamera);

            rtcSystem.UserJoined += OnUserJoined;
        }

        private void OnUserJoined(string userId)
        {
            if (string.IsNullOrEmpty(userId)) return;
            rtcSystem.AttachVideoToRawImage(remoteVideo, userId);
        }

        private void Update()
        {
            if (streamVirtualCameraView)
            {
                if (streamingVirtualCamera == false)
                {
                    rtcSystem.StartVirtualCameraViewStream(renderTextureCamera);
                    streamingVirtualCamera = true;
                }
            }
            else
            {
                if (streamingVirtualCamera)
                {
                    rtcSystem.StopVirtualCameraViewStream();
                    streamingVirtualCamera = false;
                }
            }
        }
    }
}
{{< /codeblock >}}
I also contributed some code toward streaming the virtual camera as seen from the code above.

# Showcase:
{{< youtube XolYzO_XdWk >}}
{{< image classes="fancybox fig-33" src="https://i.imgur.com/13aMPFD.jpg" thumbnail="https://i.imgur.com/13aMPFD.jpg" group="group:agorahtcdemo" >}}
{{< image classes="fancybox fig-33" src="https://i.imgur.com/0nJgk9w.jpg" thumbnail="https://i.imgur.com/0nJgk9w.jpg" group="group:agorahtcdemo" >}}
{{< image classes="fancybox clear fig-33" src="https://i.imgur.com/BzNftA4.jpg" thumbnail="https://i.imgur.com/BzNftA4.jpg" group="group:agorahtcdemo" >}}


# Extra Information

[//]: # ({{< image classes="fancybox nocaption fig-25" src="https://i.imgur.com/apl8UH2t.png" thumbnail="https://i.imgur.com/apl8UH2t.png" group="group:labs-nanoleaf-football" >}})

[comment]: <> ({{< image classes="fancybox nocaption clear fig-25" src="https://i.imgur.com/jBfEt6S.gif" thumbnail="https://i.imgur.com/jBfEt6S.gif" group="group:labs-nanoleaf-football" >}})

[//]: # ([Solarflare Studio]&#40;https://solarflarestudio.co.uk/&#41;)

[//]: # ([![SFS]&#40;https://i.imgur.com/apl8UH2t.png&#41;]&#40;https://solarflarestudio.co.uk/&#41;)

[//]: # ()
[//]: # ({{< image classes="fancybox left fig-20" src="https://i.imgur.com/jBfEt6S.gif" thumbnail="https://i.imgur.com/jBfEt6S.gif" group="group:labs-nanoleaf-football" >}})


