# Presentz.js

Presentz.js is a javascript library for synchronizing videos and slides.

It's at the heart of http://presentz.org/, a website for freely publishing conference talks, pitches, lessons and the like. [Presentz.org](https://github.com/ffissore/presentz.org) is free software as well.

## Dependencies quick reference

(skip if you are still not using presentz.js but just reading this README)

Depending on which video and slide sources you are using, you need zero to a couple of additional js libraries

- [SWFObject](https://code.google.com/p/swfobject/): if your slides are made with flash or if they are hosted on slideshare
- [Froogaloop](http://developer.vimeo.com/player/js-api): if your video is on Vimeo
- [Mediaelementjs](http://mediaelementjs.com/): if you host your video files on your own. There are also .css files and images to setup a good looking player, so using medialementjs requires some additional effort.
- [Youtube IFrame API](https://developers.google.com/youtube/iframe_api_reference): if your videos are on youtube

## A JSON file

Presentz.js starts with a presentation, a json object whose structure is

```json
{
  "title": "Video tag, images as slides",
  "chapters": [
    {
      "title": "Part 1",
      "duration": 21,
      "video": {
        "url": "http://presentz.org/assets/demo/demo1.webm",
        "thumb": "http://presentz.org/assets/demo/videotag-img.png"
      },
      "slides": [
        {
          "url": "http://presentz.org/assets/demo/slide1.png",
          "time": 0
        },
        {
          "url": "http://presentz.org/assets/demo/slide2.png",
          "time": 7
        },
        {
          "url": "http://presentz.org/assets/demo/slide3.png",
          "time": 9.5
        },
        {
          "url": "http://presentz.org/assets/demo/slide4.png",
          "time": 14
        }
      ]
    }
  ]
}
```

(A slightly richer version of this presentation can be seen at http://presentz.org/demo/01_videotag-img)

In essence:

- each presentation has a `title` and a list of `chapters`
- each chapter has 
  - a `duration` (expressed in seconds)
  - a `video` with a `url`
  - a list of `slides`
- each slide has a `url` and a `time` (expressed in seconds)

The most important information is the `time` of a slide: it's used to determine when a slidechange has to occur and which slide has to be displayed. Such time is relative to the video of the containing chapter.

## Supported video sources

Presentz.js wants to use and reuse everything is already available, but we can rely only on those video streaming services that provide a "player API". As a fallback, we can host our video files on our own servers (or on any other webserver, think amazon s3, dropbox...)

At the moment, Presentz.js supports Youtube, Vimeo and raw video sources (as in the JSON above).

### Youtube

Use the youtube url in the `video.url` property

```json
"video": {
  "url": "http://youtu.be/hJgncy4I1ig",
  "thumb": "/assets/demo/youtube-slideshare.png"
},
```

Works with both the long url `http://www.youtube.com/watch?v=hJgncy4I1ig` and the short one `http://youtu.be/hJgncy4I1ig`

### Vimeo

Use the vimeo url in the `video.url` property

```json
"video": {
  "url": "http://vimeo.com/27902834",
  "thumb": "/assets/demo/vimeo-img.png"
},
```

### Raw video files

Use the url to the video file in the `video.url` property

```json
"video": {
  "url": "http://presentz.org/assets/demo/demo1.webm",
  "thumb": "http://presentz.org/assets/demo/videotag-img.png"
},
```

Raw video files require mediaelementjs is already setup (if you just put in their .js file, the player will be so ugly to be unusable)

## Supported slide sources

### Images and SWF files

You can export a presentation made with [LibreOffice](https://www.libreoffice.org/) to images (should be "File -> Export -> HTML Document"). Or you can convert a PDF to a series of SWF with [SWFTools](http://www.swftools.org/).

Either way, once you have your files, put the url in the `slide.url` property

```json
{
  "url": "http://presentz.org/assets/demo/slide1.png",
  "time": 0
},
```

### SlideShare

Slideshows on slideshare are identified by an ID they call `PPTLocation` (in their [API](http://www.slideshare.net/developers/documentation#get_slideshow)) and `doc` (in their [player API](http://www.slideshare.net/developers/playerapi)).

Use that ID to compose a fake slideshare URL, like

```json
{
  "url": "http://www.slideshare.net/slides-110818145820-phpapp02#1",
  "time": 0
},
```

where: 
- `http://www.slideshare.net` is used to activate the slideshare plugin
- `slides-110818145820-phpapp02` is the ID (or doc, or PPTLocation)
- `#1` is the slide number (one based)

### Speakerdeck

Slideshows on speakerdeck are identified by an ID but, unfortunately, there is yet no public API to have this ID from the slideshow public url. You can get it by clicking the "Embed" link on the right of a slideshow, "Embed" again and looking for a `data-id` attribute in the `<script../>` snippet just below.

Use that ID to compose a fake speakerdeck URL, like

```json
{
  "url": "https://speakerdeck.com/4ffbeed2df7b3f00010233bf#1",
  "time": 0
},
```

where: 
- `https://speakerdeck.com` is used to activate the speakerdeck plugin
- `4ffbeed2df7b3f00010233bf` is the ID
- `#1` is the slide number (one based)

### Rvl.io

[Reveal.js](http://lab.hakim.se/reveal-js/) is framework to creating HTML + CSS with 3D transforms based slideshows, very good looking.

Support in presentz.js is experimental and expects the slides to be hosted on http://www.rvl.io/

Use the base URL and append the number of the slide you want to show, for example:

```json
{
  "url": "http://www.rvl.io/federico/presentz/#/0",
  "time": 0
}
```

where:
- `http://www.rvl.io/federico/presentz/` is the base url
- `#/0` is the slide number (zero based)

Reveal.js supports vertical slides, that are a sort of "sub slides" of a parent one. To show those slides, append the number to the parent slide number, for example

```json
{
  "url": "http://www.rvl.io/federico/presentz/#/0/1",
  "time": 10
}
```
