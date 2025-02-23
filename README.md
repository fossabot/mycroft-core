[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](LICENSE.md) [![CLA](https://img.shields.io/badge/CLA%3F-Required-blue.svg)](https://mycroft.ai/cla) [![Team](https://img.shields.io/badge/Team-Mycroft_Core-violetblue.svg)](https://github.com/MycroftAI/contributors/blob/master/team/Mycroft%20Core.md) ![Status](https://img.shields.io/badge/-Production_ready-green.svg)
[![FOSSA Status](https://app.fossa.io/api/projects/git%2Bgithub.com%2FMycroftAI%2Fmycroft-core.svg?type=shield)](https://app.fossa.io/projects/git%2Bgithub.com%2FMycroftAI%2Fmycroft-core?ref=badge_shield)

[![Build Status](https://travis-ci.org/MycroftAI/mycroft-core.svg?branch=master)](https://travis-ci.org/MycroftAI/mycroft-core) [![Coverage Status](https://coveralls.io/repos/github/MycroftAI/mycroft-core/badge.svg?branch=dev)](https://coveralls.io/github/MycroftAI/mycroft-core?branch=dev)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](http://makeapullrequest.com)
[![Join chat](https://img.shields.io/badge/Mattermost-join_chat-brightgreen.svg)](https://chat.mycroft.ai)

Mycroft
==========

Mycroft is a hackable open source voice assistant.

# Table of Contents

- [Table of Contents](#table-of-contents)
- [Getting Started](#getting-started)
- [Running Mycroft](#running-mycroft)
- [Using Mycroft](#using-mycroft)
  * [*Home* Device and Account Manager](#home-device-and-account-manager)
  * [Skills](#skills)
- [Behind the scenes](#behind-the-scenes)
  * [Pairing Information](#pairing-information)
  * [Configuration](#configuration)
  * [Using Mycroft Without Home](#using-mycroft-without-home)
  * [API Key Services](#api-key-services)
  * [Using Mycroft behind a proxy](#using-mycroft-behind-a-proxy)
    + [Using Mycroft behind a proxy without authentication](#using-mycroft-behind-a-proxy-without-authentication)
    + [Using Mycroft behind an authenticated proxy](#using-mycroft-behind-an-authenticated-proxy)
- [Getting Involved](#getting-involved)
- [Links](#links)

# Getting Started

First, get the code on your system!  The simplest method is via git ([git installation instructions](https://gist.github.com/derhuerst/1b15ff4652a867391f03)):
- `cd ~/`
- `git clone https://github.com/MycroftAI/mycroft-core.git`
- `cd mycroft-core`
- `bash dev_setup.sh`


This script sets up dependencies and a [virtualenv][about-virtualenv].  If running in an environment besides Ubuntu/Debian, Arch or Fedora you may need to manually install packages as instructed by dev_setup.sh.

[about-virtualenv]:https://virtualenv.pypa.io/en/stable/

NOTE: The default branch for this repository is 'dev', which should be considered a work-in-progress. If you want to clone a more stable version, switch over to the 'master' branch.

# Running Mycroft

Mycroft provides `start-mycroft.sh` to perform common tasks. This script uses a virtualenv created by `dev_setup.sh`.  Assuming you installed mycroft-core in your home directory run:
- `cd ~/mycroft-core`
- `./start-mycroft.sh debug`

The "debug" command will start the background services (microphone listener, skill, messagebus, and audio subsystems) as well as bringing up a text-based Command Line Interface (CLI) you can use to interact with Mycroft and see the contents of the various logs. Alternatively you can run `./start-mycroft.sh all` to begin the services without the command line interface.  Later you can bring up the CLI using `./start-mycroft.sh cli`.

The background services can be stopped as a group with:
- `./stop-mycroft.sh`

# Using Mycroft

## *Home* Device and Account Manager
Mycroft AI, Inc. maintains a device and account management system known as Mycroft Home. Developers may sign up at: https://home.mycroft.ai

By default, mycroft-core  is configured to use Home. By saying "Hey Mycroft, pair my device" (or any other request verbal request) you will be informed that your device needs to be paired. Mycroft will speak a 6-digit code which you can entered into the pairing page within the [Mycroft Home site](https://home.mycroft.ai).

Once paired, your unit will use Mycroft API keys for services such as Speech-to-Text (STT), weather and various other skills.

## Skills

Mycroft is nothing without skills.  There are a handful of default skills that are downloaded automatically to your `/opt/mycroft/skills` directory, but most need to be installed explicitly.  See the [Skill Repo](https://github.com/MycroftAI/mycroft-skills#welcome) to discover skills made by others.  And please share your own interesting work!

# Behind the scenes

## Pairing Information
Pairing information generated by registering with Home is stored in:
`~/.mycroft/identity/identity2.json` <b><-- DO NOT SHARE THIS WITH OTHERS!</b>

## Configuration
Mycroft configuration consists of 4 possible locations:
- `mycroft-core/mycroft/configuration/mycroft.conf`(Defaults)
- [Mycroft Home](https://home.mycroft.ai) (Remote)
- `/etc/mycroft/mycroft.conf`(Machine)
- `$HOME/.mycroft/mycroft.conf`(User)

When the configuration loader starts, it looks in these locations in this order, and loads ALL configurations. Keys that exist in multiple configuration files will be overridden by the last file to contain the value. This process results in a minimal amount being written for a specific device and user, without modifying default distribution files.

## Using Mycroft Without Home

If you do not wish to use the Mycroft Home service, you may insert your own API keys into the configuration files listed below in <b>configuration</b>.

The place to insert the API key looks like the following:

`[WeatherSkill]`
`api_key = ""`

Put a relevant key inside the quotes and mycroft-core should begin to use the key immediately.

## API Key Services

These are the keys currently used in Mycroft Core:

- [STT API, Google STT, Google Cloud Speech](http://www.chromium.org/developers/how-tos/api-keys)
- [Weather Skill API, OpenWeatherMap](http://openweathermap.org/api)
- [Wolfram-Alpha Skill](http://products.wolframalpha.com/api/)

## Using Mycroft behind a proxy

Many schools, universities and workplaces run a `proxy` on their network. If you need to type in a username and password to access the external internet, then you are likely behind a `proxy`.

If you plan to use Mycroft behind a proxy, then you will need to do an additional configuration step.

_NOTE: In order to complete this step, you will need to know the `hostname` and `port` for the proxy server. Your network administrator will be able to provide these details. Your network administrator may want information on what type of traffic Mycroft will be using. We use `https` traffic on port `443`, primarily for accessing ReST-based APIs._

### Using Mycroft behind a proxy without authentication

If you are using Mycroft behind a proxy without authentication, add the following environment variables, changing the `proxy_hostname.com` and `proxy_port` for the values for your network. These commands are executed from the Linux command line interface (CLI).

```bash
$ export http_proxy=http://proxy_hostname.com:proxy_port
$ export https_port=http://proxy_hostname.com:proxy_port
$ export no_proxy="localhost,127.0.0.1,localaddress,.localdomain.com,0.0.0.0,::1"
```

### Using Mycroft behind an authenticated proxy

If  you are behind a proxy which requires authentication, add the following environment variables, changing the `proxy_hostname.com` and `proxy_port` for the values for your network. These commands are executed from the Linux command line interface (CLI).

```bash
$ export http_proxy=http://user:password@proxy_hostname.com:proxy_port
$ export https_port=http://user:password@proxy_hostname.com:proxy_port
$ export no_proxy="localhost,127.0.0.1,localaddress,.localdomain.com,0.0.0.0,::1"
```

# Getting Involved

This is an open source project and we would love your help. We have prepared a [contributing](.github/CONTRIBUTING.md) guide to help you get started.

If this is your first PR or you're not sure where to get started,
say hi in [Mycroft Chat](https://chat.mycroft.ai/) and a team member would be happy to mentor you.
Join the [Mycroft Forum](https://community.mycroft.ai/) for questions and answers.

# Links
* [Creating a Skill](https://docs.mycroft.ai/skill.creation)
* [Documentation](https://docs.mycroft.ai)
* [Skill Writer API Docs](https://mycroft-core.readthedocs.io/en/master/)
* [Release Notes](https://github.com/MycroftAI/mycroft-core/releases)
* [Mycroft Chat](https://chat.mycroft.ai)
* [Mycroft Forum](https://community.mycroft.ai)
* [Mycroft Blog](https://mycroft.ai/blog)


## License
[![FOSSA Status](https://app.fossa.io/api/projects/git%2Bgithub.com%2FMycroftAI%2Fmycroft-core.svg?type=large)](https://app.fossa.io/projects/git%2Bgithub.com%2FMycroftAI%2Fmycroft-core?ref=badge_large)