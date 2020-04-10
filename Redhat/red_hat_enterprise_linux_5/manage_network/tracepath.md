### tracepath {#tracepath}

NAME

      tracepath, tracepath6 - traces path to a network host discovering MTU along this path

SYNOPSIS

      tracepath [-nc] &lt;destination&gt;[/&lt;port&gt;]

DESCRIPTION

      It  traces path to destination discovering MTU along this path.  It uses UDP port port or some random port.  It is similar to traceroute, only does not not require superuser privileges.