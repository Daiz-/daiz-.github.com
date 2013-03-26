---
layout: post
title: "Don't Buy the Snake Oil of Beamr Video"
date: 2013-02-26 22:50
comments: false
categories: 
- Video
- H.264
- Snake Oil
---
You might have heard of **[Beamr Video](http://beamrvideo.com/)**, and their impressive claims about reducing video bitrates by *"up to 4x, without losing quality"*. Sounds too good to be true? Well, as a matter of fact, it is.

<!-- more -->

## The Example Videos

The four example videos that Beamr has on their site use *very high* bitrates - 40-50 Mbps for 1080p video. These are the kind of bitrates you find on Blu-ray discs, whereas with something like Netflix's ["SuperHD"](https://netflix.com/superhd) you'd only get around ~5.6 Mbps (5800 kbps) 1080p video, and with 720p Netflix video the bitrate is only around ~3.5 Mbps (3600 kbps). If you have watched online streams like these, you'll probably know that they look quite decent. Now, if you look at the Beamr Video examples, you'll notice that even for their "reduced" clips, the bitrates are still around 9 Mbps *minimum*, and average as high as ~30 Mbps.

At this point, you can probably see the trick that Beamr is trying to pull off here: The bitrates they use are so high to begin with that even with the massively reduced filesize, the results still look very good. A classic case of [cheating on video encoder comparisons](http://x264dev.multimedia.cx/archives/472). This is, without a doubt, completely intentional on Beamr's part, as it allows them to make their impressive claims of *"up to 4x filesize reduction without losing quality"*. Change these values to something like 4 Mbps and 1 Mbps for 720p video, and suddenly the difference would be much more notable.

## The Encoding Technology

Beamr makes [impressive claims](http://beamrvideo.com/main/technology) about the technology they are using to encode videos, but there's one very important thing that they "forget" to mention: Their encoding is actually based on the free and open source [x264 video encoder](http://www.videolan.org/developers/x264.html). x264 is generally considered to be the best H.264 encoder in existence (and is used by [many notable entities](http://x264licensing.com/adopters)).

*"How can you tell that Beamr is using x264?"*, you might ask. It's pretty simple, actually. For one, if you look closely at the list of commercial x264 adopters, you'll notice **ICVT, Inc.**, the company behind Beamr, as one of the licensees (however, ICVT does not appear in the [MPEG-LA's list of AVC licensees](http://www.mpegla.com/main/programs/m2/pages/Licensees.aspx)!). We can further confirm this by opening any of their example clips in a hex editor. In the file header for the second clip (The Bourne Identity), you'll find this string:

>videomini ver. 89.15 options: cabac=1 ref=3 deblock=1:0:0 analyse=0x3:0x113 me=umh subme=8 psy=0 mixed_ref=1 me_range=16 chroma_me=1 trellis=1 8x8dct=1 cqm=0 deadzone=21,11 fast_pskip=1 chroma_qp_offset=0 threads=1 sliced_threads=0 nr=0 decimate=1 interlaced=0 bluray_compat=0 constrained_intra=0 bframes=0 weightp=2 keyint=250 keyint_min=25 scenecut=40 intra_refresh=0 rc=crf mbtree=0 crf=23.0 qcomp=0.60 qpmin=10 qpmax=51 qpstep=4 ip_ratio=1.40 aq=1:1.00

Now, let's encode the original clip they provide using the settings `x264 --bframes 0 --subme 8 --no-psy --no-mbtree` and observe the header outcome:

>x264 - core 129 r2245 bc13772 - H.264/MPEG-4 AVC codec - Copyleft 2003-2013 - http://www.videolan.org/x264.html - options: cabac=1 ref=3 deblock=1:0:0 analyse=0x3:0x113 me=hex subme=8 psy=0 mixed_ref=1 me_range=16 chroma_me=1 trellis=1 8x8dct=1 cqm=0 deadzone=21,11 fast_pskip=1 chroma_qp_offset=0 threads=12 lookahead_threads=2 sliced_threads=0 nr=0 decimate=1 interlaced=0 bluray_compat=0 constrained_intra=0 bframes=0 weightp=2 keyint=250 keyint_min=23 scenecut=40 intra_refresh=0 rc=crf mbtree=0 crf=23.0 qcomp=0.60 qpmin=0 qpmax=69 qpstep=4 ip_ratio=1.40 aq=1:1.00

As you can see, the only notable difference is that Beamr replaced the x264 name, version and copyright info with "videomini" and their own version number. Beyond the explicitly defined options, the only other option differences pretty much boils down to Beamr's x264 build being older than the one I used. No other H.264 encoder out there produces similar headers, so there is absolutely no doubt about Beamr using x264 to do their encodes.

## So Is Beamr Doing Anything Special?

So how does Beamr's encodes stack up to similar encodes done with x264? Let's find out. Turns out that encoding with the same CRF (you can think of this as a kind of "constant quality" encoding mode) value, vanilla x264 produces much smaller files. By lowering the CRF (the lower the CRF the higher the quality), however, we can approximately match the Beamr bitrate for all the clips. So, given the approximately same bitrate and settings, is there a difference? Let's take a look at some screen comparisons:

* [Clip 1 (Hanna) - Beamr vs x264 (Closely Matched Settings)](http://check2pic.ru/compare/26746/)
* [Clip 2 (The Bourne Identity) - Beamr vs x264 (Closely Matched Settings)](http://check2pic.ru/compare/26747/)
* [Clip 3 (The Bake Shop Ghost) - Beamr vs x264 (Closely Matched Settings)](http://check2pic.ru/compare/26748/)
* [Clip 4 (Safe House) - Beamr vs x264 (Closely Matched Settings)](http://check2pic.ru/compare/26749/)

>*(Note: Beamr's Safe House encode seems to be missing a frame in the beginning, which is why the framecounts differ by one.)*  
>You can download my encodes for this comparison from [here](http://blisswater.info/video/beamr/set1/), and the Beamr clips from their [website](http://beamrvideo.com). You can view detailed encoding settings for both after the post. I only tuned settings that Beamr must have specifically tuned themselves, leaving everything else to defaults. I wasn't really sure about the qpmin/qpmax, though (10/51 were the old defaults, but they could have set them manually as well), so I played it safe and used the same values.

As the results show, nothing special seems to be going on. Each pair of clips are practically identical - there are very minor differences here and there, but in action, you would notice absolutely no difference between the two versions of each clip. So, using the encoding settings found in Beamr's headers, the results can be reliably replicated with regular x264. This, in turn, allows us to do some further testing...

## How Beamr Fares in Realistic Situations

Let's take a look at [Beamr's technology page](http://beamrvideo.com/main/technology) again. At the bottom, you'll find a table that *"summarizes the typical bitrate reduction ratios achieved by Beamr Video for various types of content."* The example clips would fall into the Blu-ray disc category in their current form. If we look at how much the bitrate has been reduced in their examples, we get the following statistics:

* Clip 1 (Hanna) - ~76% reduction
* Clip 2 (The Bourne Identity) - ~49% reduction
* Clip 4 (Safe House) - ~40% reduction
* Clip 3 (The Bake Shop Ghost) - ~69% reduction

We can see that these percentages mostly fall into their "promised" 50 - 75% bitrate reduction for Blu-ray video. Now, with some math, we can transfer these percentages to the Streaming Services category, promising 20 - 40% bitrate reduction. We're going to use Netflix 1080p and 720p bitrates as our "original" values - 5.6 Mbps and 3.5 Mbps, respectively - and calculate our target bitrates based on them. We end up with the following:

* Clip 1 (Hanna) - ~40% reduction - 5.6 Mbps -> 3.4 Mbps
* Clip 2 (The Bourne Identity) - ~20% reduction - 5.6 Mbps -> 4.5 Mbps
* Clip 3 (The Bake Shop Ghost) - ~35% reduction - 3.5 mbps -> 2.3 Mbps
* Clip 4 (Safe House) - ~12% reduction - 5.6 Mbps -> 4.9 Mbps

Now, let's remember that Beamr claims that encodes done with their *"patent-pending video recompression technology"* should be able to match the visual quality of "regular" H.264 encodes at the original listed bitrates. Let's put that to the test, then! We're going to pit the magical *Beamr-optimized* x264 settings against generic high quality x264 settings (`x264 --preset veryslow --tune film --level 4.0`) and see what happens. And to give Beamr even a bigger edge, we're going to encode the comparison videos at the same reduced bitrate! Let's see how that turned out with these screen comparisons:

* [Clip 1 (Hanna) - Beamr-like settings vs HQ settings at ~3.4 Mbps](http://check2pic.ru/compare/26750/)
* [Clip 2 (The Bourne Identity) - Beamr-like settings vs HQ settings at ~4.5 Mbps](http://check2pic.ru/compare/26751/)
* [Clip 3 (The Bake Shop Ghost) - Beamr-like settings vs HQ settings at ~2.3 Mbps](http://check2pic.ru/compare/26752/)
* [Clip 4 (Safe House) - Beamr-like settings vs HQ settings at ~4.9 Mbps](http://check2pic.ru/compare/26753/)

>You can download all the encodes for this set [here](http://blisswater.info/video/beamr/set2/). For detailed encoding settings, see the stats at the bottom.

Oh dear! Looks like Beamr's settings weren't so magical after all. In every single comparison, the quality is worse than with the generic high quality settings *at the same bitrate*. This is not actually surprising at all, since Beamr's settings are basically normal, medium preset x264 settings with some mind-boggling tweaks, some of which make the quality even worse *(why on earth would you turn B-frames off if you're supposedly aiming for maximum quality compression?)*. All in all, their encoding settings look like someone's clueless first attempt at using x264 who thought that *Pro Encoding(TM)* is all about randomly tweaking complex-sounding options with no idea about their actual purpose and usage.

## Conclusion

When it comes to video encoding, it is highly unlikely that a single small company would have somehow managed to drastically increase compression efficiency of an existing format. This is the case here as well: Beamr has not actually created anything new, they're simply making wild claims based on the work of others - in this case the authors of x264.

In short, **Beamr Video is nothing but Snake Oil.** If you want to encode high quality H.264 video, you can either use x264 directly, or use any of the cloud encoding services out there that offer H.264 encoding with x264 (like [Zencoder](http://zencoder.com) for example).

#### Discuss this post on [Hacker News](http://news.ycombinator.com/item?id=5289532).

*You can take a look at the encode stats [here](https://gist.github.com/Daiz-/5245171).*