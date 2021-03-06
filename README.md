# RainwavePlayer

A library to mask the audio element and the difficulties of streaming in HTML5.

## Synopsis:

No external requirements or shims required.

HTML component (optional):
```html
<head>
	<script src="/static/RainwavePlayer.min.js"></script>
</head>

<body>
	<div id="play">Play</div>
	<div id="stop">Stop</div>
	<div id="audioContainer" style="margin-top: 1em;"></div>
</body>
```
Javascript:
```javascript
if (!RainwavePlayer.isSupported) {
	alert("No HTML5 audio support!");
}
else {
	// optional display of audio element to user
	Rainwave.audioElDest = document.getElementById("audioContainer");

	RainwavePlayer.useStation("Game");
	RainwavePlayer.addEventListener("error", function() { alert("The RainwavePlayer stopped working!") });
	RainwavePlayer.play();
	document.getElementById("play").addEventListener("click", RainwavePlayer.play);
	document.getElementById("stop").addEventListener("click", RainwavePlayer.stop);
}
```

## Functions:

| Function                              | Description   |
| ------------------------------------- | ------------- |
| `useStation(station) `                | Accepts station IDs or station names to setup audio sources. (names: All, Game, Chiptune, Covers, OCR) |
| `useStreamURLs(array)`                | An array of source URL strings, e.g. `[ "http://my.radio.com:8000/" ]` |
| `playToggle()        `                | Stops/starts playback. |
| `play()              `                | Starts playback. |
| `stop()              `                | Stops playback. |
| `toggleMute()        `                | Toggles mute. |
| `setVolume(newVolume)`                | Sets volume.  newVolume must be between `0.0` and `1.0`.  Will not unmute. |
| `addEventListener(type, function)`    | Similar to DOM event listening. |
| `removeEventListener(type, function)` | Similar to DOM event listening. |

## Events:

The following events can be emitted by the library:
* `playing` is emitted when the audio starts playing, or resumes after a stall
* `stop` when the audio has been completely shutdown
* `change` on playing or stop, status can be checked with `RainwavePlayer.isPlaying`
* `volumeChange` triggers on `setVolume()`, mute (volume will `=== 0`), and unmute
* `loading` when the stream is initializing
* `stall` when the stream has a hiccup.  (warning: can be thrown more than once for one stall!)
* `error` when the audio playback fails (this is 99% of the time network issues)
* `longLoadWarning` when the library thinks the audio element will take >20 seconds to start

Events do not have any detail associated with them, except stall, which will provide a string
as event.detail with an indication of how many streamURLs it has exhausted.

Note about "stall": the browser can throw its own stall events many times, repeatedly, before
it will revert to "playing" or "stop".  Please make sure you have your own code setup
to handle repeated errors!

## Properties:

| Property       | Type     | Description   |
| -------------- | -------- | ------------- |
| `isSupported ` | Boolean  | Can the browser play back HTML5 audio? |
| `isPlaying   ` | Boolean  | Is the library playing/trying to play (`true`) or stopped? (`false`) |
| `isMuted     ` | Boolean  | `true` when muted. |
| `type        ` | String   | What kind of audio does the browser support: "Vorbis" or "MP3"? |
| `mimetype    ` | String   | MIME type of the supported audio format, e.g. `audio/ogg` |
| `volume      ` | Number   | `0.0` to `1.0`. `0.0` during mute. |
| `audioElDest ` | DOM Node | Optional for compatability.  Provide a DOM node (getElementById/etc) for RainwavePlayer to place an audio DOM node in. Set this to be somewhere on your page, even if visiblity: hidden or outside the browser's viewport, to avoid trouble with some browsers. If you forget to define it, the library will generate a div to use but will not append it anywhere on the page, which works in almost cases. |

All properties can be changed, though doing so may break the player.

### Forcing Compatibility

If you are dead certain your situation can playback audio and `isSupported` is false, you can
force a playback attempt:

```javascript
RainwavePlayer.isSupported = true;
RainwavePlayer.type = "mp3";
RainwavePlayer.type = "audio/mpeg";
RainwavePlayer.useStation("All");
RainwavePlayer.play();
```