---
title: "Nanoleaf - Goal Reactive Lighting"
date: "11 Jul 2021"
showDate: true
comments: false
showSocial: true
showMeta: true
categories:
  - projects
tags:
  - python
  - labs 
  - solarflare-studio
autoThumbnailImage: false 
thumbnailImagePosition: "top"
thumbnailImage: https://i.imgur.com/veFiH5G.jpg
coverImage: https://i.imgur.com/g2oZgdE.png 
metaAlignment: center
---
<!--more-->
{{< toc >}}

# The plan:
The plan was simple. I had nanoleaves lying around from a previous project ([Wishing Tree](https://orakeshi.github.io/post/digital-wishing-tree/)) and wanted to do something with them. Before starting my software development apprenticeship, I used to enjoy watching football. I would put on several games regardless of the day they were played on. Now, due to myself working during the week and creating personal projects on the weekend, my time is limited, and thus I rarely ever get to watch the games.

I thought, instead of taking the simple approach of keeping open a live tracker/listening to the radio, I would take a creative approach of lighting up the nanoleaves based on the team that scores. I wanted to produce an output quick. I didnt care about how the code war written nor how long it took for the leaves to light up. My goal was to have my nanoleaves light up when a team scores no matter what.

{{< image classes="fancybox center clear" src="https://i.imgur.com/g2oZgdE.png" thumbnail="https://i.imgur.com/g2oZgdE.png" group="group:labs-nanoleaf-football" thumbnail-width="70%" thumbnail-height="70%" title="Image of the spare nanoleaves from the wishing tree project" >}}

# Design Process:
Immediately upon undertaking this project, I knew I needed the football team colours. I was fortunate enough that a website ([teamcolorcodes](https://teamcolorcodes.com/)) already had all the teams in the premier league with their team colours RGB. This saved me a-lot of time. At this point I had to have a way of storing all the colours for the application to use. I went with a json file which contained the team name, along with the three colours that represent the team

{{< image classes="fancybox center clear" src="https://i.imgur.com/ibaTgbj.png" thumbnail="https://i.imgur.com/ibaTgbj.png" group="group:labs-nanoleaf-football" thumbnail-width="70%" thumbnail-height="70%" title="Image showing the teams and there 3 main RGB colours of their badge/kit" >}}

After creating the JSON file with the team colours, I decided to draw a simple design sketch on figma showing the 3 frames/stages of the colours that the nanoleaves will look like upon a team scoring. (These colours will change dependent on team colours)

{{< image classes="fancybox center clear" src="https://i.imgur.com/JnzFtRg.png" thumbnail="https://i.imgur.com/JnzFtRg.png" group="group:labs-nanoleaf-football" thumbnail-width="70%" thumbnail-height="70%" title="Image showing the 3 frames of the lights changing colour when a team scores" >}}

## The Problem With The Nanoleaf API:
The way that nanoleaves work is via sending requests to the API. The nanoleaf documentation is severely lacking and quite underwhelming in what the leaves are able to do. That being said, the main way to light up nanoleaves from code is to send a {{< hl-text red >}} PUT{{< /hl-text >}} request to the leaves with RGB data. This request follows a format along the lines of the following:

Leaf 1 -> PUT Request = ('{"write": {"animdata": "{{< hl-text orange >}} 1{{< /hl-text >}} {{< hl-text blue >}} 1{{< /hl-text >}} {{< hl-text green >}} 237 233 57{{< /hl-text >}}"}')

The request above follows a certain format as stated in the nanoleaf documentation. That is, to create a write command and send the following attributes. The first "{{< hl-text orange >}} 1{{< /hl-text >}}" is the frame. I would want to send one colour so {{< hl-text orange >}} 1{{< /hl-text >}} frame would be needed. The second "{{< hl-text blue >}} 1{{< /hl-text >}}" is the ID of the leaf. This is used to target the specific leaf to send the request too. Finally, the last {{< hl-text green >}} 3 numbers{{< /hl-text >}} are the RGB value to send to the leaf. This works if you want to light up the leaves however its incredibly inefficient, due to the amount of requests that are needed to light up all the leaves in a sequence. In addition, it's prone to many issues, including overloading pythons request library.

# Development:
Starting development, I had a clear idea in mind of what I was meant to do, as-well as the challenges that would arise. Due to this project being a quick side project, I {{< hl-text red >}} did not care{{< /hl-text >}} about following best practices and followed the approach of "As long as the code works".

{{< image classes="fancybox center clear" src="https://i.imgur.com/B1OEq3c.gif" thumbnail="https://i.imgur.com/B1OEq3c.gif" group="group:labs-nanoleaf-football" thumbnail-width="70%" thumbnail-height="70%" title="GIF showing a timelapse of me developing the application" >}}

## Packet Builder Algorithm
To combat the issue of having to send three requests to each leaf every second to change their colours, I came up with a solution, that I called the packet builder. The solution is to have 1 request that is sent to the leaves. Inside this request will be the total frames, each leaf ID and all the RGB values the leaves will light up with.

To the human reading the packet that is sent, it will seem like a large ball of mess containing mass amounts of integers, however to the API this request is massively more efficient than sending each request individually to each leaf 3 times per second.

There are 3 main methods/functions that encompass this packet builder algorithm. Those are getting the layout of the nanoleaves and all the panels that are on the network. The second is getting the ID of each leaf from the panels recognised on the network. The final is the packet builder itself.

### GetLayout()
{{< codeblock "main.py" "python" "" "main.py" >}}
# -*- coding: utf-8 -*-
"""
This method is responsible for accessing the layout of the nanoleaves on the network
"""
def GetLayout():
    global outputFormat
    # Send layout request to nanoleaves on network
    newResponse = requests.get("http://" + str(UniversalIP.rstrip()) + ":" + defaultPort + "/api/v1/" + str(UniversalAuthCode.rstrip()) + "/panelLayout/layout")
    output = str(newResponse.json())

    # Format output to be acceptable for packetBuilder
    outputFormat = output[output.index('['):]
    outputFormat = outputFormat.split('}')
    PacketBuilder()
    return
{{< /codeblock >}}

The {{< hl-text orange >}} GetLayout(){{< /hl-text >}} method is very simple in nature. A {{< hl-text red >}} GET{{< /hl-text >}} request is sent to the Nanoleaf API. The JSON string that is returned is passed into a variable called {{< hl-text blue >}} outputFormat{{< /hl-text >}}. This string will be used later on when getting the ID of each leaf.

### GetId()
The {{< hl-text orange >}} GetId(){{< /hl-text >}} method is the next part of the packet builder process. This method simply iterates through the layout JSON string, which contains all the leaves on the network that were detected, and searches for the string {{< hl-text blue >}} panelId{{< /hl-text >}}. If this string is found in the layout request, the text will be formatted and split.
From here, that stripped string (Leaf specific ID) will be added to an array named {{< hl-text blue >}} leafId{{< /hl-text >}}. Once the whole string has been iterated over, the {{< hl-text blue >}} leafId{{< /hl-text >}} array is returned. This is vital for the PacketBuilder script.

{{< codeblock "main.py" "python" "" "main.py" >}}
# -*- coding: utf-8 -*-
"""
Method is responsible for getting ID from the nanoleaves.
The Ids are fetched by iterating through a list of leaves from the network
A list of the leaf IDs is returned on the method call
"""
def GetId(layout):
    leafId = []
    for i in range(len(layout)):
        if "panelId" in layout[i]:
            strippedData = layout[i].split(':')
            formatStripped = strippedData[1]
            temp = re.findall(r'\d+', formatStripped)
            leafId.append(temp)
    return leafId
{{< /codeblock >}}

### PacketBuilder()
The {{< hl-text orange >}} PacketBuilder(){{< /hl-text >}} method, the final part of the packet builder process, is shown below. This method is responsible for actually building the request that will be sent to the nanoleaf API.

{{< codeblock "main.py" "python" "" "main.py" >}}
# -*- coding: utf-8 -*-
"""
This method handles building the data to send to the leaves.
The leaf data is compiled into one efficient request and sent to the leaves once
"""
def PacketBuilder():
    global numPanels
    leafId = GetId(outputFormat)
    teamNanoleafColours = ReadPixelsInFile()

    f = open("PacketSent.txt", "w")
    animDataList = []

    # For the amount of leafs on the network
    for i in range(len(leafId)):
        numPanels = len(leafId)
        leafIdNum = str(leafId[i])
        currentLeafId = leafIdNum.strip("[, ', ]")

        # Display animation for 4 seconds
        timeDisplay = "4"

        animDataNew = currentLeafId + " "+str(len(teamNanoleafColours[i]))+" "
        animDataList.append(animDataNew)
        
        # For each RGB value in team values at current ID
        for x in range (len(teamNanoleafColours[i])):
            # Setup & Format time, number of panels and colours to send over network
            animTimeSetup = str(teamNanoleafColours[i][x].rstrip()+" 0 " + timeDisplay +" ")
            tempRGB = animTimeSetup.replace('\\n', '').replace("|", '').replace(",", '').replace("'", '').replace("[", "").replace("]","").replace("\"", "").replace("\\", "").replace("  ", " ")

            # Add current animation sequnce to list
            animDataList.append(tempRGB)

    # Create 1 long string from list
    str1 = ''.join(animDataList)

    # Build custom animation command with packet data
    leafColour = ('{"write": {"animData": "' + '{0} {1}'.format(numPanels, str1[:-1]) + '","loop": true,"animType": "custom","palette": [],"version": "2.0","command": "display"}}')

    # Send long string to nanoleaves with request
    newResponse = requests.put("http://" + str(UniversalIP.rstrip()) + ":" + defaultPort + "/api/v1/" + str(UniversalAuthCode.rstrip()) + "/effects", leafColour)

    # Write packet data to text document to spot syntax issues
    f.write(leafColour)

    checkScore()
    return
{{< /codeblock >}}

The script does a-lot of things. The first of which is iterating over the {{< hl-text blue >}} leafId{{< /hl-text >}} array. For each iteration, a new string is built. The string contains the {{< hl-text blue >}} leafId{{< /hl-text >}}, the time of the animation and the total frames. This string is then appended to an array.

Once all leaf Ids have finished being processed, the array of strings has a for-loop iterate over its elements. This is where the request and format of the request is formed. A new string is created. It contains the previously made string as-well as the RBG colours from the texts.json made earlier in the design stage.

{{< codeblock "main.py" "python" "" "main.py" >}}
# -*- coding: utf-8 -*-
"""
Method handles reading the teams RGB light data.
The method opens and reads a JSON file with all the teams in the premier league
"""
def ReadPixelsInFile():
    global jsonLeafColours
    leafId = GetId(outputFormat)
    currentTeamColours = {}

    # For all leafs available
    for i in range(len(leafId)):
        x = 1

        # Assign var with name of team that scored
        teamName = checkScore.teamScored
        leafIdNum = str(leafId[i])
        currentLeafId = leafIdNum.strip("[, ', ]")

        # Open JSON containing all team names
        path = "teams.json"
        with open(path, 'r') as j:
            teams = json.loads(j.read())

        # If team name is in JSON file
        if (teamName in teams):
            # Create Array to temp store colours
            jsonLeafColours = []

            # While pointer is less than RGB values assigned to the teams (Always 3)
            while x <= 3:
                jsonLeafColours.append(str((teams[teamName][str(x)])))
                x = x + 1

            j.close()

        # Set Dictionary at first leaf ID index equal to the temp array created
        currentTeamColours[i] = jsonLeafColours

    return currentTeamColours
{{< /codeblock >}}

This method above is the function responsible for checking the currents teams the user has chosen to watch and stores their team colours in a dictionary called "{{< hl-text blue >}} currentTeamColours{{< /hl-text >}}". This dictionary has an array of the colours stored in it. The dictionary is returned once the function is finished running.

{{< codeblock "main.py" "python" "" "main.py" >}}
# Build custom animation command with packet data
leafColour = ('{"write": {"animData": "' + '{0} {1}'.format(numPanels, str1[:-1]) + '","loop": true,"animType": "custom","palette": [],"version": "2.0","command": "display"}}')

# Send long string to nanoleaves with request
newResponse = requests.put("http://" + str(UniversalIP.rstrip()) + ":" + defaultPort + "/api/v1/" + str(UniversalAuthCode.rstrip()) + "/effects", leafColour)
{{< /codeblock >}}

The lines of code above are the last of the {{< hl-text orange >}} PacketBuilder(){{< /hl-text >}}. They are responsible for creating a custom {{< hl-text blue >}} animData{{< /hl-text >}} request to send to the nanoleaf API. The {{< hl-text blue >}} leafColour{{< /hl-text >}} variable is sent to the nanoleaf API via a {{< hl-text red >}} PUT{{< /hl-text >}} request. The above code doesn't show the actual request that is sent. The actual request is shown below:

{{< image classes="fancybox center clear" src="https://i.imgur.com/TTtH9Gu.png" thumbnail="https://i.imgur.com/TTtH9Gu.png" group="group:labs-nanoleaf-football" thumbnail-width="70%" thumbnail-height="70%" title="GIF showing a timelapse of me developing the application" >}}

The above image is showing the request that is received by the nanoleaves. This is extremely more efficient and is a great solution to the issue described earlier. With this packet builder complete and working, the final part is using the {{< hl-text red >}} beautiful soup{{< /hl-text >}} library to scrape live football data and change the leaves colours based on the team that scored.

## Getting The Live Football Scores
For this part of the application I wanted to take the quickest route possible. To me, that meant listening for a change of goal on a live scoring webpage and changing the light colours based on the information from the webpage.

Once I decided upon a website ([sportinglife](https://www.sportinglife.com/)), I implemented the final aspects of the code.

{{< codeblock "main.py" "python" "" "main.py" >}}
def checkScore():
    global teamOneScore, url
    global teamTwoScore
    global inputUrl

    # If URL has not been entered-> Input email
    if inputUrl != True:
        url = input("Please enter url of game to listen for: ")

        # If URL is not from website Sportinglife, make user try again
        if "sportinglife" not in url:
            print("URL must be provided from https://www.sportinglife.com")

            checkScore()
            return

        else:
            inputUrl = True

    # If site is accepted -> Request Site HTML data
    response = requests.get(url)

    html_soup = BeautifulSoup(response.text, 'html.parser')

    type(html_soup)

    # Get Score & Teams from HTML
    score_containers = html_soup.find_all('span', class_='MatchScore__StyledScore-wxe8oz-2 dvrzWw')
    teamsPlaying = html_soup.find_all('h2', class_='TeamName__StyledTeamName-sc-1275yii-0 fagppE')
{{< /codeblock >}}

Above is showing the input part of the code. This is where I will enter the URL to the football game. This is then passed to the {{< hl-text red >}} BeautifulSoup{{< /hl-text >}} library. From there I am finding the two classes, in the HTML source code, responsible for displaying the score and the team names. I store both of these elements into variables.

{{< codeblock "main.py" "python" "" "main.py" >}}
# If Team two has scored -> Get nanoleaf layout
elif (int(teamTwo) == teamTwoScore):
    checkScore.teamScored = teamsPlaying[1].text
    teamTwoScore += 1
    GetLayout()
    return

    # If no one has scored, re run function
    else:
        time.sleep(1)
        checkScore()
        return
{{< /codeblock >}}

The code above is the final aspect of the application. It simply checks the function every 1 second waiting for the score for either team to increase. If a team scores than the {{< hl-text orange >}} GetLayout(){{< /hl-text >}} method is called and the application runs, builds the packet/request and the leaves light up. If no team has scored, the method runs itself every second until a team does score. This function will run for the length of the game

# Showcase
{{< image classes="fancybox center clear" src="https://i.imgur.com/kxelHQi.png" thumbnail="https://i.imgur.com/kxelHQi.png" group="group:labs-nanoleaf-football" thumbnail-width="70%" thumbnail-height="70%" title="GIF showing a timelapse of me developing the application" >}}

[//]: # ({% youtube lJIrF4YjHfQ %})
{{< youtube Z9_qjpeSMbk >}}
[Goal Reactive Lighting ManUtdGoal](https://www.youtube.com/watch?v=Z9_qjpeSMbk)

{{< image classes="fancybox nocaption fig-33" src="https://i.imgur.com/oNpiolv.png" thumbnail="https://i.imgur.com/oNpiolv.png" group="group:labs-nanoleaf-football" >}}
{{< image classes="fancybox nocaption fig-33" src="https://i.imgur.com/apyg8AY.png" thumbnail="https://i.imgur.com/apyg8AY.png" group="group:labs-nanoleaf-football" >}}
{{< image classes="fancybox nocaption fig-33" src="https://i.imgur.com/RlJ06gb.png" thumbnail="https://i.imgur.com/RlJ06gb.png" group="group:labs-nanoleaf-football" >}}
{{< image classes="fancybox nocaption clear" src="https://i.imgur.com/i06eyDk.gif" thumbnail="https://i.imgur.com/i06eyDk.gif" group="group:labs-nanoleaf-football" thumbnail-width="100%" title="GIF showing a timelapse of me developing the application" >}}

[//]: # (thumbnail-height="50px" thumbnail-width="50px")
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


