---
layout: post
title: "Solved: File marked as permanent failure on server"
date: 2012-05-31 19:28
comments: true
categories: [music, software, howto]
---
Google Music has this annoying problem where it will sometimes barf on your MP3
files with the error message you see above. And it won't tell you what's wrong,
either. The internet doesn't know, either.

<!-- more -->
So I set out to take those unprocessed files and apply any change to them I
could think of, until one of them made the upload work. And the answer is
(drumroll): Bad VBR headers. I suspect these are created by bad MP3 encoders, in
my case by LAME3.99r.

To fix this, you can install Foobar2000 (still the best multi-format Swiss army
knife of a music player), and perform "right click on the file -> Utilities ->
Fix VBR MP3 header". After you rewrite the file, Music Manager will detect that
it has changed, find it to be no longer broken, and upload it.

You'll want to open up `%LOCALAPPDATA%\Google\MusicManager\FailureReport.txt` and
scan this for all the files that MusicManager complained about, then fix the MBR
headers on those. 


**Update**: A lot of comments have pointed out that this doesn't seem
to be a problem with the VBR headers, but that just the act of
changing the file in any way at all is what is doing the trick. That
sort of makes sense to me, but poses another question. If Google has
blacklisted some files based on a content checksum, what's the reason
for that?
