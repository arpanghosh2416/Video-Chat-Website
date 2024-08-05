"# Video-Chat-Website"
DOCUMENTATION OF THE VIDEO-CHAT-WEBSITE USING SOCKET.IO IN JS- A WEB SOCKET PROGRAMMING.

We are using Node Js for the server and simple Html and CSS with it to create the web socket programming.
Create a directory say video-chat-website and in that folder and open cmd from there and write - npm init.

Now it’s a node js project. Fill out the required information which will be chosen by default.
Install the dependencies inside of the video-chat-website folder: -

1. npm install -g nodemon
2. npm install express ejs socket.io uuid peer

We will create a server.js file inside this folder where we will write all our logic for the server using request and response concept where socket.io will be implemented.
Creating our server using Express Js : -
The first thing we need to do is get our server up and running. We’re going to use Express to accomplish this. Express is a minimalist web framework for Node.js — Express makes it very easy to create and run a web server with Node.
Let’s create a boilerplate Express starter app file in #server.js: -
The code for the server.js is: -
// server.js
const express = require(“express”);
const app = express();
const server = require(“http”).Server(app);
app.get(“/”, (req, res) => {
res.status(200).send(“Hello World”);
});
server.listen(3030);

Our server is ready to go.
Now run the command: - nodemon server.js
Nodemon will be having the following features: -
• Automatic restarting of application.
• Detects default file extension to monitor.
• Default support for node but easy to run any executable, such as python, ruby, make, etc.
• Ignoring specific files or directories.
• Watch specific directories.
• Works with server applications or one time run utilities and REPLs.
• Scriptable through node require statements.

Why socket.io?
Click in the link socket.io for the documentation of this dependency.

The server is up and write in your browser: -
Localhost:3030. You will be able to see “Hello World” in your browser.

In the server.js add the line: -
app.set(‘view engine’, ‘ejs’);

Prepare the file structure: -

1. Video-Chat-website
   1.1. Public
   1.1.1 script.js
   1.1.2 style.css
   1.2. views
   1.2.1. room.ejs
   1.3. server.js

In this room.ejs we will write ecma js as html5: -

<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Young-Architects</title>
    <link rel="stylesheet" href="style.css" />
    <script src="/socket.io/socket.io.js"></script>
    <script src="https://kit.fontawesome.com/c939d0e917.js"></script>
    <script src="https://unpkg.com/peerjs@1.3.1/dist/peerjs.min.js"></script>
    <script>
      //const ROOM_ID = "<%= roomId %>";
      //const ROOM_ID = "caddf02f-4cb5-4dcf-b37d-050474c5268b";
    </script>
  </head>
  <body>
    <div class="header">
      <div class="logo">
        <div class="header__back">
          <i class="fas fa-angle-left"></i>
        </div>
        <h3>Video Chat</h2>
      </div>
    </div>  
    <div class="main">  
    <div class="main__left">
      <div class="videos__group">
        <div id="video-grid">

        </div>
      </div>
      <div class="options">
        <div class="options__left">
          <div id="stopVideo" class="options__button">
            <i class="fa fa-video-camera"></i>
          </div>
          <div id="muteButton" class="options__button">
            <i class="fa fa-microphone"></i>
          </div>
          <div id="showChat" class="options__button">
            <i class="fa fa-comment"></i>
          </div>
        </div>
        <div class="options__right">
          <div id="inviteButton" class="options__button">
            <i class="fas fa-user-plus"></i>
          </div>
        </div>
      </div>
    </div>
    <div class="main__right">
      <div class="main__chat_window">
          <div class="messages">

          </div>
      </div>
      <div class="main__message_container">
        <input id="chat_message" type="text" autocomplete="off" placeholder="Type message here...">
        <div id="send" class="options__button">
          <i class="fa fa-plus" aria-hidden="true"></i>
        </div>
      </div>
    </div>

  </div>
  </body>
  <script src="script.js"></script>
</html>

In server.js the code run as:
const express = require("express");
const app = express();
const server = require("http").Server(app);
const { v4: uuidv4 } = require("uuid");
app.set("view engine", "ejs");
const io = require("socket.io")(server, {
cors: {
origin: '\*'
}
});
const { ExpressPeerServer } = require("peer");
const opinions = {
debug: true,
}

app.use("/peerjs", ExpressPeerServer(server, opinions));
app.use(express.static("public"));

