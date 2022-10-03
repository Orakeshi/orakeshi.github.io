---
title: "Honeywell Battlefield"
date: "06 May 2022"
showDate: true
comments: false
showSocial: true
showMeta: true
categories:
  - projects
tags:
  - c-sharp
  - unity
  - multitaction
  - solarflare-studio
autoThumbnailImage: false 
thumbnailImagePosition: "top"
thumbnailImage: https://i.imgur.com/hj0wjWd.jpg
coverImage: https://i.imgur.com/NCDPyGf.jpg
metaAlignment: center 
syntaxHighlighter: highlight.js
---
<!--more-->
{{< toc >}}

# Project Information
This project, titled {{< hl-text red >}} "The Digital Wishing Tree"{{< /hl-text >}} was the first project I worked on in my software development career. This saw the in house team at solarflare create an interactive digital wishing tree using a combination of technologies.

Solarflare Studio (Company I work for) have a labs of sorts. This is where the internal team will get together and discuss ideas of projects they can create with in house resources. These projects are often fun, crazy and show off the technology available to solarflare.

In this project I completed the majority of development (code). I worked closely with the integrated designer, lead creative and CTO. The outcome of this project was that I had gained masses of knowledge in both Unity3D and coding with C#. This project was extremely beneficial to my learning as a software developer.

Projection Mapping, Nanoleaves & more were used to achieve the final result.
{{< image classes="fancybox center clear" src="https://i.imgur.com/UNjM0s4.gif" thumbnail="https://i.imgur.com/UNjM0s4.gif" group="group:wishingtree" thumbnail-width="50%" thumbnail-height="50%" title="GIF showing people using the tree" >}}

Above is a brief showcase of the complete wishing tree. This is the finished product that we delivered at the end of the SDLC.

=> The project works via the following:
- Open webpage & Submit wish with a swipe action
- Watch the real life wishing tree installation
  - Nanoleavs will light when wish lands on tree
  - Existing wish will flow onto a static leaf and be written
  - Lights will start reacting and lighting on the tree & nanoleaves
# Design:
The wishing tree went through many design iterations. As I was the main developer on the project, I was not heavily involved in regard to the design process. That being said I still had an interest as I was to develop the tree based on the designs provided.

{{< image classes="fancybox center clear" src="https://i.imgur.com/XwPuqij.png" thumbnail="https://i.imgur.com/XwPuqij.png" group="group:wishingtree" thumbnail-width="70%" thumbnail-height="70%" title="Image showing multitude of designs the tree went through" >}}

As seen from the image above the tree went through many iterations. Working closely with the integrated designer and creative lead we kept experimenting with the output of the tree. In the end we agreed on the design below.

{{< image classes="fancybox center clear" src="https://i.imgur.com/JL019sM.png" thumbnail="https://i.imgur.com/JL019sM.png" group="group:wishingtree" thumbnail-width="70%" thumbnail-height="70%" title="Image showing the design of the wishing tree" >}}

Above, is the final design we decided upon. We planned to integrate more colours, but the tree shape and size was the conclusion.

# Development:
Development of this project saw a multitude of languages, tools, tech etc used. I had to learn how to {{< hl-text blue >}} solder{{< /hl-text >}} to connect the nanoleaves together using custom-made extensions cables. In addition to this, I had to pickup {{< hl-text blue >}} web technologies{{< /hl-text >}} such as basic HTML, CSS, JS and PHP. These were needed in order to set up a local MySQL database to store the wishes.

## Webpage & PHP
{{< codeblock "connect.php" "php" "" "connect.php" >}}
$dbhost = 'localhost'; //Location of database
$dbuser = 'root'; 
$dbpassword = ''; //Password for database
$dbname = 'wishingtree'; //Database Name
$wish = $_POST['wish'];

    //Database connection
    $connection = new mysqli($dbhost, $dbuser, $dbpassword, $dbname); //Start a new connection to databse
    if($connection->connect_error){ //If connection error present
        die('Connection Failed : '.$connection->connect_error); //Stop the PHP and output error
    }else{ 
        $stmt = $connection->prepare("insert into tblwish(wish)values(?)"); //Insert new wish into table
        $stmt->bind_param("s" ,$wish); 
        $stmt->execute();
        echo "Wish Successfull"; //Output success message
        $stmt->close(); //Close the request and connection
        $connection->close();
    }
{{< /codeblock >}}

