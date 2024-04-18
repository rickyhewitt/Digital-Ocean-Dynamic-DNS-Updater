Digital Ocean Dynamic DNS-Updater
=================================

This project is a continuation of bensquire/Digital-Ocean-Dynamic-DNS-Updater, which was a previous DigitalOcean Dynamic DNS script that is no longer maintained.

Differences:
--------
- Added (working) IPv6 support
- Removed PHP support

Purpose:
--------
Allows the dynamic updating of an 'A' or 'AAAA' record that is managed by Digital Ocean's DNS servers.

Usage:
------
Provide your API key, the domain you want to update and the 'Record' for that domain and schedule the script to run however
often you want (using, for example, the Windows Scheduler or a cron job).

E.g:

My home server has a sticky IP, I want to be able to connect to it remotely using:

    home.joebloggs.com

I'd create an 'A' record in DO with the hostname 'home', under the domain 'joebloggs.co.uk' and while I was there
retrieve my ['Personal Access Token'](https://cloud.digitalocean.com/settings/applications).


Example Usage:
--------------
The Python script has been designed to be called as a command line tool. Config is passed into it in the form of CLI parameters, for example:

    python updater.py accessToken domain record recordType

where 'accessToken' is your ['Personal Access Token'](https://cloud.digitalocean.com/settings/applications), 'domain' is the domain name you want to update (e.g: joebloggs.com), 'record' is the value of the record you want to update (e.g: home), and 'recordtype' is either A or AAAA.


Run Continuously / Cron Style:
-----------------------------

You can run this script continuously (every X number of seconds) by calling it:

    # If you wan to run it every 5 minutes
    python updater.py accessToken domain record recordType --run-every 300

This mode is perfect if you want to run inside a Docker container, where there is no cron by default.

    TOKEN={your token}
    DOMAIN={your domain}
    RECORD={your record}
    RTYPE=A
    TIMEOUT=60
    ARGS="$TOKEN $DOMAIN $RECORD $RTYPE --run-every $TIMEOUT"

    docker run \
      -it \
      --rm \
      --name do-ddns-updater \
      -v "$PWD":/usr/src/app \
      -w /usr/src/app \
      python:3 python updater.py $ARGS


Thanks to:
----------
@surfer190, @nickwest, @gnoeley, @ryanwcraig, @c17r, @larroy for adding additional features, testing code and feedback.
