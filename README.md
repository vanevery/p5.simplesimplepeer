# p5.simplesimplepeer
Simple P5 WebRTC

Include this library in a p5 sketch and share audio/video streams or the canvas itself as a stream.  Can also share data (string only for now).  All WebRTC so Peer to Peer.  

Running a public signaling server on https://simplesimplepeer.itp.io - Run your own signalling server by running server.js (with Node, Express and Socket.io).  

## Getting Started

Requires [simplepeer](https://github.com/feross/simple-peer) and [socket.io](https://socket.io/) be included in the HTML:
```
<script type="text/javascript" src="https://simplesimplepeer.itp.io/simplepeer.min.js"></script>
```
```
<script type="text/javascript" src="https://simplesimplepeer.itp.io/socket.io.js"></script>
```

Of course, this library needs to be included as well:
```
<script type="text/javascript" src="https://simplesimplepeer.itp.io/simplesimplepeer.js"></script>
```

Use the callback from [createCapture](https://p5js.org/reference/#/p5/createCapture) to get at the media stream.  Instantiate SimpleSimplePeer with a reference to the sketch (this), a string indicating if this is audio/video ("CAPTURE") or a canvas ("CANVAS"), the media stream, and a unique room name.  The sketch id from the p5 editor works well (in this case, "jZQ64AMJc").  Add a callback for the "stream" event, in this case, a function defined later called "gotStream":
```
let myVideo = null;

function setup() {
  createCanvas(400,400);

  myVideo = createCapture(VIDEO, 
    function(stream) {
	    let ssp = new SimpleSimplePeer(this, "CAPTURE", stream, "jZQ64AMJc")
  	  ssp.on('stream', gotStream);
    }
  );
}
```

You can specify the normal [MediaStreamConstraints](https://developer.mozilla.org/en-US/docs/Web/API/MediaStreamConstraints) to specify framerates, sizes, and stream types.  You'll want to mute your local video to prevent feedback:
```
let myVideo = null;

function setup() {
  createCanvas(400,400);
  
  let constraints = {audio: true, video: true};
  myVideo = createCapture(constraints, 
    function(stream) {
      let ssp = new SimpleSimplePeer(this, "CAPTURE", stream, "jZQ64AMJc")
      ssp.on('stream', gotStream);
    }
  );

  myVideo.elt.muted = true;
}
```

The "stream" callback gives a normal video element: 
```
let otherVideo;
function gotStream(stream, id) {
  otherVideo = stream;
  //otherVideo.id and id are the same and unique identifier
}
```

Both video elements can be used in the draw loop:
```
function draw() {
  if (myVideo != null) {
    image(myVideo,0,0,width/2,height);
    text("My Video", 10, 10);
  }

  if (otherVideo != null) {
    image(otherVideo,width/2,0,width/2,height);
    text("Their Video", width/2+10, 10);
  }  
}
```

Alternatively the p5 Canvas can be streamed instead of video:


More documentation forthcoming.

Examples
* Basic Video Example: https://editor.p5js.org/shawn/sketches/jZQ64AMJc
* Basic Audio/Video Example (on same canvas): https://editor.p5js.org/shawn/sketches/2AXFd9TLV
* Basic Canvas Example: https://editor.p5js.org/shawn/sketches/e4LTqKI8Q
* Video on Canvas Example: https://editor.p5js.org/shawn/sketches/U396jFtFT
* Another Video on Canvas Example: https://editor.p5js.org/shawn/sketches/fW5DrBPAK
* Data Only: https://editor.p5js.org/shawn/sketches/w83C-S6DU

* ADVANCED:  Manipulated Video on Canvas + Audio: https://editor.p5js.org/shawn/sketches/SbNzhNujd

Contributed Examples
* Video + Data: https://editor.p5js.org/AidanNelson/sketches/8EcgJpEUi
* Flocking Video: https://editor.p5js.org/AidanNelson/sketches/yu2CjoP8H
* ADVANCED: Frame Differencing: https://editor.p5js.org/dano/sketches/ZVOoN1GB9
