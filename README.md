# Nagios Radio Plugins

Plugins Nagios pour monitorer des flux radio (Icecast/Shoutcast) :
- check_stream_up : vérifie la disponibilité du flux (HTTP 200 + Content-Type audio)
- check_stream_silence : détecte le silence via ffmpeg/volumedetect sur une fenêtre donnée

## Dépendances
- ffmpeg (requis)

## Installation
- Copier les scripts dans votre répertoire de plugins personnalisés (ex: /opt/Custom-Nagios-Plugins/radio)
- Rendre exécutable: `chmod +x radio/check_stream_*`

## Commandes Nagios (exemples)
```nagios
define command{
        command_name    check_stream_up
        command_line    $USER2$/radio/check_stream_up $ARG1$
}

define command{
        command_name    check_stream_silence
        command_line    $USER2$/radio/check_stream_silence $ARG1$ $ARG2$ $ARG3$
}
```

## Services Nagios (exemples)
```nagios
define host{
        use       linux-server
        host_name cibo-fm
        alias     CIBO FM Icecast
        address   stream.statsradio.com
}

define service{
        use                 local-service
        host_name           cibo-fm
        service_description Radio Stream Up
        check_command       check_stream_up!https://stream.statsradio.com:8118/stream
}

define service{
        use                 local-service
        host_name           cibo-fm
        service_description Radio Silence Detector
        check_command       check_stream_silence!https://stream.statsradio.com:8118/stream!-45!10
}
```

## Notes
- Pour Icecast, la page status HTML du mount peut être consultée: `https://stream.statsradio.com:8118/status.xsl?mount=/stream`.
- Pour HLS, `check_stream_up` reconnait `application/vnd.apple.mpegurl`.

## Contact
- Email: gauclair@sarius.ca

## License
Apache-2.0