{{< codeblock "convert.php" "php" "" "convert.php" >}}
$dbhost = 'localhost';
$dbuser = 'root';
$dbpass = '';
$dbname = 'wishingtree';

    $dblink = new mysqli($dbhost, $dbuser, $dbpass, $dbname);

    if ($dblink->connect_errno) {
        printf("Failed to connect to database");
        exit();
    }
    $result = $dblink->query("SELECT * FROM tblwish ORDER BY id DESC LIMIT 1");
    //$result = $dblink->query("SELECT wish FROM tblwish"); //Query for selecting wishes from table

    //Initialize array variable
    $dbdata = array();

    //Fetch into associative array
    while ( $row = $result->fetch_assoc())  {
    $dbdata[]=$row;
    }

    //Print array in JSON format
    echo json_encode($dbdata);
{{< /codeblock >}}

Above are the two {{< hl-text red >}} PHP{{< /hl-text >}} scripts I used in the application. These were used to retrieve the wishes from the database and also write the wish to the database.

{{< codeblock "index.html" "html" "" "index.html" >}}
<article>
    <aside></aside>
    <!-- <h1 id="wishtitle">Enter Wish below:</h1> -->

    <form id="wishform" action="connect.php" method="POST">
        <textarea placeholder="Type your message here..." id="wishinput" name="wish" maxlength="30" type="text" oninput="this.value = this.value.replace(/[^a-z A-Z]/g, '');"></textarea>
    </form>
            
</article>
{{< /codeblock >}}

The use case for the PHP is seen above. I set up a form in HTML in order to post the wish to the database. This was then retrieved within the unity scene.

The complete webpage looks like the following:

{{< image classes="fancybox center clear" src="https://i.imgur.com/i7TiXd6.png" thumbnail="https://i.imgur.com/i7TiXd6.png" group="group:wishingtree" thumbnail-width="50%" thumbnail-height="50%" title="Image of the wishing tree webpage" >}}

## Unity Application
The unity application took a while to develop. This was a large project from initiation to deployment. Having to develop alot of different aspects of the project took alot of work.

The application was split into many modules some main ones being:
- Nanoleaves
- Wishes
- UI
- Network
- Animations

& alot more ^

### Wish Pathing
For the wish pathing I decided to use {{< hl-text blue >}} Bézier curves{{< /hl-text >}}. After watching tutorials on how to set these up in unity, I was able to integrate them into the scene with ease. Once a wish was received in the scene, I wanted it to spawn at various pre-defined locations. From these locations I wanted the wish to follow a wind blowing animation along this path.

To do this I started by creating the various wish paths to each leaf I wanted the wish to land on.

{{< image classes="fancybox center clear" src="https://i.imgur.com/f41GKyW.png" thumbnail="https://i.imgur.com/f41GKyW.png" group="group:wishingtree" thumbnail-width="70%" thumbnail-height="70%" title="Image showing bezier curve paths" >}}

### Animations
Animations was a very simple part of development. We wanted a way to have pulse like animations when the wish lands. To achieve this we simply used a sprite that scaled and fades over a given time.

We instantiate the pulse at the leaf that the wish lands on and play the pulse animation. We defined these early on in development as seen from the gif below.

{{< image classes="fancybox center clear" src="https://i.imgur.com/h4PgAkQ.gif" thumbnail="https://i.imgur.com/h4PgAkQ.gif" group="group:wishingtree" thumbnail-width="70%" thumbnail-height="70%" title="GIF showing the pulse animation for the tree" >}}

### Final Thoughts On Development:
With development complete I reflected on everything that had been achieved. The wishing tree took a lot of work from the team. From the application development alone there were masses of components that complete the functionality we were aiming for. That being said I enjoyed the development a lot, and it was a great first project to be involved in.

# Challenges & Solutions:
There were many challenges that I faced during the development of the wishing tree. Below are some of the most challenging problems I encountered along with the solution/approach I took to fix them.

## Finding Nanoleaf Controller
Perhaps the most difficult challenge was recognising the nanoleaves on the network. As we wanted this experience to be portable I wanted to make sure that no matter what network we are connected to, we could both recognise the leaves & link them to the unity scene with their authentication token.

Nanoleaves connect to the network via a controller. We discovered that every nanoleaf controller has a mac address of the following format:

    00:55:da:xx:xx:xx

This was perfect because it meant that every mac address contains the string {{< hl-text red >}} "00:55:da"{{< /hl-text >}}. With this we could scan the network that the device is connected to and look for any mac addresses that contain the string.

