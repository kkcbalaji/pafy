Pafy Documentation
******************
.. module:: Pafy

This is the documentation for Pafy - Python API for YouTube

A quick start intro with usage examples is available in the `README <http://github.com/np1/pafy/blob/master/README.rst>`_

Development / Source code / Bug reporting: `github.com/np1/pafy
<https://github.com/np1/pafy/>`_

Homepage: `np1.github.io/pafy <http://np1.github.io/pafy/>`_

Pafy Objects and Stream Objects
===============================

Pafy objects relate to videos hosted on YouTube.  They hold metadata such as
*title*, *viewcount*, *author* and *video ID*

Stream objects relate to individual streams of a YouTube video. They hold
stream-specific data such as *resolution*, *bitrate* and *url*.  Each Pafy
object contains multiple stream objects.


Pafy Objects
============

.. autoclass:: pafy.Pafy

Create a Pafy object using the *pafy.new()* function, giving a YouTube video URL as
the argument


.. function:: new(video_url)

    Create a new Pafy object

    :param url: The YouTube url or 11 character video id of the video
    :rtype: Pafy object

Example::

    import pafy
    myvid = pafy.new("http://www.youtube.com/watch?v=dQw4w9WgXc")

Pafy Attributes
---------------

Once you have created a Pafy object using :func:`pafy.new`, several data
attributes are available

.. attribute:: Pafy.author

    The author of the video (*str*)

.. attribute:: Pafy.bigthumb (*str*)

    The url of the video's display image (not always available)

.. attribute:: Pafy.bigthumbhd

    The url of the video's larger display image (not always available) (*str*)

.. attribute:: Pafy.category

    The category of the video (*str*)

.. attribute:: Pafy.description

    The video description text (*str*)

.. attribute:: Pafy.duration

    The duration of the stream (*string formatted as HH:MM:SS*)

.. attribute:: Pafy.keywords

    A list of the video's keywords (not always available) (*[str]*)

.. attribute:: Pafy.length

    The duration of the streams in seconds (*int*)

.. attribute:: Pafy.rating

    The rating of the video (0-5), (*float*)

.. attribute:: Pafy.thumb

    The url of the video's thumbnail image (*str*)

.. attribute:: Pafy.title

    The title of the video (*str*)

.. attribute:: Pafy.videoid

    The 11-character video id (*str*)

.. attribute:: Pafy.viewcount

    The viewcount of the video (*int*)

An example of accessing this video metadata is shown below::

    import pafy
    v = pafy.new("dQw4w9WgXcQ")
    print(v.title)
    print(v.duration)
    print(v.rating)
    print(v.author)
    print(v.length)
    print(v.keywords)
    print(v.thumb)
    print(v.videoid)
    print(v.viewcount)

Which will result in this output::

    Rick Astley - Never Gonna Give You Up
    00:03:33
    4.75177729422
    RickAstleyVEVO
    213
    ['Rick', 'Astley', 'Sony', 'BMG', 'Music', 'UK', 'Pop']
    https://i1.ytimg.com/vi/dQw4w9WgXcQ/default.jpg
    dQw4w9WgXcQ
    69788014

Pafy Methods
------------

The :func:`Pafy.getbest` and :func:`Pafy.getbestaudio` methods are a quick
way to access the highest quality streams for a particular video without
needing to query the stream lists. 

.. function:: Pafy.getbest([preftype="any"][, ftypestrict=True])

    Selects the stream with the highest resolution.  This will return a
    "normal" stream (ie. one with video and audio)

    :param preftype: Preferred type, set to *mp4*, *webm*, *flv*, *3gp* or *any*
    :type preftype: str
    :param ftypestrict: Set to *False* to return a type other than that specified in preftype if it has a higher resolution
    :type ftypestrict: boolean
    :rtype: :class:`pafy.Stream`


.. function:: Pafy.getbestaudio([preftype="any"][, ftypestrict=True])

    Selects the audio stream with the highest bitrate.

    :param preftype: Preferred type, set to *ogg* or *m4a* or *any*
    :type preftype: str
    :param ftypestrict: Set to *False* to return a type other than that specified in preftype if that has the highest bitrate
    :type ftypestrict: boolean
    :rtype: :class:`pafy.Stream`


Stream Lists
------------

A Pafy object provides multiple stream lists.  These are:

.. attribute:: Pafy.streams

    A list of regular streams (streams containing both audio and video)

.. attribute:: Pafy.audiostreams

    A list of audio-only streams (aac streams (.m4a) and ogg vorbis streams (.ogg))

.. attribute:: Pafy.videostreams

    A list of video-only streams (Note: these streams have no audio data)

.. attribute:: Pafy.oggstreams

    A list of ogg vorbis encoded audio streams

.. attribute:: Pafy.m4astreams

    A list of aac encoded audio streams

.. attribute:: Pafy.allstreams

    A list of all available streams


