# How to play a Youtube video in a Blazor Web Application 

**Note**: For more information about this sample visit these URLs 

https://developers.google.com/youtube

https://developers.google.com/youtube/iframe_api_reference

## 1. Create a Blazor Web Application with Visual Studio 2022 Commuity Edition


## 2. Create a new JavaScript file

We create a new JavaScript for integrating a YouTube video player into a webpage

We right click on the wwwroot folder and we select the menu option Add New Element

![image](https://github.com/user-attachments/assets/1022fea5-27ad-4d72-8e5a-a1c08e2a2f03)

We input the new JavaScript file name **youtubePlayer.ja**:

![image](https://github.com/user-attachments/assets/3485cb66-7a92-491c-8a71-60275635c431)

See the application folders and files structure

![image](https://github.com/user-attachments/assets/07e6e356-9137-4abf-964b-40a4b83f2e0a)

This is the new **youtubePlayer.ja** sourcecode:

```javascript
var tag = document.createElement('script');
tag.id = 'iframe-demo';
tag.src = 'https://www.youtube.com/iframe_api';
var firstScriptTag = document.getElementsByTagName('script')[0];
firstScriptTag.parentNode.insertBefore(tag, firstScriptTag);

var player;
function onYouTubeIframeAPIReady() {
    player = new YT.Player('existing-iframe-example', {
        events: {
            'onReady': onPlayerReady,
            'onStateChange': onPlayerStateChange
        }
    });
}
function onPlayerReady(event) {
    document.getElementById('existing-iframe-example').style.borderColor = '#FF6D00';
}
function changeBorderColor(playerStatus) {
    var color;
    if (playerStatus == -1) {
        color = "#37474F"; // unstarted = gray
    } else if (playerStatus == 0) {
        color = "#FFFF00"; // ended = yellow
    } else if (playerStatus == 1) {
        color = "#33691E"; // playing = green
    } else if (playerStatus == 2) {
        color = "#DD2C00"; // paused = red
    } else if (playerStatus == 3) {
        color = "#AA00FF"; // buffering = purple
    } else if (playerStatus == 5) {
        color = "#FF6D00"; // video cued = orange
    }
    if (color) {
        document.getElementById('existing-iframe-example').style.borderColor = color;
    }
}
function onPlayerStateChange(event) {
    changeBorderColor(event.data);
}
```

## 3. Include the JavaScript file reference in the App.razor component

We include the following reference in the App.razor:

```
<script src="youtubePlayer.js"></script>
```

This is the App.razor whole code:

```razor
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <base href="/" />
    <link rel="stylesheet" href="bootstrap/bootstrap.min.css" />
    <link rel="stylesheet" href="app.css" />
    <link rel="stylesheet" href="YoutubePlayer.styles.css" />
    <link rel="icon" type="image/png" href="favicon.png" />
    <HeadOutlet @rendermode="InteractiveServer" />
</head>

<body>
    <Routes @rendermode="InteractiveServer" />
    <script src="_framework/blazor.web.js"></script>
    <script src="youtubePlayer.js"></script>
</body>

</html>
```

# 4. Create a new Razor Component for invoking the JavaScript functions

We right click on the **Pages** folder and select the menu option Add New Razor Component

![image](https://github.com/user-attachments/assets/aeb626b0-109e-45ab-946b-f527afaa8b01)

And we input the component name **YoutubePlayerComponent.razor**

![image](https://github.com/user-attachments/assets/e7fd01e3-0a10-45c7-881a-a7949ba0f2c9)

This is the **YoutubePlayerComponent.razor** source code:

```razor
@page "/player"

@inject IJSRuntime JSRuntime

<h1>YouTube Video Player</h1>

<!-- Container for the four videos -->
<div class="video-container">
    <iframe id="video1"
            width="300" height="200"
            src="https://www.youtube.com/embed/M7lc1UVf-VE?enablejsapi=1"
            frameborder="0"
            style="border: solid 4px #37474F">
    </iframe>

    <iframe id="video2"
            width="300" height="200"
            src="https://www.youtube.com/embed/M7lc1UVf-VE?enablejsapi=1"
            frameborder="1"
            style="border: solid 4px #37474F">
    </iframe>

    <iframe id="video3"
            width="300" height="200"
            src="https://www.youtube.com/embed/M7lc1UVf-VE?enablejsapi=1"
            frameborder="0"
            style="border: solid 4px #37474F">
    </iframe>

    <iframe id="video4"
            width="300" height="200"
            src="https://www.youtube.com/embed/M7lc1UVf-VE?enablejsapi=1"
            frameborder="0"
            style="border: solid 4px #37474F">
    </iframe>
</div>

<br />

<button class="btn btn-primary" width="10" height="10" @onclick="MessageShow">Click me</button>

<br />

<br />

<p>@message</p>

@code {
    protected override async Task OnAfterRenderAsync(bool firstRender)
    {
        if (firstRender)
        {
            // Ensure the YouTube API is loaded
            await JSRuntime.InvokeVoidAsync("eval", "onYouTubeIframeAPIReady();");
        }
    }

    private string message = "";

    private void MessageShow()
    {
        message = "Hola Amigo!";
    }
}

<!-- Add this CSS to style the container and the spacing between the videos -->
<style>
    .video-container {
        display: flex;
        justify-content: space-between;
        gap: 20px; /* Space between the videos */
        flex-wrap: wrap; /* Ensure it wraps in case the screen size is too small */
    }
</style>
```