{{< codeblock "CmdHandler.cs" "csharp" "" "CmdHandler.cs" >}}
/// <summary>
/// Method handles opening the command promt and sending the command + Getting the output
/// </summary>
/// <param name="input"></param>
private void Command(string input)
{
  var processInfo = new ProcessStartInfo("cmd.exe", input)
  {
    // Lines hide the CMD menu so the user does not see this happening
    CreateNoWindow = true,
    UseShellExecute = false,
    RedirectStandardInput = true,
    RedirectStandardOutput = true,
    RedirectStandardError = true
  };
  
  var process = Process.Start(processInfo);
  output = process.StandardOutput.ReadToEnd();

  process.WaitForExit();


  finished = true;

  return;
}

/// <summary>
/// This method is responsbile for getting the users IPV4 address
/// </summary>
private void GetMachineInfo()
{
    string[] cmdOutput = output.Split('\n');
    int x = 1;
    for (int i = 0; i < cmdOutput.Length; i++)
    {
        if (cmdOutput[i].Contains("IPv4 Address"))
        {
            string[] tempArray = cmdOutput[i].Split(':');
            machineIpAddress = tempArray[x].Replace(" ", "");
        }

    }
    output = "";
    return;
}

/// <summary>
/// Method handles splittling the lines up and iterating through
/// </summary>
private void GetControllerInfo()
{
    int x = 0;
    string[] cmdOutput = output.Split('\n');

    // This runs through the output string and checks each line for nanoleafs
    for (int i = 0; i < cmdOutput.Length; i++)
    {

        if (cmdOutput[i].Contains("00-55-da"))
        {
            // If a nanoleaf controller is found it formats it and stores the data in a list
            cmdOutputLine.Add(cmdOutput[i]);
            cmdOutputLine[x].Trim();
            string temp = cmdOutputLine[x] = Regex.Replace(cmdOutputLine[x], " {4,}", ",");

            formattedOutput.Add(temp);
            x++;
        }

    }
    output = "";
    GetIP();
}
{{< /codeblock >}}

With the code above we could recognise and control any nanoleaf controller on any network that the device was connected to.

## Customizing Leaf Colours
Another particularly challenging aspect of the application was having a way for us to light the nanoleaves to any colours we wanted. As we wanted to create bespoke and diverse tree animations we wanted the nanoleaves to match in colour. The issue is that for the nanoleaves the only way to change there colours is to predefine the colours in a list and send them to the nanoleaf API.

This was a conflict of interest with how we wanted the application to perform. In no way did we want to manually write multiple animations for the leaves. My solution was an algorithm I created called "Nanoleaf Scraper"

### Nanoleaf Pixel Scraper
My idea was to have the nanoleaves scrape pixels from custom videos we input. This way we could create these beautiful bespoke gradient videos. We could input these into the app. This means we could overlay this gradient over the tree via projection and have the nanoleaves light up with the same gradient. In theory, they should both match and look amazing.

For the nanoleaves to have this possible I needed to get the RGB value of the pixel at the given nanoleaf location in real life. To achieve this, I created the pixel scraping animation.

{{< image classes="fancybox center clear" src="https://i.imgur.com/sAk9BwU.gif"
thumbnail="https://i.imgur.com/sAk9BwU.gif" group="group:wishingtree" thumbnail-width="70%" thumbnail-height="70%" title="GIF showing the wishing tree gradient video" >}}

Above shows the typical gradient video that would be passed to the application. given a video such as above, I wanted to scrape the nanoleaf colours as the chosen location in the video above. 

{{< codeblock "GetPixel.cs" "csharp" "" "GetPixel.cs" >}}
/// <summary>
/// Method handles getting the video clip to be split up
/// </summary>
public void NewVideo()
{
  //Access the videoplayer clip and play the video clip with the random number
  videoPlayer.clip = vptest.vids[file.currentVidNum];

  videoPlayer.Stop();

  videoPlayer.renderMode = VideoRenderMode.APIOnly;
  videoPlayer.EnableAudioTrack(0, false);
  videoPlayer.prepareCompleted += Prepared;
  videoPlayer.sendFrameReadyEvents = true;

  videoPlayer.frameReady += FrameReady;
  videoPlayer.Prepare();

  //Assign x and y values of leafs to these vars
  x = screenPosNew.x;
  y = screenPosNew.y;
}

/// <summary>
/// Plays the video provided as a parameter
/// </summary>
/// <param name="vp"></param>
private void Prepared(UnityEngine.Video.VideoPlayer vp)
{
    vp.Play(); //Play video when ready
}

