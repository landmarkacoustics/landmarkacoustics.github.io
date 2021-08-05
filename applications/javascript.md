---
title: Javascript Code for Client Browsers
author: Ben Taft
copyright: 2020 by Landmark Acoustics LLC
---

# Javascript Code for Client Browsers

## Structure thoughts from October 2020

The client consists of three components.
There is a user interface, of course.
There is logic for performing the WebSocket connection as well.
In between these two components is the code for capturing audio.

At its simplest, the user interface could be one button.
That button would control starting and stopping recording.
However, there should also be a feedback text box.
More advanced user interfaces will include controls for choosing a context.
These may be handled by the Django html maker, rather than a script, though.

The audio code has the potential to be the most complicated.
There are several different ways that it could grab the sound data, depending
upon the browser and hardware.
The easiest thing to do would be to use the WebRecording API, which can render
a FLAC-compressed Ogg stream that can be sent directly over the WebSocket.
It seems like only Firefox supports that API right now, though.
Another option is to use the WebAudio API to encode raw sound samples into a
format that is good to send over a WebSocket.
In that case, I probably need to use a third-party ZLIB implementation to keep
the connection as efficient as possible.
Safari may need to use a third way to handle data.
So, that sucks.

More concretely, the audio logic also must be considered in parts.
First, it has to query the client system's recording capabilities.
It has to create a transmittable summary that includes the sample and bit rates
for the audio, the number of channels and the encoding format.
Ultimately it should actually try to collect hardware information so that we
can account for idiosyncracies of frequency response and such.
Next, it has to establish a hierarchy of recording formats from most- to
least-preferred and negotiate with the server about which one to use.
Finally, it has to construct and manage the data stream that pipes audio from
the system's microphone to the WebSocket.

The WebSocket logic may need very little help.
I think that authentication will be handled during the initial HTTP conneciton
upgrade because cookies from Django will be sent along.
I need to verify that.
Once the connection happens the server needs to know about the format that the
audio stream will have, and then it's just pipe the data until the user clicks
"stop."

## Audio Logic

### Querying the system's capabilities

There is no unified way to query a client system's recording capabilities.
The are three Web APIs that we might want to use. [MediaDevices][1] is where we
get the stream from the microphone. However, it might not provide details like
the sample rate that the microphone records at! [Media Capabilities][2] may
provide information about the available devices even before the stream begins.
However, there seems to be a disconnect between the codecs that it says is
possible and the codecs that the browser will actually use.
[MediaStream Recording][3], if it is available, provides a file stream in a
specific audio codec. 

I noodled around with trying to interrogate the system about its capabilities.
It looks like there is no way to guarantee that either a `MediaStream` or an
`AudioContext` will actually tell you what its bitrate is.

Aha! I need to create `OfflineAudiocContext` objects. The online equivalent has
its outputs tied to a speaker. It requires three arguments: `numberOfChannels`,
`length` (The number of the samples in the buffer will be
`length / numberOfChannels`.), and `sampleRate`. **NOPE**, you can't connect a
`MediaStreamTrackAudioSourecNode` to an `OfflineAudioContext`.

It looks like `AudioWorkletNode` is the way to go.


[1]:https://developer.mozilla.org/en-US/docs/Web/API/MediaDevices
[2]:https://developer.mozilla.org/en-US/docs/Web/API/Media_Capabilities_API
[3]:https://developer.mozilla.org/en-US/docs/Web/API/MediaStream_Recording_API
