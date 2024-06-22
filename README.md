# alpine-pkg-glibc

[![CircleCI](https://circleci.com/gh/sgerrand/alpine-pkg-glibc/tree/main.svg?style=svg)](https://circleci.com/gh/sgerrand/alpine-pkg-glibc/tree/main) ![x86_64](https://img.shields.io/badge/x86__64-supported-brightgreen.svg)

This is the [GNU C Library](https://gnu.org/software/libc/) as a Alpine Linux package to run binaries linked against `glibc`. This package utilizes a custom built glibc binary based on the vanilla glibc source. Built binary artifacts come from https://github.com/sgerrand/docker-glibc-builder.

## Releases

See the [releases page](https://github.com/sgerrand/alpine-pkg-glibc/releases) for the latest download links. If you are using tools like `localedef` you will need the `glibc-bin` and `glibc-i18n` packages in addition to the `glibc` package.

## Installing

The current installation method for these packages is to pull them in using `wget` or `curl` and install the local file with `apk`:

    wget -q -O /etc/apk/keys/sgerrand.rsa.pub https://alpine-pkgs.sgerrand.com/sgerrand.rsa.pub
    wget https://github.com/sgerrand/alpine-pkg-glibc/releases/download/2.35-r1/glibc-2.35-r1.apk
    apk add glibc-2.35-r1.apk

### Please Note

:warning: The URL of the public signing key has changed! :warning:

Any previous reference to `https://raw.githubusercontent.com/sgerrand/alpine-pkg-glibc/master/sgerrand.rsa.pub` should be updated with immediate effect to `https://alpine-pkgs.sgerrand.com/sgerrand.rsa.pub`.

## Locales

You will need to generate your locale if you would like to use a specific one for your glibc application. You can do this by installing the `glibc-i18n`
package and generating a locale using the `localedef` binary. An example for en_US.UTF-8 would be:

    wget https://github.com/sgerrand/alpine-pkg-glibc/releases/download/2.35-r1/glibc-bin-2.35-r1.apk
    wget https://github.com/sgerrand/alpine-pkg-glibc/releases/download/2.35-r1/glibc-i18n-2.35-r1.apk
    apk add glibc-bin-2.35-r1.apk glibc-i18n-2.35-r1.apk
    /usr/glibc-compat/bin/localedef -i en_US -f UTF-8 en_US.UTF-8

## musl - current state
https://wiki.musl-libc.org/open-issues
Locale limitations
Locale support is very limited, and barely works. Translation of LC_TIME is not possible because the key strings for ABMON_5 and MON_5 (“May”) are identical. Custom collation orders (LC_COLLATE) are not implemented at all, despite there always having been an intent to support them. LC_NUMERIC and LC_MONETARY also admit no variation by locale. Solving these problems requires a major overhaul, but the main missing prerequisite is involvement from users who want the functionality.

Building officially recognized locales for musl is also a recognized open problem. The bulk of the data should be derived mechanically from the Unicode CLDR where possible, but the CLDR seems to lack certain time format variants corresponding to the ones C/POSIX needs for nl_langinfo/strftime. This probably requires a great deal of manual work to remedy, ideally getting the missing formats added upstream with Unicode.

    