/// <summary>
/// This is the method that splits up the video into frames and prepares to be stored in file
/// </summary>
/// <param name="vp"></param>
/// <param name="frameIndex"></param>
private void FrameReady(UnityEngine.Video.VideoPlayer vp, long frameIndex)
{
    var textureToCopy = vp.texture;
    vp.frame = frameIndex + 30;

    Texture mainTexture = textureToCopy;
    Texture2D texture2D = new Texture2D(mainTexture.width, mainTexture.height, TextureFormat.RGBA32, false);

    RenderTexture currentRT = RenderTexture.active;
    RenderTexture renderTexture = new RenderTexture(mainTexture.width, mainTexture.height, 32);

    Graphics.Blit(mainTexture, renderTexture);

    RenderTexture.active = renderTexture;

    texture2D.ReadPixels(new Rect(0, 0, renderTexture.width, renderTexture.height), 0, 0);
    texture2D.Apply();

    pixels = texture2D.GetPixels(x, y, 1, 1);

    RenderTexture.active = currentRT;

    tempColourPixels.Add(pixels[0]);

    long frameCount = Convert.ToInt64(vp.frameCount);

    //If the current frame exceeds 10% of the max videos frame it means its at the end and should copy 
    //the list into another list to access outside of script
    if (vp.frame >= frameCount - frameCount * 0.01)
    {
        finalColourPixels = tempColourPixels;
        file.frames = true;
        return;
    }
}

/// <summary>
/// Resets the lists used to store the pixels
/// </summary>
public void ResetPixels()
{
    finalColourPixels.Clear();
    tempColourPixels.Clear();
}
{{< /codeblock >}}

The above script is responsible for doing exactly what I want. The videos are passed to the script. From there the leaves locations are fetched in pixels. I am then converting each frame of the video to a texture, grabbing the pixel and string that in a file. Once all videos are complete I can point the leaves to read all the pixels in the files that are output and sync them with the tree gradient.

This meant I had now completed scraping the pixel data and displaying the leaves as the correct colours.

### Output Of Leaf Colours:

{{< image classes="fancybox fig-50 center nocaption" src="https://i.imgur.com/O41TSFd.png?1" thumbnail="https://i.imgur.com/O41TSFd.png?1" group="group:wishingtree" >}}
{{< image classes="fancybox fig-50 center clear nocaption" src="https://i.imgur.com/pxG01zQ.jpg" thumbnail="https://i.imgur.com/pxG01zQ.jpg" group="group:wishingtree" >}}

As seen from the images above, the left gradient video roughly matches the nanoleaves in the actual installation.
# Showcase:

{{< youtube HtmS5DKOyn0 >}}
{{< image classes="fancybox nocaption fig-50" src="https://i.imgur.com/gVO0gvr.jpg" thumbnail="https://i.imgur.com/gVO0gvr.jpg" group="group:wishingtree" >}}
{{< image classes="fancybox nocaption fig-50" src="https://i.imgur.com/sBEOTVD.jpg" thumbnail="https://i.imgur.com/sBEOTVD.jpg" group="group:wishingtree" >}}
{{< image classes="fancybox nocaption fig-33" src="https://i.imgur.com/5CaofPL.jpg" thumbnail="https://i.imgur.com/5CaofPL.jpg" group="group:wishingtree" >}}
{{< image classes="fancybox nocaption fig-33" src="https://i.imgur.com/ccQSEB4.jpg" thumbnail="https://i.imgur.com/ccQSEB4.jpg" group="group:wishingtree" >}}
{{< image classes="fancybox nocaption fig-33" src="https://i.imgur.com/PKx21cZ.jpg" thumbnail="https://i.imgur.com/PKx21cZ.jpg" group="group:wishingtree" >}}

# Extra Information
{{< image classes="fancybox clear nocaption fig-25" src="https://i.imgur.com/apl8UH2t.png" thumbnail="https://i.imgur.com/apl8UH2t.png" group="group:wishingtree" >}}
[Solarflare Studio](https://solarflarestudio.co.uk/)

LINK TO PROJECT WRITEUP:
https://drive.google.com/file/d/1QUc5PsR7m_RMbc1YZI-grEKgD3dW_MFX/view?usp=sharing

[//]: # ([![SFS]&#40;https://i.imgur.com/apl8UH2t.png&#41;]&#40;https://solarflarestudio.co.uk/&#41;)

[//]: # ()
[//]: # ({{< image classes="fancybox left fig-20" src="https://i.imgur.com/jBfEt6S.gif" thumbnail="https://i.imgur.com/jBfEt6S.gif" group="group:wishingtree" >}})


[//]: # (## Repo/Download Source:)

[//]: # (GitHub: https://github.com/Orakeshi/Nanoleaf-PremierLeague)

[//]: # ()
[//]: # (Download: https://github.com/Orakeshi/Nanoleaf-PremierLeague/archive/refs/heads/main.zip)