app.get("/", (req, res) => {
res.redirect(`/${uuidv4()}`);
});

app.get("/:room", (req, res) => {
res.render("room", { roomId: req.params.room });
});

io.on("connection", (socket) => {
socket.on("join-room", (roomId, userId, userName) => {
socket.join(roomId);
setTimeout(()=>{
socket.to(roomId).broadcast.emit("user-connected", userId);
}, 1000)
socket.on("message", (message) => {
io.to(roomId).emit("createMessage", message, userName);
});
});
});

server.listen(process.env.PORT || 3030);
In script.js, the file goes as:
const socket = io("/");
const videoGrid = document.getElementById("video-grid");
const myVideo = document.createElement("video");
const showChat = document.querySelector("#showChat");
const backBtn = document.querySelector(".header\_\_back");
myVideo.muted = true;

backBtn.addEventListener("click", () => {
document.querySelector(".main**left").style.display = "flex";
document.querySelector(".main**left").style.flex = "1";
document.querySelector(".main**right").style.display = "none";
document.querySelector(".header**back").style.display = "none";
});

showChat.addEventListener("click", () => {
document.querySelector(".main**right").style.display = "flex";
document.querySelector(".main**right").style.flex = "1";
document.querySelector(".main**left").style.display = "none";
document.querySelector(".header**back").style.display = "block";
});

const user = prompt("Enter your name");

var peer = new Peer({
host: '127.0.0.1',
port: 3030,
path: '/peerjs',
config: {
'iceServers': [
{ url: 'stun:stun01.sipphone.com' },
{ url: 'stun:stun.ekiga.net' },
{ url: 'stun:stunserver.org' },
{ url: 'stun:stun.softjoys.com' },
{ url: 'stun:stun.voiparound.com' },
{ url: 'stun:stun.voipbuster.com' },
{ url: 'stun:stun.voipstunt.com' },
{ url: 'stun:stun.voxgratia.org' },
{ url: 'stun:stun.xten.com' },
{
url: 'turn:192.158.29.39:3478?transport=udp',
credential: 'JZEOEt2V3Qb0y27GRntt2u2PAYA=',
username: '28224511:1379330808'
},
{
url: 'turn:192.158.29.39:3478?transport=tcp',
credential: 'JZEOEt2V3Qb0y27GRntt2u2PAYA=',
username: '28224511:1379330808'
}
]
},

debug: 3
});

let myVideoStream;
navigator.mediaDevices
.getUserMedia({
audio: true,
video: true,
})
.then((stream) => {
myVideoStream = stream;
addVideoStream(myVideo, stream);

    peer.on("call", (call) => {
      console.log('someone call me');
      call.answer(stream);
      const video = document.createElement("video");
      call.on("stream", (userVideoStream) => {
        addVideoStream(video, userVideoStream);
      });
    });

    socket.on("user-connected", (userId) => {
      connectToNewUser(userId, stream);
    });

});

const connectToNewUser = (userId, stream) => {
console.log('I call someone' + userId);
const call = peer.call(userId, stream);
const video = document.createElement("video");
call.on("stream", (userVideoStream) => {
addVideoStream(video, userVideoStream);
});
};

peer.on("open", (id) => {
console.log('my id is' + id);
socket.emit("join-room", ROOM_ID, id, user);
});

const addVideoStream = (video, stream) => {
video.srcObject = stream;
video.addEventListener("loadedmetadata", () => {
video.play();
videoGrid.append(video);
});
};

let text = document.querySelector("#chat_message");
let send = document.getElementById("send");
let messages = document.querySelector(".messages");

send.addEventListener("click", (e) => {
if (text.value.length !== 0) {
socket.emit("message", text.value);
text.value = "";
}
});

text.addEventListener("keydown", (e) => {
if (e.key === "Enter" && text.value.length !== 0) {
socket.emit("message", text.value);
text.value = "";
}
});

const inviteButton = document.querySelector("#inviteButton");
const muteButton = document.querySelector("#muteButton");
const stopVideo = document.querySelector("#stopVideo");
muteButton.addEventListener("click", () => {
const enabled = myVideoStream.getAudioTracks()[0].enabled;
if (enabled) {
myVideoStream.getAudioTracks()[0].enabled = false;
html = `<i class="fas fa-microphone-slash"></i>`;
muteButton.classList.toggle("background**red");
muteButton.innerHTML = html;
} else {
myVideoStream.getAudioTracks()[0].enabled = true;
html = `<i class="fas fa-microphone"></i>`;
muteButton.classList.toggle("background**red");
muteButton.innerHTML = html;
}
});