An example of accessing stream lists::

    >>> import pafy
    >>> v = pafy.new("cyMHZVT91Dw")
    >>> v.audiostreams
    [audio:m4a@48k, audio:m4a@128k, audio:m4a@256k]
    >>> v.streams
    [normal:webm@640x360, normal:mp4@640x360, normal:flv@320x240, normal:3gp@320x240, normal:3gp@176x144]
    >>> v.allstreams
    [normal:webm@640x360, normal:mp4@640x360, normal:flv@320x240, normal:3gp@320x240, normal:3gp@176x144, video:m4v@854x480, video:m4v@640x360, video:m4v@426x240, video:m4v@256x144, audio:m4a@48k, audio:m4a@128k, audio:m4a@256k]
    

Stream Objects
==============

.. class:: pafy.Stream

After you have created a :class:`Pafy` object using :func:`new`, you
can then access the streams using one of the `Stream Lists`_, or by calling
:func:`Pafy.getbest` or :func:`Pafy.getbestaudio` on the object.


Stream Attributes
-----------------

    A Stream object can be used to access the following attributes


.. attribute:: Stream.url

    The direct access URL of the stream.  This can be used to stream the media
    in mplayer or vlc, or for downloading with wget or curl.  To download
    directly, use the :func:`Stream.download` method

.. attribute:: Stream.bitrate

    The bitrate of the stream - if it is an audio stream, otherwise None,
    This is a string of the form *"192k"*. 

.. attribute:: Stream.dimensions

    A 2-tuple (x, y) representing the resolution of a video stream.

.. attribute:: Stream.extension

    The format of the stream, will be one of: ``'ogg'``, ``'m4a'``, ``'mp4'``,
    ``'flv'``, ``'webm'``, ``'3gp'``

.. attribute:: Stream.mediatype

    A string attribute that is ``'normal'``, ``'audio'`` or ``'video'``, 
    depending on the content of the stream

.. attribute:: Stream.quality

    The resolution or the bitrate of the stream, depending on whether the
    stream is video or audio respectively

.. attribute:: Stream.resolution

    The resolution of a video as a string, eg: "820x640".  Note if the stream
    is 3D this will be appended; eg: "820x640-3D".  

    For audio streams, this will be set to "0x0"

.. attribute:: Stream.rawbitrate

    The bitrate of an audio stream, *int*
    
    For video streams, this will be set to *None*

.. attribute:: Stream.threed

    Whether the stream is a 3D video (*boolean*)

.. attribute:: Stream.title

    The title of the video, this will be the same as :attr:`Pafy.title`

.. attribute:: Stream.notes

    Any additional notes regarding the stream (eg, 6-channel surround) *str*
   

An example of accessing Stream attributes::

    >>> import pafy
    >>> v = pafy.new("cyMHZVT91Dw")
    >>> v.audiostreams
    [audio:m4a@48k, audio:m4a@128k, audio:m4a@256k]
    >>> mystream = v.audiostreams[2]
    >>> mystream.rawbitrate
    255940
    >>> mystream.bitrate
    '256k'
    >>> mystream.url
    'http://r20---sn-aigllnes.c.youtube.com/videoplayback?ipbits=8&clen=1130...


Stream Methods
--------------

.. function:: Stream.get_filesize()     

    Returns the filesize of a stream

.. function:: Stream.download([filepath=""][, quiet=False][, callback=None])

    Downloads the stream object

    :param filepath: The filepath to use to save the stream, defaults to *title.extension* if ommitted
    :type filepath: string
    :param quiet: Whether to supress output of the download progress
    :type quiet: boolean
    :param callback: Call back function to use for receiving download progress
    :type callback: function or None
    
    If a callback function is provided, it will be called repeatedly for each
    chunk downloaded.  It must be a function that takes five arguments. These
    are:

    - total bytes in stream, *int*
    - total bytes downloaded, *int*
    - ratio downloaded (0-1), *float*
    - download rate (kbps), *float*
    - ETA in seconds, *float*

Download example::

    import pafy
    v = pafy.new("cyMHZVT91Dw")
    s = v.getbest()
    print("Size is %s" % s.get_filesize())
    s.download()

Will download the file to the current working directory with the filename
*title.extension* (eg. "cute kittens.mp4") and output the following progress statistics::

    Size is 34775366
    1,015,808 Bytes [2.92%] received. Rate: [ 640 kbps].  ETA: [51 secs] 

Download using *callback* example::

    import pafy

    # callback function, this callback simply prints the bytes received,
    # ratio downloaded and eta.
    def mycb(total, recvd, ratio, rate, eta):
        print(recvd, ratio, eta)

    p = pafy.new("cyMHZVT91Dw")
    ba = p.getbestaudio()
    ba.download(quiet=True, callback=mycb)

The output of this will appear as follows, while the file is downloading::

    (16384, 0.001449549245392125, 20.05230682669207)
    (32768, 0.00289909849078425, 16.88200659636641)
    (49152, 0.004348647736176375, 15.196503182407469)
    (65536, 0.0057981969815685, 14.946467230009146)
    (81920, 0.007247746226960625, 15.066431667096913)
    (98304, 0.00869729547235275, 14.978577915171627)
    (114688, 0.010146844717744874, 14.529802172976945)
    (131072, 0.011596393963137, 14.31917945870373)
    ...
    
