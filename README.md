# RainwavePlayer

A library to mask the audio element and the difficulties of streaming in HTML5.

## Synopsis:

```javascript
if (!RainwavePlayer.isSupported) {
	alert("No HTML5 audio support!");
}
else {
	RainwavePlayer.useStation("Game");
	RainwavePlayer.addEventListener("error", function() { alert("The RainwavePlayer stopped working!") });
	RainwavePlayer.play();
	stopButton.addEventListener("click", RainwavePlayer.stop);
}
```

## Functions:

* `useStation(station)  :` Accepts station IDs or station names to setup audio sources. (e.g. "All")
* `useStreamURLs(array) :` An array of source URL strings.
* `playToggle()         :` Stops/starts playback.
* `play()               :` Starts playback.
* `stop()               :` Stops playback.
* `toggleMute()         :` Toggles mute.
* `setVolume(newVolume) :` Sets volume.  Must be between 0.0 and 1.0.

## Events:

The following events can be emitted by the library:
* "playing" is emitted when the audio starts playing, or resumes after a stall
* "stop" when the audio has been completely shutdown
* "change" on playing or stop, status can be checked with `RainwavePlayer.isPlaying`
* "volumeChange" triggers on `setVolume()`, mute (volume will `=== 0`), and unmute
* "loading" when the stream is initializing
* "stall" when the stream has a hiccup.  (warning: can be thrown more than once for one stall!)
* "error" when the audio playback fails (this is 99% of the time network issues)
* "longLoadWarning" when the library thinks the audio element will take >20 seconds to start

Events do not have any detail associated with them, except stall, which will provide a string
as event.detail with the reason for the stall.

Note about "stall": the browser can throw its own stall events many times, repeatedly, before
it will revert to "playing" or "stop".  Please make sure you have your own code setup
to handle repeated errors!

## Setters:

* `audioElDest   :` Optional for compaibility - what DOM element to place the audio tag in.
                Set this to be somewhere on your page, even if visiblity: hidden or
                outside the browser's viewport, to avoid trouble with some browsers.
                If you forget to define it, the library will generate a div to use but
                will not append it anywhere on the page.

## Getters/Properties:

* `isSupported   :` Boolean: Can the browser play back HTML5 audio?
* `isPlaying     :` Boolean: Is the library playing/trying to play (true) or stopped? (false)
* `isMuted       :` Boolean.
* `type          :` String : What kind of audio does the browser support: "Vorbis" or "MP3"?
* `volume        :` Number : 0.0 to 1.0. 0.0 during mute.

All properties can be changed, but please don't.  You'll break your own player. :)