stopVideo.addEventListener("click", () => {
const enabled = myVideoStream.getVideoTracks()[0].enabled;
if (enabled) {
myVideoStream.getVideoTracks()[0].enabled = false;
html = `<i class="fas fa-video-slash"></i>`;
stopVideo.classList.toggle("background**red");
stopVideo.innerHTML = html;
} else {
myVideoStream.getVideoTracks()[0].enabled = true;
html = `<i class="fas fa-video"></i>`;
stopVideo.classList.toggle("background**red");
stopVideo.innerHTML = html;
}
});

inviteButton.addEventListener("click", (e) => {
prompt(
"Copy this link and send it to people you want to meet with",
window.location.href
);
});

socket.on("createMessage", (message, userName) => {
messages.innerHTML =
messages.innerHTML +
`<div class="message">
        <b><i class="far fa-user-circle"></i> <span> ${userName === user ? "me" : userName
    }</span> </b>
        <span>${message}</span>
    </div>`;
});
In style.css the code goes as:
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@400;500;600&display=swap");

:root {
--main-darklg: #1d2635;
--main-dark: #161d29;
--primary-color: #2f80ec;
--main-light: #eeeeee;
font-family: "Poppins", sans-serif;
}

- {
  margin: 0;
  padding: 0;
  }

.header {
display: flex;
justify-content: center;
align-items: center;
height: 8vh;
position: relative;
width: 100%;
background-color: var(--main-darklg);
}

.logo > h3 {
color: var(--main-light);
}

.main {
overflow: hidden;
height: 92vh;
display: flex;
}

.main\_\_left {
flex: 0.7;
display: flex;
flex-direction: column;
}

.videos\_\_group {
flex-grow: 1;
display: flex;
justify-content: center;
align-items: center;
padding: 1rem;
background-color: var(--main-dark);
}

video {
height: 300px;
border-radius: 1rem;
margin: 0.5rem;
width: 400px;
object-fit: cover;
-moz-transform: rotateY(180deg);
-webkit-transform: rotateY(180deg);
transform: rotateY(180deg);
}

.options {
padding: 1rem;
display: flex;
background-color: var(--main-darklg);
}

.options\_\_left {
display: flex;
}

.options\_\_right {
margin-left: auto;
}

.options\_\_button {
display: flex;
justify-content: center;
align-items: center;
background-color: var(--primary-color);
height: 50px;
border-radius: 5px;
color: var(--main-light);
font-size: 1.2rem;
width: 50px;
margin: 0 0.5rem;
}

.background\_\_red {
background-color: #f6484a;
}

.main\_\_right {
display: flex;
flex-direction: column;
flex: 0.3;
background-color: #242f41;
}

.main\_\_chat_window {
flex-grow: 1;
overflow-y: scroll;
}

.main\_\_chat_window::-webkit-scrollbar {
display: none;
}

.main\_\_message_container {
padding: 1rem;
display: flex;
align-items: center;
justify-content: center;
}

.main\_\_message_container > input {
height: 50px;
flex: 1;
font-size: 1rem;
border-radius: 5px;
padding-left: 20px;
border: none;
}

.messages {
display: flex;
flex-direction: column;
margin: 1.5rem;
}

.message {
display: flex;
flex-direction: column;
}

.message > b {
color: #eeeeee;
display: flex;
align-items: center;
text-transform: capitalize;
}

.message > b > i {
margin-right: 0.7rem;
font-size: 1.5rem;
}

.message > span {
background-color: #eeeeee;
margin: 1rem 0;
padding: 1rem;
border-radius: 5px;
}

#video-grid {
display: flex;
justify-content: center;
flex-wrap: wrap;
}

#showChat {
display: none;
}

.header\_\_back {
display: none;
position: absolute;
font-size: 1.3rem;
top: 17px;
left: 28px;
color: #fff;
}

@media (max-width: 700px) {
.main**right {
display: none;
}
.main**left {
width: 100%;
flex: 1;
}

video {
height: auto;
width: 100%;
}

#showChat {
display: flex;
}
}
