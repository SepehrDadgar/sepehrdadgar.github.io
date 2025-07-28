---
layout: project
title: subtitle translator
date: 2023-12-11
description: a simple script to translate srt files using the free web server of google translate using some tricks to circumvent the restrictions
tags:
  - python
  - subtitle
  - farsi_subtitle
  - translation
github: SepehrDadgar/free_srt_translator
---
# Subtitle Translator

I wanted to watch some Master Chef with my mom and since my mom doesn't speak English i had to find subtitles for it however no matter where i looked i couldn't find subtitles in Farsi. 
there was a translation for it made for it by the Gem group but it was no where to be downloaded.

 so I had an idea😂
 just turn the English `.srt` into plain text and put it into google translator
 it somehow worked very well
 but there were too many problems, the subtitles would merge together also because it turned line by line the translation would be off

so i thought there must be someone who has already made a python script that does this and there was however most of them needed a google cloud account
this was unacceptable for me because no 💵💵💵

turns out there is a free version of google translate API and its the API for the web version of the google translate but it is limited to 5000 requests and for movies or a 45 minute series like Master Chef if we separately send the requests for each line of dialogue it would be way over 5000.

so i just tricked the request limit by combining a number of lines together and when the translation came back i would separate them and it would work

I don't know about the morality of what I'm doing right now but doing this is free now 👍

