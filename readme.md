# OpenWrt for TP-Link TL-MR6400 V4

OpenWrt for TP-Link TL-MR6400 V4 with ModemManager included.


```
docker run -ti --platform linux/amd64 ubuntu bash

apt update

apt install -y build-essential ccache ecj fastjar file g++ gawk gettext git java-propose-classpath libelf-dev libncurses5-dev libncursesw5-dev libssl-dev python3-dev python3 unzip wget  python3-setuptools rsync subversion swig time xsltproc zlib1g-dev build-essential

wget https://downloads.openwrt.org/snapshots/targets/ramips/mt76x8/openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64.tar.zst

tar --zstd -xvf openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64.tar.zst

cd openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64

make image PROFILE="tplink_tl-mr6400-v4" PACKAGES="modemmanager"
```

```
cd /openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8

tplink_tl-mr6400-v4-kernel.bin
tplink_tl-mr6400-v5-kernel.bin
```

```

TP-Link TL-MR6400 OpenWrt
The TP-Link TL-MR6400 v4 router can be flashed with OpenWrt firmware to enhance its functionality. Here are some key points to consider:

Flashing Process: To flash OpenWrt on the TP-Link TL-MR6400 v4, you can use the TFTP recovery method. Set up your PC as a TFTP server with the IP address 192.168.0.225, and place the firmware file in the TFTP directory as tp_recovery.bin. Power on the router while holding the reset button for 8 seconds to initiate the TFTP recovery process.
Firmware Download: Download the latest OpenWrt firmware for the TP-Link TL-MR6400 v4 from the official OpenWrt website. Ensure you select the correct firmware variant that supports TFTP recovery.
Network Interfaces: After flashing, the default configuration includes:
br-lan: LAN & WiFi, default IP 192.168.1.1/24
vlan0: LAN ports, no default configuration
vlan1: LAN/WAN port, default configuration is DHCP
wlan0: WiFi, disabled by default
wwan0: Broadband, not configured by default
LTE Connection: To configure the LTE connection, you need to disable DHCP for the wwan interface and set it to use QMI protocol. The configuration file /etc/config/network should include:
config interface 'wwan'
        option proto 'qmi'
        option device '/dev/cdc-wdm0'
        option apn 'internet'
        option pdptype 'ip'
        option dhcp '0'

uqmi Package: Ensure you have the latest uqmi package installed, as it is required for LTE modem functionality. You can download it from a GitHub repository if it is not available in the official OpenWrt repository.
LED Configuration: You can configure the LEDs to indicate different statuses, such as power, internet connection, WiFi connection, LAN connection, and signal strength. This can be done by editing the /sys/class/leds/ directory.
Cron Job: To address the LTE disconnect issue, you can add a cron job to restart the network interfaces if the LTE connection is lost. The cron job should look like:
* * * * * [ "$(uqmi -d /dev/cdc-wdm0 --get-current-settings | grep -oFm1 "Out of cal

```





LOG

```
docker run -ti --platform linux/amd64 ubuntu bash
Unable to find image 'ubuntu:latest' locally
latest: Pulling from library/ubuntu
de44b265507a: Pull complete
Digest: sha256:80dd3c3b9c6cecb9f1667e9290b3bc61b78c2678c02cbdae5f0fea92cc6734ab
Status: Downloaded newer image for ubuntu:latest
root@26ba92ad893d:/# apt update
Get:1 http://security.ubuntu.com/ubuntu noble-security InRelease [126 kB]
Get:2 http://archive.ubuntu.com/ubuntu noble InRelease [256 kB]
Get:3 http://archive.ubuntu.com/ubuntu noble-updates InRelease [126 kB]
Get:4 http://archive.ubuntu.com/ubuntu noble-backports InRelease [126 kB]
Get:5 http://security.ubuntu.com/ubuntu noble-security/multiverse amd64 Packages [15.5 kB]
Get:6 http://security.ubuntu.com/ubuntu noble-security/universe amd64 Packages [1035 kB]
Get:7 http://security.ubuntu.com/ubuntu noble-security/main amd64 Packages [740 kB]
Get:8 http://archive.ubuntu.com/ubuntu noble/multiverse amd64 Packages [331 kB]
Get:9 http://security.ubuntu.com/ubuntu noble-security/restricted amd64 Packages [724 kB]
Get:10 http://archive.ubuntu.com/ubuntu noble/main amd64 Packages [1808 kB]
Get:11 http://archive.ubuntu.com/ubuntu noble/universe amd64 Packages [19.3 MB]
Get:12 http://archive.ubuntu.com/ubuntu noble/restricted amd64 Packages [117 kB]
Get:13 http://archive.ubuntu.com/ubuntu noble-updates/restricted amd64 Packages [739 kB]
Get:14 http://archive.ubuntu.com/ubuntu noble-updates/multiverse amd64 Packages [19.7 kB]
Get:15 http://archive.ubuntu.com/ubuntu noble-updates/universe amd64 Packages [1269 kB]
Get:16 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 Packages [995 kB]
Get:17 http://archive.ubuntu.com/ubuntu noble-backports/universe amd64 Packages [15.1 kB]
Fetched 27.8 MB in 7s (3854 kB/s)
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
7 packages can be upgraded. Run 'apt list --upgradable' to see them.
root@26ba92ad893d:/# ^Ct install -y build-essential ccache ecj fastjar file g++ gawk gettext git java-propose-classpath libelf-dev libncurses5-dev libncursesw5-dev libssl-dev python3-dev python3 unzip wget  python3-setuptools rsync subversion swig time xsltproc zlib1g-dev
 build
root@26ba92ad893d:/# apt install -y build-essential ccache ecj fastjar file g++ gawk gettext git java-propose-classpath libelf-dev libncurses5-dev libncursesw5-dev libssl-dev python3-dev python3 unzip wget  python3-setuptools rsync subversion swig time xsltproc zlib1g-dev build-essential
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
Note, selecting 'libncurses-dev' instead of 'libncurses5-dev'
Note, selecting 'libncurses-dev' instead of 'libncursesw5-dev'
The following additional packages will be installed:
  adduser alsa-topology-conf alsa-ucm-conf autoconf automake autopoint autotools-dev binfmt-support binutils binutils-common binutils-x86-64-linux-gnu bsdextrautils bsdutils bzip2 ca-certificates ca-certificates-java
  cpp cpp-13 cpp-13-x86-64-linux-gnu cpp-x86-64-linux-gnu dbus dbus-bin dbus-daemon dbus-session-bus-common dbus-system-bus-common dctrl-tools debhelper debugedit default-jre-headless devscripts dh-autoreconf
  dh-strip-nondeterminism diffstat dirmngr distro-info-data dpkg-dev dput dwz fakeroot fontconfig-config fonts-dejavu-core fonts-dejavu-mono g++-13 g++-13-x86-64-linux-gnu g++-x86-64-linux-gnu gcc gcc-13 gcc-13-base
  gcc-13-x86-64-linux-gnu gcc-x86-64-linux-gnu gettext-base git-man gnupg gnupg-l10n gnupg-utils gpg gpg-agent gpg-wks-client gpgconf gpgsm groff-base intltool-debian iso-codes jarwrapper java-common java-wrappers
  javahelper javascript-common keyboxd krb5-locales less libalgorithm-diff-perl libalgorithm-diff-xs-perl libalgorithm-merge-perl libaliased-perl libaom3 libapparmor1 libapr1t64 libaprutil1t64 libapt-pkg-perl
  libarchive-cpio-perl libarchive-zip-perl libarray-intspan-perl libasan8 libasound2-data libasound2t64 libatomic1 libauthen-sasl-perl libavahi-client3 libavahi-common-data libavahi-common3 libb-hooks-endofscope-perl
  libb-hooks-op-check-perl libberkeleydb-perl libbinutils libblkid1 libbrotli1 libbsd0 libc-dev-bin libc-devtools libc6-dev libcapture-tiny-perl libcbor0.10 libcc1-0 libcgi-fast-perl libcgi-pm-perl
  libclass-data-inheritable-perl libclass-method-modifiers-perl libclass-xsaccessor-perl libclone-perl libcommon-sense-perl libconfig-tiny-perl libconst-fast-perl libcpanel-json-xs-perl libcrypt-dev libctf-nobfd0
  libctf0 libcups2t64 libcurl3t64-gnutls libdata-dpath-perl libdata-dump-perl libdata-messagepack-perl libdata-optlist-perl libdata-validate-domain-perl libdata-validate-ip-perl libdata-validate-uri-perl libdbus-1-3
  libde265-0 libdebhelper-perl libdeflate0 libdevel-callchecker-perl libdevel-size-perl libdevel-stacktrace-perl libdistro-info-perl libdpkg-perl libdw1t64 libdynaloader-functions-perl libeclipse-jdt-core-java libedit2
  libelf1t64 libemail-address-xs-perl libencode-locale-perl liberror-perl libexception-class-perl libexpat1 libexpat1-dev libexporter-tiny-perl libfakeroot libfcgi-bin libfcgi-perl libfcgi0t64 libfido2-1
  libfile-basedir-perl libfile-chdir-perl libfile-dirlist-perl libfile-fcntllock-perl libfile-find-rule-perl libfile-homedir-perl libfile-listing-perl libfile-stripnondeterminism-perl libfile-touch-perl
  libfile-which-perl libfont-afm-perl libfont-ttf-perl libfontconfig1 libfreetype6 libfreezethaw-perl libgcc-13-dev libgd3 libgdbm-compat4t64 libgdbm6t64 libgit-wrapper-perl libglib2.0-0t64 libglib2.0-data libgomp1
  libgpgme11t64 libgpm2 libgprofng0 libgraphite2-3 libgssapi-krb5-2 libharfbuzz0b libheif-plugin-aomdec libheif-plugin-aomenc libheif-plugin-libde265 libheif1 libhiredis1.1.0 libhtml-form-perl libhtml-format-perl
  libhtml-html5-entities-perl libhtml-parser-perl libhtml-tagset-perl libhtml-tokeparser-simple-perl libhtml-tree-perl libhttp-cookies-perl libhttp-daemon-perl libhttp-date-perl libhttp-message-perl
  libhttp-negotiate-perl libhwasan0 libicu74 libimport-into-perl libindirect-perl libio-html-perl libio-interactive-perl libio-pty-perl libio-socket-ssl-perl libio-string-perl libipc-run-perl libipc-run3-perl
  libipc-system-simple-perl libisl23 libiterator-perl libiterator-util-perl libitm1 libjansson4 libjbig0 libjpeg-turbo8 libjpeg8 libjs-jquery libjs-sphinxdoc libjs-underscore libjson-maybexs-perl libjson-perl
  libjson-xs-perl libk5crypto3 libkeyutils1 libkrb5-3 libkrb5support0 libksba8 liblcms2-2 libldap-common libldap2 liblerc4 liblist-compare-perl liblist-someutils-perl liblist-someutils-xs-perl liblist-utilsby-perl
  liblocale-gettext-perl liblog-any-adapter-screen-perl liblog-any-perl liblsan0 libltdl-dev libltdl7 liblwp-mediatypes-perl liblwp-protocol-https-perl liblzo2-2 libmagic-mgc libmagic1t64 libmail-sendmail-perl
  libmailtools-perl libmarkdown2 libmath-base85-perl libmldbm-perl libmodule-implementation-perl libmodule-runtime-perl libmoo-perl libmoox-aliases-perl libmount1 libmouse-perl libmpc3 libmpfr6 libnamespace-clean-perl
  libncurses6 libnet-domain-tld-perl libnet-http-perl libnet-ipv6addr-perl libnet-netmask-perl libnet-smtp-ssl-perl libnet-ssleay-perl libnetaddr-ip-perl libnghttp2-14 libnspr4 libnss3 libnumber-compare-perl
  libobject-pad-perl libpackage-stash-perl libpackage-stash-xs-perl libparams-classify-perl libparams-util-perl libpath-iterator-rule-perl libpath-tiny-perl libpcsclite1 libperl5.38t64 libperlio-gzip-perl
  libperlio-utf8-strict-perl libpipeline1 libpng16-16t64 libpod-constants-perl libpod-parser-perl libpopt0 libproc-processtable-perl libpsl5t64 libpython3-dev libpython3-stdlib libpython3.12-dev libpython3.12-minimal
  libpython3.12-stdlib libpython3.12t64 libquadmath0 libre-engine-re2-perl libre2-10 libreadline8t64 libregexp-pattern-license-perl libregexp-pattern-perl libregexp-wildcards-perl librole-tiny-perl librtmp1 libsasl2-2
  libsasl2-modules libsasl2-modules-db libsereal-decoder-perl libsereal-encoder-perl libserf-1-1 libset-intspan-perl libsframe1 libsharpyuv0 libsigsegv2 libsmartcols1 libsocket6-perl libsodium23 libsort-versions-perl
  libsqlite3-0 libssh-4 libstdc++-13-dev libstrictures-perl libstring-copyright-perl libstring-escape-perl libstring-license-perl libstring-shellquote-perl libsub-exporter-perl libsub-exporter-progressive-perl
  libsub-identify-perl libsub-install-perl libsub-name-perl libsub-override-perl libsub-quote-perl libsvn1 libsyntax-keyword-try-perl libsys-hostname-long-perl libterm-readkey-perl libtext-glob-perl
  libtext-levenshteinxs-perl libtext-markdown-discount-perl libtext-xslate-perl libtiff6 libtime-duration-perl libtime-moment-perl libtimedate-perl libtool libtry-tiny-perl libtsan2 libtypes-serialiser-perl libubsan1
  libuchardet0 libunicode-utf8-perl libunwind8 liburi-perl libutf8proc3 libuuid1 libvariable-magic-perl libwebp7 libwww-mechanize-perl libwww-perl libwww-robotrules-perl libx11-6 libx11-data libxau6 libxcb1 libxdmcp6
  libxext6 libxml-libxml-perl libxml-namespacesupport-perl libxml-parser-perl libxml-sax-base-perl libxml-sax-expat-perl libxml-sax-perl libxml2 libxmuu1 libxpm4 libxs-parse-keyword-perl libxs-parse-sublike-perl
  libxslt1.1 libyaml-0-2 libyaml-libyaml-perl libzstd-dev licensecheck lintian linux-libc-dev lsb-release lto-disabled-list lzip lzop m4 make man-db manpages manpages-dev media-types mount netbase
  openjdk-21-jre-headless openssh-client openssl patch patchutils perl perl-modules-5.38 perl-openssl-defaults pinentry-curses po-debconf publicsuffix python-apt-common python3-apt python3-bcrypt python3-certifi
  python3-cffi-backend python3-chardet python3-cryptography python3-debian python3-gpg python3-idna python3-magic python3-minimal python3-nacl python3-paramiko python3-pkg-resources python3-requests python3-six
  python3-unidiff python3-urllib3 python3-xdg python3.12 python3.12-dev python3.12-minimal readline-common rpcsvc-proto shared-mime-info strace t1utils tzdata ucf util-linux wdiff xauth xdg-user-dirs xz-utils zstd
Suggested packages:
  cron quota ecryptfs-utils autoconf-archive gnu-standards autoconf-doc binutils-doc gprofng-gui bzip2-doc distcc | icecc cpp-doc gcc-13-locales cpp-13-doc default-dbus-session-bus | dbus-session-bus debtags dh-make
  default-jre adequate at autopkgtest bls-standalone bsd-mailx | mailx check-all-the-things cvs-buildpackage diffoscope disorderfs dose-extra duck elpa-devscripts faketime gnuplot how-can-i-help libdbd-pg-perl
  libfile-desktopentry-perl libterm-size-perl libyaml-syck-perl mmdebstrap mutt piuparts postgresql-client pristine-lfs quilt ratt reprotest svn-buildpackage w3m debian-keyring equivs libgitlab-api-v4-perl
  libsoap-lite-perl pristine-tar dbus-user-session libpam-systemd pinentry-gnome3 tor mini-dinstall ant g++-multilib g++-13-multilib gcc-13-doc gawk-doc gcc-multilib flex bison gdb gcc-doc gcc-13-multilib
  gdb-x86-64-linux-gnu gettext-doc libasprintf-dev libgettextpo-dev git-daemon-run | git-daemon-sysvinit git-doc git-email git-gui gitk gitweb git-cvs git-mediawiki git-svn parcimonie xloadimage gpg-wks-server scdaemon
  groff isoquery apache2 | lighttpd | httpd alsa-utils libasound2-plugins libdigest-hmac-perl libgssapi-perl glibc-doc cups-common bzr libgd-tools gdbm-l10n low-memory-monitor gpm krb5-doc krb5-user libheif-plugin-x265
  libheif-plugin-ffmpegdec libheif-plugin-jpegdec libheif-plugin-jpegenc libheif-plugin-j2kdec libheif-plugin-j2kenc libheif-plugin-rav1e libheif-plugin-svtenc libio-compress-brotli-perl liblcms2-utils libtool-doc
  libcrypt-ssleay-perl cryptsetup-bin ncurses-doc libscalar-number-perl pcscd libsasl2-modules-gssapi-mit | libsasl2-modules-gssapi-heimdal libsasl2-modules-ldap libsasl2-modules-otp libsasl2-modules-sql libssl-doc
  libstdc++-13-doc libbareword-filehandles-perl libmultidimensional-perl libxstring-perl gfortran | fortran95-compiler gcj-jdk libbusiness-isbn-perl libregexp-ipv6-perl libauthen-ntlm-perl libxml-sax-expatxs-perl
  bash-completion binutils-multiarch libtext-template-perl m4-doc make-doc apparmor www-browser nfs-common libnss-mdns fonts-dejavu-extra fonts-ipafont-gothic fonts-ipafont-mincho fonts-wqy-microhei | fonts-wqy-zenhei
  fonts-indic keychain libpam-ssh monkeysphere ssh-askpass ed diffutils-doc perl-doc libterm-readline-gnu-perl | libterm-readline-perl-perl libtap-harness-archive-perl pinentry-doc libmail-box-perl python3-doc
  python3-tk python3-venv python-apt-doc python-cryptography-doc python3-cryptography-vectors python-nacl-doc python3-gssapi python3-invoke python3-openssl python3-socks python-requests-doc python-setuptools-doc
  python3-brotli python-pyxdg-doc python3.12-venv python3.12-doc readline-doc openssh-server python3-braceexpand db5.3-util libapache2-mod-svn subversion-tools swig-doc swig-examples zip dosfstools kbd util-linux-extra
  util-linux-locales wdiff-doc
Recommended packages:
  uuid-runtime
The following NEW packages will be installed:
  adduser alsa-topology-conf alsa-ucm-conf autoconf automake autopoint autotools-dev binfmt-support binutils binutils-common binutils-x86-64-linux-gnu bsdextrautils build-essential bzip2 ca-certificates
  ca-certificates-java ccache cpp cpp-13 cpp-13-x86-64-linux-gnu cpp-x86-64-linux-gnu dbus dbus-bin dbus-daemon dbus-session-bus-common dbus-system-bus-common dctrl-tools debhelper debugedit default-jre-headless
  devscripts dh-autoreconf dh-strip-nondeterminism diffstat dirmngr distro-info-data dpkg-dev dput dwz ecj fakeroot fastjar file fontconfig-config fonts-dejavu-core fonts-dejavu-mono g++ g++-13 g++-13-x86-64-linux-gnu
  g++-x86-64-linux-gnu gawk gcc gcc-13 gcc-13-base gcc-13-x86-64-linux-gnu gcc-x86-64-linux-gnu gettext gettext-base git git-man gnupg gnupg-l10n gnupg-utils gpg gpg-agent gpg-wks-client gpgconf gpgsm groff-base
  intltool-debian iso-codes jarwrapper java-common java-propose-classpath java-wrappers javahelper javascript-common keyboxd krb5-locales less libalgorithm-diff-perl libalgorithm-diff-xs-perl libalgorithm-merge-perl
  libaliased-perl libaom3 libapparmor1 libapr1t64 libaprutil1t64 libapt-pkg-perl libarchive-cpio-perl libarchive-zip-perl libarray-intspan-perl libasan8 libasound2-data libasound2t64 libatomic1 libauthen-sasl-perl
  libavahi-client3 libavahi-common-data libavahi-common3 libb-hooks-endofscope-perl libb-hooks-op-check-perl libberkeleydb-perl libbinutils libbrotli1 libbsd0 libc-dev-bin libc-devtools libc6-dev libcapture-tiny-perl
  libcbor0.10 libcc1-0 libcgi-fast-perl libcgi-pm-perl libclass-data-inheritable-perl libclass-method-modifiers-perl libclass-xsaccessor-perl libclone-perl libcommon-sense-perl libconfig-tiny-perl libconst-fast-perl
  libcpanel-json-xs-perl libcrypt-dev libctf-nobfd0 libctf0 libcups2t64 libcurl3t64-gnutls libdata-dpath-perl libdata-dump-perl libdata-messagepack-perl libdata-optlist-perl libdata-validate-domain-perl
  libdata-validate-ip-perl libdata-validate-uri-perl libdbus-1-3 libde265-0 libdebhelper-perl libdeflate0 libdevel-callchecker-perl libdevel-size-perl libdevel-stacktrace-perl libdistro-info-perl libdpkg-perl libdw1t64
  libdynaloader-functions-perl libeclipse-jdt-core-java libedit2 libelf-dev libelf1t64 libemail-address-xs-perl libencode-locale-perl liberror-perl libexception-class-perl libexpat1 libexpat1-dev libexporter-tiny-perl
  libfakeroot libfcgi-bin libfcgi-perl libfcgi0t64 libfido2-1 libfile-basedir-perl libfile-chdir-perl libfile-dirlist-perl libfile-fcntllock-perl libfile-find-rule-perl libfile-homedir-perl libfile-listing-perl
  libfile-stripnondeterminism-perl libfile-touch-perl libfile-which-perl libfont-afm-perl libfont-ttf-perl libfontconfig1 libfreetype6 libfreezethaw-perl libgcc-13-dev libgd3 libgdbm-compat4t64 libgdbm6t64
  libgit-wrapper-perl libglib2.0-0t64 libglib2.0-data libgomp1 libgpgme11t64 libgpm2 libgprofng0 libgraphite2-3 libgssapi-krb5-2 libharfbuzz0b libheif-plugin-aomdec libheif-plugin-aomenc libheif-plugin-libde265
  libheif1 libhiredis1.1.0 libhtml-form-perl libhtml-format-perl libhtml-html5-entities-perl libhtml-parser-perl libhtml-tagset-perl libhtml-tokeparser-simple-perl libhtml-tree-perl libhttp-cookies-perl
  libhttp-daemon-perl libhttp-date-perl libhttp-message-perl libhttp-negotiate-perl libhwasan0 libicu74 libimport-into-perl libindirect-perl libio-html-perl libio-interactive-perl libio-pty-perl libio-socket-ssl-perl
  libio-string-perl libipc-run-perl libipc-run3-perl libipc-system-simple-perl libisl23 libiterator-perl libiterator-util-perl libitm1 libjansson4 libjbig0 libjpeg-turbo8 libjpeg8 libjs-jquery libjs-sphinxdoc
  libjs-underscore libjson-maybexs-perl libjson-perl libjson-xs-perl libk5crypto3 libkeyutils1 libkrb5-3 libkrb5support0 libksba8 liblcms2-2 libldap-common libldap2 liblerc4 liblist-compare-perl liblist-someutils-perl
  liblist-someutils-xs-perl liblist-utilsby-perl liblocale-gettext-perl liblog-any-adapter-screen-perl liblog-any-perl liblsan0 libltdl-dev libltdl7 liblwp-mediatypes-perl liblwp-protocol-https-perl liblzo2-2
  libmagic-mgc libmagic1t64 libmail-sendmail-perl libmailtools-perl libmarkdown2 libmath-base85-perl libmldbm-perl libmodule-implementation-perl libmodule-runtime-perl libmoo-perl libmoox-aliases-perl libmouse-perl
  libmpc3 libmpfr6 libnamespace-clean-perl libncurses-dev libncurses6 libnet-domain-tld-perl libnet-http-perl libnet-ipv6addr-perl libnet-netmask-perl libnet-smtp-ssl-perl libnet-ssleay-perl libnetaddr-ip-perl
  libnghttp2-14 libnspr4 libnss3 libnumber-compare-perl libobject-pad-perl libpackage-stash-perl libpackage-stash-xs-perl libparams-classify-perl libparams-util-perl libpath-iterator-rule-perl libpath-tiny-perl
  libpcsclite1 libperl5.38t64 libperlio-gzip-perl libperlio-utf8-strict-perl libpipeline1 libpng16-16t64 libpod-constants-perl libpod-parser-perl libpopt0 libproc-processtable-perl libpsl5t64 libpython3-dev
  libpython3-stdlib libpython3.12-dev libpython3.12-minimal libpython3.12-stdlib libpython3.12t64 libquadmath0 libre-engine-re2-perl libre2-10 libreadline8t64 libregexp-pattern-license-perl libregexp-pattern-perl
  libregexp-wildcards-perl librole-tiny-perl librtmp1 libsasl2-2 libsasl2-modules libsasl2-modules-db libsereal-decoder-perl libsereal-encoder-perl libserf-1-1 libset-intspan-perl libsframe1 libsharpyuv0 libsigsegv2
  libsocket6-perl libsodium23 libsort-versions-perl libsqlite3-0 libssh-4 libssl-dev libstdc++-13-dev libstrictures-perl libstring-copyright-perl libstring-escape-perl libstring-license-perl libstring-shellquote-perl
  libsub-exporter-perl libsub-exporter-progressive-perl libsub-identify-perl libsub-install-perl libsub-name-perl libsub-override-perl libsub-quote-perl libsvn1 libsyntax-keyword-try-perl libsys-hostname-long-perl
  libterm-readkey-perl libtext-glob-perl libtext-levenshteinxs-perl libtext-markdown-discount-perl libtext-xslate-perl libtiff6 libtime-duration-perl libtime-moment-perl libtimedate-perl libtool libtry-tiny-perl
  libtsan2 libtypes-serialiser-perl libubsan1 libuchardet0 libunicode-utf8-perl libunwind8 liburi-perl libutf8proc3 libvariable-magic-perl libwebp7 libwww-mechanize-perl libwww-perl libwww-robotrules-perl libx11-6
  libx11-data libxau6 libxcb1 libxdmcp6 libxext6 libxml-libxml-perl libxml-namespacesupport-perl libxml-parser-perl libxml-sax-base-perl libxml-sax-expat-perl libxml-sax-perl libxml2 libxmuu1 libxpm4
  libxs-parse-keyword-perl libxs-parse-sublike-perl libxslt1.1 libyaml-0-2 libyaml-libyaml-perl libzstd-dev licensecheck lintian linux-libc-dev lsb-release lto-disabled-list lzip lzop m4 make man-db manpages
  manpages-dev media-types netbase openjdk-21-jre-headless openssh-client openssl patch patchutils perl perl-modules-5.38 perl-openssl-defaults pinentry-curses po-debconf publicsuffix python-apt-common python3
  python3-apt python3-bcrypt python3-certifi python3-cffi-backend python3-chardet python3-cryptography python3-debian python3-dev python3-gpg python3-idna python3-magic python3-minimal python3-nacl python3-paramiko
  python3-pkg-resources python3-requests python3-setuptools python3-six python3-unidiff python3-urllib3 python3-xdg python3.12 python3.12-dev python3.12-minimal readline-common rpcsvc-proto rsync shared-mime-info
  strace subversion swig t1utils time tzdata ucf unzip wdiff wget xauth xdg-user-dirs xsltproc xz-utils zlib1g-dev zstd
The following packages will be upgraded:
  bsdutils libblkid1 libmount1 libsmartcols1 libuuid1 mount util-linux
7 upgraded, 464 newly installed, 0 to remove and 0 not upgraded.
Need to get 227 MB of archives.
After this operation, 855 MB of additional disk space will be used.
Get:1 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 bsdutils amd64 1:2.39.3-9ubuntu6.2 [95.2 kB]
Get:2 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 util-linux amd64 2.39.3-9ubuntu6.2 [1127 kB]
Get:3 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 mount amd64 2.39.3-9ubuntu6.2 [118 kB]
Get:4 http://archive.ubuntu.com/ubuntu noble/main amd64 liblocale-gettext-perl amd64 1.07-6ubuntu5 [15.8 kB]
Get:5 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libpython3.12-minimal amd64 3.12.3-1ubuntu0.4 [834 kB]
Get:6 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libexpat1 amd64 2.6.1-2ubuntu0.2 [87.4 kB]
Get:7 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 python3.12-minimal amd64 3.12.3-1ubuntu0.4 [2336 kB]
Get:8 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 python3-minimal amd64 3.12.3-0ubuntu2 [27.4 kB]
Get:9 http://archive.ubuntu.com/ubuntu noble/main amd64 media-types all 10.1.0 [27.5 kB]
Get:10 http://archive.ubuntu.com/ubuntu noble/main amd64 netbase all 6.4 [13.1 kB]
Get:11 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 tzdata all 2024a-3ubuntu1.1 [273 kB]
Get:12 http://archive.ubuntu.com/ubuntu noble/main amd64 readline-common all 8.2-4build1 [56.5 kB]
Get:13 http://archive.ubuntu.com/ubuntu noble/main amd64 libreadline8t64 amd64 8.2-4build1 [153 kB]
Get:14 http://archive.ubuntu.com/ubuntu noble/main amd64 libsqlite3-0 amd64 3.45.1-1ubuntu2 [701 kB]
Get:15 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libpython3.12-stdlib amd64 3.12.3-1ubuntu0.4 [2068 kB]
Get:16 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 python3.12 amd64 3.12.3-1ubuntu0.4 [651 kB]
Get:17 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libpython3-stdlib amd64 3.12.3-0ubuntu2 [10.0 kB]
Get:18 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 python3 amd64 3.12.3-0ubuntu2 [23.0 kB]
Get:19 http://archive.ubuntu.com/ubuntu noble/main amd64 libpopt0 amd64 1.19+dfsg-1build1 [28.6 kB]
Get:20 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 rsync amd64 3.2.7-1ubuntu1.2 [436 kB]
Get:21 http://archive.ubuntu.com/ubuntu noble/main amd64 libpipeline1 amd64 1.5.7-2 [23.6 kB]
Get:22 http://archive.ubuntu.com/ubuntu noble/universe amd64 binfmt-support amd64 2.2.2-7 [56.2 kB]
Get:23 http://archive.ubuntu.com/ubuntu noble/main amd64 libmpfr6 amd64 4.2.1-1build1 [355 kB]
Get:24 http://archive.ubuntu.com/ubuntu noble/main amd64 libsigsegv2 amd64 2.14-1ubuntu2 [15.0 kB]
Get:25 http://archive.ubuntu.com/ubuntu noble/main amd64 gawk amd64 1:5.2.1-2build3 [463 kB]
Get:26 http://archive.ubuntu.com/ubuntu noble/main amd64 perl-modules-5.38 all 5.38.2-3.2build2 [3110 kB]
Get:27 http://archive.ubuntu.com/ubuntu noble/main amd64 libgdbm6t64 amd64 1.23-5.1build1 [34.4 kB]
Get:28 http://archive.ubuntu.com/ubuntu noble/main amd64 libgdbm-compat4t64 amd64 1.23-5.1build1 [6710 B]
Get:29 http://archive.ubuntu.com/ubuntu noble/main amd64 libperl5.38t64 amd64 5.38.2-3.2build2 [4873 kB]
Get:30 http://archive.ubuntu.com/ubuntu noble/main amd64 perl amd64 5.38.2-3.2build2 [231 kB]
Get:31 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libblkid1 amd64 2.39.3-9ubuntu6.2 [123 kB]
Get:32 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libmount1 amd64 2.39.3-9ubuntu6.2 [134 kB]
Get:33 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libsmartcols1 amd64 2.39.3-9ubuntu6.2 [65.2 kB]
Get:34 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libuuid1 amd64 2.39.3-9ubuntu6.2 [35.5 kB]
Get:35 http://archive.ubuntu.com/ubuntu noble/main amd64 adduser all 3.137ubuntu1 [101 kB]
Get:36 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 openssl amd64 3.0.13-0ubuntu3.4 [1003 kB]
Get:37 http://archive.ubuntu.com/ubuntu noble/main amd64 ca-certificates all 20240203 [159 kB]
Get:38 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libdbus-1-3 amd64 1.14.10-4ubuntu4.1 [210 kB]
Get:39 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 dbus-bin amd64 1.14.10-4ubuntu4.1 [39.3 kB]
Get:40 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 dbus-session-bus-common all 1.14.10-4ubuntu4.1 [80.5 kB]
Get:41 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libapparmor1 amd64 4.0.1really4.0.1-0ubuntu0.24.04.3 [50.3 kB]
Get:42 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 dbus-daemon amd64 1.14.10-4ubuntu4.1 [118 kB]
Get:43 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 dbus-system-bus-common all 1.14.10-4ubuntu4.1 [81.6 kB]
Get:44 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 dbus amd64 1.14.10-4ubuntu4.1 [24.3 kB]
Get:45 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 distro-info-data all 0.60ubuntu0.2 [6590 B]
Get:46 http://archive.ubuntu.com/ubuntu noble/main amd64 iso-codes all 4.16.0-1 [3492 kB]
Get:47 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 krb5-locales all 1.20.1-6ubuntu2.2 [14.0 kB]
Get:48 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 less amd64 590-2ubuntu2.1 [142 kB]
Get:49 http://archive.ubuntu.com/ubuntu noble/main amd64 libbsd0 amd64 0.12.1-1build1 [41.2 kB]
Get:50 http://archive.ubuntu.com/ubuntu noble/main amd64 libelf1t64 amd64 0.190-1.1build4 [57.6 kB]
Get:51 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libglib2.0-0t64 amd64 2.80.0-6ubuntu3.2 [1546 kB]
Get:52 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libglib2.0-data all 2.80.0-6ubuntu3.2 [48.5 kB]
Get:53 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libkrb5support0 amd64 1.20.1-6ubuntu2.2 [33.7 kB]
Get:54 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libk5crypto3 amd64 1.20.1-6ubuntu2.2 [81.8 kB]
Get:55 http://archive.ubuntu.com/ubuntu noble/main amd64 libkeyutils1 amd64 1.6.3-3build1 [9490 B]
Get:56 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libkrb5-3 amd64 1.20.1-6ubuntu2.2 [347 kB]
Get:57 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libgssapi-krb5-2 amd64 1.20.1-6ubuntu2.2 [143 kB]
Get:58 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libicu74 amd64 74.2-1ubuntu3.1 [10.9 MB]
Get:59 http://archive.ubuntu.com/ubuntu noble/main amd64 libxml2 amd64 2.9.14+dfsg-1.3ubuntu3 [762 kB]
Get:60 http://archive.ubuntu.com/ubuntu noble/main amd64 libyaml-0-2 amd64 0.2.5-1build1 [51.5 kB]
Get:61 http://archive.ubuntu.com/ubuntu noble/main amd64 lsb-release all 12.0-2 [6564 B]
Get:62 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 python-apt-common all 2.7.7ubuntu3 [74.4 kB]
Get:63 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 python3-apt amd64 2.7.7ubuntu3 [169 kB]
Get:64 http://archive.ubuntu.com/ubuntu noble/main amd64 python3-cffi-backend amd64 1.16.0-2build1 [77.3 kB]
Get:65 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 python3-pkg-resources all 68.1.2-2ubuntu1.1 [168 kB]
Get:66 http://archive.ubuntu.com/ubuntu noble/main amd64 shared-mime-info amd64 2.4-4 [474 kB]
Get:67 http://archive.ubuntu.com/ubuntu noble/main amd64 ucf all 3.0043+nmu1 [56.5 kB]
Get:68 http://archive.ubuntu.com/ubuntu noble/main amd64 xdg-user-dirs amd64 0.18-1build1 [18.4 kB]
Get:69 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 bsdextrautils amd64 2.39.3-9ubuntu6.2 [73.9 kB]
Get:70 http://archive.ubuntu.com/ubuntu noble/main amd64 libmagic-mgc amd64 1:5.45-3build1 [307 kB]
Get:71 http://archive.ubuntu.com/ubuntu noble/main amd64 libmagic1t64 amd64 1:5.45-3build1 [87.2 kB]
Get:72 http://archive.ubuntu.com/ubuntu noble/main amd64 file amd64 1:5.45-3build1 [22.0 kB]
Get:73 http://archive.ubuntu.com/ubuntu noble/main amd64 gettext-base amd64 0.21-14ubuntu2 [38.4 kB]
Get:74 http://archive.ubuntu.com/ubuntu noble/main amd64 libuchardet0 amd64 0.0.8-1build1 [75.3 kB]
Get:75 http://archive.ubuntu.com/ubuntu noble/main amd64 groff-base amd64 1.23.0-3build2 [1020 kB]
Get:76 http://archive.ubuntu.com/ubuntu noble/main amd64 libcbor0.10 amd64 0.10.2-1.2ubuntu2 [25.8 kB]
Get:77 http://archive.ubuntu.com/ubuntu noble/main amd64 libedit2 amd64 3.1-20230828-1build1 [97.6 kB]
Get:78 http://archive.ubuntu.com/ubuntu noble/main amd64 libfido2-1 amd64 1.14.0-1build3 [83.5 kB]
Get:79 http://archive.ubuntu.com/ubuntu noble/main amd64 libgpm2 amd64 1.20.7-11 [14.1 kB]
Get:80 http://archive.ubuntu.com/ubuntu noble/main amd64 libjansson4 amd64 2.14-2build2 [32.8 kB]
Get:81 http://archive.ubuntu.com/ubuntu noble/main amd64 libncurses6 amd64 6.4+20240113-1ubuntu2 [112 kB]
Get:82 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libnghttp2-14 amd64 1.59.0-1ubuntu0.1 [74.3 kB]
Get:83 http://archive.ubuntu.com/ubuntu noble/main amd64 libpng16-16t64 amd64 1.6.43-5build1 [187 kB]
Get:84 http://archive.ubuntu.com/ubuntu noble/main amd64 libpsl5t64 amd64 0.21.2-1.1build1 [57.1 kB]
Get:85 http://archive.ubuntu.com/ubuntu noble/main amd64 libxau6 amd64 1:1.0.9-1build6 [7160 B]
Get:86 http://archive.ubuntu.com/ubuntu noble/main amd64 libxdmcp6 amd64 1:1.1.3-0ubuntu6 [10.3 kB]
Get:87 http://archive.ubuntu.com/ubuntu noble/main amd64 libxcb1 amd64 1.15-1ubuntu2 [47.7 kB]
Get:88 http://archive.ubuntu.com/ubuntu noble/main amd64 libx11-data all 2:1.8.7-1build1 [115 kB]
Get:89 http://archive.ubuntu.com/ubuntu noble/main amd64 libx11-6 amd64 2:1.8.7-1build1 [650 kB]
Get:90 http://archive.ubuntu.com/ubuntu noble/main amd64 libxext6 amd64 2:1.3.4-1build2 [30.4 kB]
Get:91 http://archive.ubuntu.com/ubuntu noble/main amd64 libxmuu1 amd64 2:1.1.3-3build2 [8958 B]
Get:92 http://archive.ubuntu.com/ubuntu noble/main amd64 man-db amd64 2.12.0-4build2 [1237 kB]
Get:93 http://archive.ubuntu.com/ubuntu noble/main amd64 manpages all 6.7-2 [1384 kB]
Get:94 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 openssh-client amd64 1:9.6p1-3ubuntu13.5 [905 kB]
Get:95 http://archive.ubuntu.com/ubuntu noble/main amd64 publicsuffix all 20231001.0357-0.1 [129 kB]
Get:96 http://archive.ubuntu.com/ubuntu noble/main amd64 libunwind8 amd64 1.6.2-3build1 [55.2 kB]
Get:97 http://archive.ubuntu.com/ubuntu noble/main amd64 strace amd64 6.8-0ubuntu2 [584 kB]
Get:98 http://archive.ubuntu.com/ubuntu noble/main amd64 time amd64 1.9-0.2build1 [45.2 kB]
Get:99 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 wget amd64 1.21.4-1ubuntu4.1 [334 kB]
Get:100 http://archive.ubuntu.com/ubuntu noble/main amd64 xauth amd64 1:1.1.2-1build1 [25.6 kB]
Get:101 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 xz-utils amd64 5.6.1+really5.4.5-1build0.1 [267 kB]
Get:102 http://archive.ubuntu.com/ubuntu noble/main amd64 alsa-topology-conf all 1.2.5.1-2 [15.5 kB]
Get:103 http://archive.ubuntu.com/ubuntu noble/main amd64 libasound2-data all 1.2.11-1build2 [21.0 kB]
Get:104 http://archive.ubuntu.com/ubuntu noble/main amd64 libasound2t64 amd64 1.2.11-1build2 [399 kB]
Get:105 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 alsa-ucm-conf all 1.2.10-1ubuntu5.4 [64.8 kB]
Get:106 http://archive.ubuntu.com/ubuntu noble/main amd64 m4 amd64 1.4.19-4build1 [244 kB]
Get:107 http://archive.ubuntu.com/ubuntu noble/main amd64 autoconf all 2.71-3 [339 kB]
Get:108 http://archive.ubuntu.com/ubuntu noble/main amd64 autotools-dev all 20220109.1 [44.9 kB]
Get:109 http://archive.ubuntu.com/ubuntu noble/main amd64 automake all 1:1.16.5-1.3ubuntu1 [558 kB]
Get:110 http://archive.ubuntu.com/ubuntu noble/main amd64 autopoint all 0.21-14ubuntu2 [422 kB]
Get:111 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 binutils-common amd64 2.42-4ubuntu2.3 [239 kB]
Get:112 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libsframe1 amd64 2.42-4ubuntu2.3 [14.9 kB]
Get:113 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libbinutils amd64 2.42-4ubuntu2.3 [575 kB]
Get:114 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libctf-nobfd0 amd64 2.42-4ubuntu2.3 [97.1 kB]
Get:115 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libctf0 amd64 2.42-4ubuntu2.3 [94.5 kB]
Get:116 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libgprofng0 amd64 2.42-4ubuntu2.3 [849 kB]
Get:117 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 binutils-x86-64-linux-gnu amd64 2.42-4ubuntu2.3 [2463 kB]
Get:118 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 binutils amd64 2.42-4ubuntu2.3 [18.1 kB]
Get:119 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libc-dev-bin amd64 2.39-0ubuntu8.3 [60.8 kB]
Get:120 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 linux-libc-dev amd64 6.8.0-51.52 [1769 kB]
Get:121 http://archive.ubuntu.com/ubuntu noble/main amd64 libcrypt-dev amd64 1:4.4.36-4build1 [112 kB]
Get:122 http://archive.ubuntu.com/ubuntu noble/main amd64 rpcsvc-proto amd64 1.4.2-0ubuntu7 [67.4 kB]
Get:123 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libc6-dev amd64 2.39-0ubuntu8.3 [2164 kB]
Get:124 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 gcc-13-base amd64 13.3.0-6ubuntu2~24.04 [51.5 kB]
Get:125 http://archive.ubuntu.com/ubuntu noble/main amd64 libisl23 amd64 0.26-3build1 [680 kB]
Get:126 http://archive.ubuntu.com/ubuntu noble/main amd64 libmpc3 amd64 1.3.1-1build1 [54.5 kB]
Get:127 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 cpp-13-x86-64-linux-gnu amd64 13.3.0-6ubuntu2~24.04 [10.7 MB]
Get:128 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 cpp-13 amd64 13.3.0-6ubuntu2~24.04 [1038 B]
Get:129 http://archive.ubuntu.com/ubuntu noble/main amd64 cpp-x86-64-linux-gnu amd64 4:13.2.0-7ubuntu1 [5326 B]
Get:130 http://archive.ubuntu.com/ubuntu noble/main amd64 cpp amd64 4:13.2.0-7ubuntu1 [22.4 kB]
Get:131 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libcc1-0 amd64 14.2.0-4ubuntu2~24.04 [48.0 kB]
Get:132 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libgomp1 amd64 14.2.0-4ubuntu2~24.04 [148 kB]
Get:133 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libitm1 amd64 14.2.0-4ubuntu2~24.04 [29.7 kB]
Get:134 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libatomic1 amd64 14.2.0-4ubuntu2~24.04 [10.5 kB]
Get:135 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libasan8 amd64 14.2.0-4ubuntu2~24.04 [3031 kB]
Get:136 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 liblsan0 amd64 14.2.0-4ubuntu2~24.04 [1322 kB]
Get:137 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libtsan2 amd64 14.2.0-4ubuntu2~24.04 [2772 kB]
Get:138 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libubsan1 amd64 14.2.0-4ubuntu2~24.04 [1184 kB]
Get:139 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libhwasan0 amd64 14.2.0-4ubuntu2~24.04 [1641 kB]
Get:140 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libquadmath0 amd64 14.2.0-4ubuntu2~24.04 [153 kB]
Get:141 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libgcc-13-dev amd64 13.3.0-6ubuntu2~24.04 [2681 kB]
Get:142 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 gcc-13-x86-64-linux-gnu amd64 13.3.0-6ubuntu2~24.04 [21.1 MB]
Get:143 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 gcc-13 amd64 13.3.0-6ubuntu2~24.04 [494 kB]
Get:144 http://archive.ubuntu.com/ubuntu noble/main amd64 gcc-x86-64-linux-gnu amd64 4:13.2.0-7ubuntu1 [1212 B]
Get:145 http://archive.ubuntu.com/ubuntu noble/main amd64 gcc amd64 4:13.2.0-7ubuntu1 [5018 B]
Get:146 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libstdc++-13-dev amd64 13.3.0-6ubuntu2~24.04 [2420 kB]
Get:147 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 g++-13-x86-64-linux-gnu amd64 13.3.0-6ubuntu2~24.04 [12.2 MB]
Get:148 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 g++-13 amd64 13.3.0-6ubuntu2~24.04 [16.1 kB]
Get:149 http://archive.ubuntu.com/ubuntu noble/main amd64 g++-x86-64-linux-gnu amd64 4:13.2.0-7ubuntu1 [964 B]
Get:150 http://archive.ubuntu.com/ubuntu noble/main amd64 g++ amd64 4:13.2.0-7ubuntu1 [1100 B]
Get:151 http://archive.ubuntu.com/ubuntu noble/main amd64 make amd64 4.3-4.1build2 [180 kB]
Get:152 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libdpkg-perl all 1.22.6ubuntu6.1 [269 kB]
Get:153 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 bzip2 amd64 1.0.8-5.1build0.1 [34.5 kB]
Get:154 http://archive.ubuntu.com/ubuntu noble/main amd64 patch amd64 2.7.6-7build3 [104 kB]
Get:155 http://archive.ubuntu.com/ubuntu noble/main amd64 lto-disabled-list all 47 [12.4 kB]
Get:156 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 dpkg-dev all 1.22.6ubuntu6.1 [1074 kB]
Get:157 http://archive.ubuntu.com/ubuntu noble/main amd64 build-essential amd64 12.10ubuntu1 [4928 B]
Get:158 http://archive.ubuntu.com/ubuntu noble/main amd64 ca-certificates-java all 20240118 [11.6 kB]
Get:159 http://archive.ubuntu.com/ubuntu noble/universe amd64 libhiredis1.1.0 amd64 1.2.0-6ubuntu3 [41.4 kB]
Get:160 http://archive.ubuntu.com/ubuntu noble/universe amd64 ccache amd64 4.9.1-1 [592 kB]
Get:161 http://archive.ubuntu.com/ubuntu noble/main amd64 dctrl-tools amd64 2.24-3build3 [106 kB]
Get:162 http://archive.ubuntu.com/ubuntu noble/main amd64 libdebhelper-perl all 13.14.1ubuntu5 [89.8 kB]
Get:163 http://archive.ubuntu.com/ubuntu noble/main amd64 libtool all 2.4.7-7build1 [166 kB]
Get:164 http://archive.ubuntu.com/ubuntu noble/main amd64 dh-autoreconf all 20 [16.1 kB]
Get:165 http://archive.ubuntu.com/ubuntu noble/main amd64 libarchive-zip-perl all 1.68-1 [90.2 kB]
Get:166 http://archive.ubuntu.com/ubuntu noble/main amd64 libsub-override-perl all 0.10-1 [10.0 kB]
Get:167 http://archive.ubuntu.com/ubuntu noble/main amd64 libfile-stripnondeterminism-perl all 1.13.1-1 [18.1 kB]
Get:168 http://archive.ubuntu.com/ubuntu noble/main amd64 dh-strip-nondeterminism all 1.13.1-1 [5362 B]
Get:169 http://archive.ubuntu.com/ubuntu noble/main amd64 libdw1t64 amd64 0.190-1.1build4 [261 kB]
Get:170 http://archive.ubuntu.com/ubuntu noble/main amd64 debugedit amd64 1:5.0-5build2 [46.1 kB]
Get:171 http://archive.ubuntu.com/ubuntu noble/main amd64 dwz amd64 0.15-1build6 [115 kB]
Get:172 http://archive.ubuntu.com/ubuntu noble/main amd64 gettext amd64 0.21-14ubuntu2 [864 kB]
Get:173 http://archive.ubuntu.com/ubuntu noble/main amd64 intltool-debian all 0.35.0+20060710.6 [23.2 kB]
Get:174 http://archive.ubuntu.com/ubuntu noble/main amd64 po-debconf all 1.0.21+nmu1 [233 kB]
Get:175 http://archive.ubuntu.com/ubuntu noble/main amd64 debhelper all 13.14.1ubuntu5 [869 kB]
Get:176 http://archive.ubuntu.com/ubuntu noble/main amd64 java-common all 0.75+exp1 [6798 B]
Get:177 http://archive.ubuntu.com/ubuntu noble/main amd64 liblcms2-2 amd64 2.14-2build1 [161 kB]
Get:178 http://archive.ubuntu.com/ubuntu noble/main amd64 libjpeg-turbo8 amd64 2.1.5-2ubuntu2 [150 kB]
Get:179 http://archive.ubuntu.com/ubuntu noble/main amd64 libjpeg8 amd64 8c-2ubuntu11 [2148 B]
Get:180 http://archive.ubuntu.com/ubuntu noble/main amd64 libnspr4 amd64 2:4.35-1.1build1 [117 kB]
Get:181 http://archive.ubuntu.com/ubuntu noble/main amd64 libnss3 amd64 2:3.98-1build1 [1445 kB]
Get:182 http://archive.ubuntu.com/ubuntu noble/main amd64 libpcsclite1 amd64 2.0.3-1build1 [21.4 kB]
Get:183 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 openjdk-21-jre-headless amd64 21.0.5+11-1ubuntu1~24.04 [46.4 MB]
Get:184 http://archive.ubuntu.com/ubuntu noble/main amd64 default-jre-headless amd64 2:1.21-75+exp1 [3094 B]
Get:185 http://archive.ubuntu.com/ubuntu noble/main amd64 gpgconf amd64 2.4.4-2ubuntu17 [103 kB]
Get:186 http://archive.ubuntu.com/ubuntu noble/main amd64 libksba8 amd64 1.6.6-1build1 [122 kB]
Get:187 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libsasl2-modules-db amd64 2.1.28+dfsg1-5ubuntu3.1 [20.4 kB]
Get:188 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libsasl2-2 amd64 2.1.28+dfsg1-5ubuntu3.1 [53.2 kB]
Get:189 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libldap2 amd64 2.6.7+dfsg-1~exp1ubuntu8.1 [195 kB]
Get:190 http://archive.ubuntu.com/ubuntu noble/main amd64 dirmngr amd64 2.4.4-2ubuntu17 [323 kB]
Get:191 http://archive.ubuntu.com/ubuntu noble/main amd64 gnupg-utils amd64 2.4.4-2ubuntu17 [108 kB]
Get:192 http://archive.ubuntu.com/ubuntu noble/main amd64 gpg amd64 2.4.4-2ubuntu17 [565 kB]
Get:193 http://archive.ubuntu.com/ubuntu noble/main amd64 pinentry-curses amd64 1.2.1-3ubuntu5 [35.2 kB]
Get:194 http://archive.ubuntu.com/ubuntu noble/main amd64 gpg-agent amd64 2.4.4-2ubuntu17 [227 kB]
Get:195 http://archive.ubuntu.com/ubuntu noble/main amd64 gpgsm amd64 2.4.4-2ubuntu17 [232 kB]
Get:196 http://archive.ubuntu.com/ubuntu noble/main amd64 keyboxd amd64 2.4.4-2ubuntu17 [78.3 kB]
Get:197 http://archive.ubuntu.com/ubuntu noble/main amd64 gnupg all 2.4.4-2ubuntu17 [359 kB]
Get:198 http://archive.ubuntu.com/ubuntu noble/main amd64 libfile-dirlist-perl all 0.05-3 [7286 B]
Get:199 http://archive.ubuntu.com/ubuntu noble/main amd64 libfile-which-perl all 1.27-2 [12.5 kB]
Get:200 http://archive.ubuntu.com/ubuntu noble/main amd64 libfile-homedir-perl all 1.006-2 [37.0 kB]
Get:201 http://archive.ubuntu.com/ubuntu noble/main amd64 libfile-touch-perl all 0.12-2 [7498 B]
Get:202 http://archive.ubuntu.com/ubuntu noble/main amd64 libio-pty-perl amd64 1:1.20-1build2 [31.2 kB]
Get:203 http://archive.ubuntu.com/ubuntu noble/main amd64 libipc-run-perl all 20231003.0-1 [92.1 kB]
Get:204 http://archive.ubuntu.com/ubuntu noble/main amd64 libclass-method-modifiers-perl all 2.15-1 [16.1 kB]
Get:205 http://archive.ubuntu.com/ubuntu noble/main amd64 libclass-xsaccessor-perl amd64 1.19-4build4 [33.1 kB]
Get:206 http://archive.ubuntu.com/ubuntu noble/main amd64 libb-hooks-op-check-perl amd64 0.22-3build1 [9518 B]
Get:207 http://archive.ubuntu.com/ubuntu noble/main amd64 libdynaloader-functions-perl all 0.003-3 [12.1 kB]
Get:208 http://archive.ubuntu.com/ubuntu noble/main amd64 libdevel-callchecker-perl amd64 0.008-2build3 [13.2 kB]
Get:209 http://archive.ubuntu.com/ubuntu noble/main amd64 libparams-classify-perl amd64 0.015-2build5 [20.1 kB]
Get:210 http://archive.ubuntu.com/ubuntu noble/main amd64 libmodule-runtime-perl all 0.016-2 [16.4 kB]
Get:211 http://archive.ubuntu.com/ubuntu noble/main amd64 libimport-into-perl all 1.002005-2 [10.7 kB]
Get:212 http://archive.ubuntu.com/ubuntu noble/main amd64 librole-tiny-perl all 2.002004-1 [16.3 kB]
Get:213 http://archive.ubuntu.com/ubuntu noble/main amd64 libsub-quote-perl all 2.006008-1ubuntu1 [20.7 kB]
Get:214 http://archive.ubuntu.com/ubuntu noble/main amd64 libmoo-perl all 2.005005-1 [47.4 kB]
Get:215 http://archive.ubuntu.com/ubuntu noble/main amd64 libencode-locale-perl all 1.05-3 [11.6 kB]
Get:216 http://archive.ubuntu.com/ubuntu noble/main amd64 libtimedate-perl all 2.3300-2 [34.0 kB]
Get:217 http://archive.ubuntu.com/ubuntu noble/main amd64 libhttp-date-perl all 6.06-1 [10.2 kB]
Get:218 http://archive.ubuntu.com/ubuntu noble/main amd64 libfile-listing-perl all 6.16-1 [11.3 kB]
Get:219 http://archive.ubuntu.com/ubuntu noble/main amd64 libhtml-tagset-perl all 3.20-6 [11.3 kB]
Get:220 http://archive.ubuntu.com/ubuntu noble/main amd64 liburi-perl all 5.27-1 [88.0 kB]
Get:221 http://archive.ubuntu.com/ubuntu noble/main amd64 libhtml-parser-perl amd64 3.81-1build3 [85.8 kB]
Get:222 http://archive.ubuntu.com/ubuntu noble/main amd64 libhtml-tree-perl all 5.07-3 [200 kB]
Get:223 http://archive.ubuntu.com/ubuntu noble/main amd64 libclone-perl amd64 0.46-1build3 [10.7 kB]
Get:224 http://archive.ubuntu.com/ubuntu noble/main amd64 libio-html-perl all 1.004-3 [15.9 kB]
Get:225 http://archive.ubuntu.com/ubuntu noble/main amd64 liblwp-mediatypes-perl all 6.04-2 [20.1 kB]
Get:226 http://archive.ubuntu.com/ubuntu noble/main amd64 libhttp-message-perl all 6.45-1ubuntu1 [78.2 kB]
Get:227 http://archive.ubuntu.com/ubuntu noble/main amd64 libhttp-cookies-perl all 6.11-1 [18.2 kB]
Get:228 http://archive.ubuntu.com/ubuntu noble/main amd64 libhttp-negotiate-perl all 6.01-2 [12.4 kB]
Get:229 http://archive.ubuntu.com/ubuntu noble/main amd64 perl-openssl-defaults amd64 7build3 [6626 B]
Get:230 http://archive.ubuntu.com/ubuntu noble/main amd64 libnet-ssleay-perl amd64 1.94-1build4 [316 kB]
Get:231 http://archive.ubuntu.com/ubuntu noble/main amd64 libio-socket-ssl-perl all 2.085-1 [195 kB]
Get:232 http://archive.ubuntu.com/ubuntu noble/main amd64 libnet-http-perl all 6.23-1 [22.3 kB]
Get:233 http://archive.ubuntu.com/ubuntu noble/main amd64 liblwp-protocol-https-perl all 6.13-1 [9006 B]
Get:234 http://archive.ubuntu.com/ubuntu noble/main amd64 libtry-tiny-perl all 0.31-2 [20.8 kB]
Get:235 http://archive.ubuntu.com/ubuntu noble/main amd64 libwww-robotrules-perl all 6.02-1 [12.6 kB]
Get:236 http://archive.ubuntu.com/ubuntu noble/main amd64 libwww-perl all 6.76-1 [138 kB]
Get:237 http://archive.ubuntu.com/ubuntu noble/main amd64 patchutils amd64 0.4.2-1build3 [77.0 kB]
Get:238 http://archive.ubuntu.com/ubuntu noble/main amd64 wdiff amd64 1.2.2-6build1 [29.1 kB]
Get:239 http://archive.ubuntu.com/ubuntu noble/main amd64 devscripts all 2.23.7 [1069 kB]
Get:240 http://archive.ubuntu.com/ubuntu noble/main amd64 diffstat amd64 1.66-1build1 [29.7 kB]
Get:241 http://archive.ubuntu.com/ubuntu noble/main amd64 python3-chardet all 5.2.0+dfsg-1 [117 kB]
Get:242 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 zstd amd64 1.5.5+dfsg2-2build1.1 [644 kB]
Get:243 http://archive.ubuntu.com/ubuntu noble/main amd64 python3-debian all 0.1.49ubuntu2 [115 kB]
Get:244 http://archive.ubuntu.com/ubuntu noble/main amd64 libgpgme11t64 amd64 1.18.0-4.1ubuntu4 [136 kB]
Get:245 http://archive.ubuntu.com/ubuntu noble/main amd64 python3-gpg amd64 1.18.0-4.1ubuntu4 [209 kB]
Get:246 http://archive.ubuntu.com/ubuntu noble/main amd64 python3-xdg all 0.28-2 [38.3 kB]
Get:247 http://archive.ubuntu.com/ubuntu noble/main amd64 dput all 1.1.3ubuntu3 [46.5 kB]
Get:248 http://archive.ubuntu.com/ubuntu noble/universe amd64 libeclipse-jdt-core-java all 3.32.0+eclipse4.26-2 [6438 kB]
Get:249 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 unzip amd64 6.0-28ubuntu4.1 [174 kB]
Get:250 http://archive.ubuntu.com/ubuntu noble/universe amd64 java-wrappers all 0.4 [8662 B]
Get:251 http://archive.ubuntu.com/ubuntu noble/universe amd64 ecj all 3.32.0+eclipse4.26-2 [16.5 kB]
Get:252 http://archive.ubuntu.com/ubuntu noble/main amd64 libfakeroot amd64 1.33-1 [32.4 kB]
Get:253 http://archive.ubuntu.com/ubuntu noble/main amd64 fakeroot amd64 1.33-1 [67.2 kB]
Get:254 http://archive.ubuntu.com/ubuntu noble/main amd64 fonts-dejavu-mono all 2.37-8 [502 kB]
Get:255 http://archive.ubuntu.com/ubuntu noble/main amd64 fonts-dejavu-core all 2.37-8 [835 kB]
Get:256 http://archive.ubuntu.com/ubuntu noble/main amd64 fontconfig-config amd64 2.15.0-1.1ubuntu2 [37.3 kB]
Get:257 http://archive.ubuntu.com/ubuntu noble/main amd64 libbrotli1 amd64 1.1.0-2build2 [331 kB]
Get:258 http://archive.ubuntu.com/ubuntu noble/main amd64 librtmp1 amd64 2.4+20151223.gitfa8646d.1-2build7 [56.3 kB]
Get:259 http://archive.ubuntu.com/ubuntu noble/main amd64 libssh-4 amd64 0.10.6-2build2 [188 kB]
Get:260 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libcurl3t64-gnutls amd64 8.5.0-2ubuntu10.6 [333 kB]
Get:261 http://archive.ubuntu.com/ubuntu noble/main amd64 liberror-perl all 0.17029-2 [25.6 kB]
Get:262 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 git-man all 1:2.43.0-1ubuntu7.2 [1100 kB]
Get:263 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 git amd64 1:2.43.0-1ubuntu7.2 [3679 kB]
Get:264 http://archive.ubuntu.com/ubuntu noble/main amd64 gnupg-l10n all 2.4.4-2ubuntu17 [65.9 kB]
Get:265 http://archive.ubuntu.com/ubuntu noble/main amd64 gpg-wks-client amd64 2.4.4-2ubuntu17 [70.9 kB]
Get:266 http://archive.ubuntu.com/ubuntu noble/universe amd64 fastjar amd64 2:0.98-7 [67.1 kB]
Get:267 http://archive.ubuntu.com/ubuntu noble/universe amd64 jarwrapper all 0.79 [10.3 kB]
Get:268 http://archive.ubuntu.com/ubuntu noble/universe amd64 javahelper all 0.79 [83.5 kB]
Get:269 http://archive.ubuntu.com/ubuntu noble/universe amd64 java-propose-classpath all 0.79 [3284 B]
Get:270 http://archive.ubuntu.com/ubuntu noble/main amd64 javascript-common all 11+nmu1 [5936 B]
Get:271 http://archive.ubuntu.com/ubuntu noble/main amd64 libalgorithm-diff-perl all 1.201-1 [41.8 kB]
Get:272 http://archive.ubuntu.com/ubuntu noble/main amd64 libalgorithm-diff-xs-perl amd64 0.04-8build3 [11.2 kB]
Get:273 http://archive.ubuntu.com/ubuntu noble/main amd64 libalgorithm-merge-perl all 0.08-5 [11.4 kB]
Get:274 http://archive.ubuntu.com/ubuntu noble/main amd64 libaliased-perl all 0.34-3 [12.8 kB]
Get:275 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libaom3 amd64 3.8.2-2ubuntu0.1 [1941 kB]
Get:276 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libapr1t64 amd64 1.7.2-3.1ubuntu0.1 [108 kB]
Get:277 http://archive.ubuntu.com/ubuntu noble/main amd64 libaprutil1t64 amd64 1.6.3-1.1ubuntu7 [91.9 kB]
Get:278 http://archive.ubuntu.com/ubuntu noble/main amd64 libapt-pkg-perl amd64 0.1.40build7 [68.4 kB]
Get:279 http://archive.ubuntu.com/ubuntu noble/main amd64 libarchive-cpio-perl all 0.10-3 [10.3 kB]
Get:280 http://archive.ubuntu.com/ubuntu noble/main amd64 libarray-intspan-perl all 2.004-2 [25.0 kB]
Get:281 http://archive.ubuntu.com/ubuntu noble/main amd64 libavahi-common-data amd64 0.8-13ubuntu6 [29.7 kB]
Get:282 http://archive.ubuntu.com/ubuntu noble/main amd64 libavahi-common3 amd64 0.8-13ubuntu6 [23.3 kB]
Get:283 http://archive.ubuntu.com/ubuntu noble/main amd64 libavahi-client3 amd64 0.8-13ubuntu6 [26.8 kB]
Get:284 http://archive.ubuntu.com/ubuntu noble/main amd64 libmodule-implementation-perl all 0.09-2 [12.0 kB]
Get:285 http://archive.ubuntu.com/ubuntu noble/main amd64 libsub-exporter-progressive-perl all 0.001013-3 [6718 B]
Get:286 http://archive.ubuntu.com/ubuntu noble/main amd64 libvariable-magic-perl amd64 0.63-1build3 [35.1 kB]
Get:287 http://archive.ubuntu.com/ubuntu noble/main amd64 libb-hooks-endofscope-perl all 0.28-1 [15.8 kB]
Get:288 http://archive.ubuntu.com/ubuntu noble/main amd64 libberkeleydb-perl amd64 0.64-2build4 [120 kB]
Get:289 http://archive.ubuntu.com/ubuntu noble/main amd64 libfreetype6 amd64 2.13.2+dfsg-1build3 [402 kB]
Get:290 http://archive.ubuntu.com/ubuntu noble/main amd64 libfontconfig1 amd64 2.15.0-1.1ubuntu2 [139 kB]
Get:291 http://archive.ubuntu.com/ubuntu noble/main amd64 libsharpyuv0 amd64 1.3.2-0.4build3 [15.8 kB]
Get:292 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libheif-plugin-aomdec amd64 1.17.6-1ubuntu4.1 [10.4 kB]
Get:293 http://archive.ubuntu.com/ubuntu noble/main amd64 libde265-0 amd64 1.0.15-1build3 [166 kB]
Get:294 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libheif-plugin-libde265 amd64 1.17.6-1ubuntu4.1 [8176 B]
Get:295 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libheif1 amd64 1.17.6-1ubuntu4.1 [275 kB]
Get:296 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libdeflate0 amd64 1.19-1build1.1 [43.9 kB]
Get:297 http://archive.ubuntu.com/ubuntu noble/main amd64 libjbig0 amd64 2.1-6.1ubuntu2 [29.7 kB]
Get:298 http://archive.ubuntu.com/ubuntu noble/main amd64 liblerc4 amd64 4.0.0+ds-4ubuntu2 [179 kB]
Get:299 http://archive.ubuntu.com/ubuntu noble/main amd64 libwebp7 amd64 1.3.2-0.4build3 [230 kB]
Get:300 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libtiff6 amd64 4.5.1+git230720-4ubuntu2.2 [199 kB]
Get:301 http://archive.ubuntu.com/ubuntu noble/main amd64 libxpm4 amd64 1:3.5.17-1build2 [36.5 kB]
Get:302 http://archive.ubuntu.com/ubuntu noble/main amd64 libgd3 amd64 2.3.3-9ubuntu5 [128 kB]
Get:303 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libc-devtools amd64 2.39-0ubuntu8.3 [69.7 kB]
Get:304 http://archive.ubuntu.com/ubuntu noble/main amd64 libcapture-tiny-perl all 0.48-2 [20.2 kB]
Get:305 http://archive.ubuntu.com/ubuntu noble/main amd64 libcgi-pm-perl all 4.63-1 [185 kB]
Get:306 http://archive.ubuntu.com/ubuntu noble/main amd64 libfcgi0t64 amd64 2.4.2-2.1build1 [26.8 kB]
Get:307 http://archive.ubuntu.com/ubuntu noble/main amd64 libfcgi-perl amd64 0.82+ds-3build2 [21.7 kB]
Get:308 http://archive.ubuntu.com/ubuntu noble/main amd64 libcgi-fast-perl all 1:2.17-1 [10.3 kB]
Get:309 http://archive.ubuntu.com/ubuntu noble/main amd64 libclass-data-inheritable-perl all 0.08-3 [8084 B]
Get:310 http://archive.ubuntu.com/ubuntu noble/main amd64 libcommon-sense-perl amd64 3.75-3build3 [20.4 kB]
Get:311 http://archive.ubuntu.com/ubuntu noble/main amd64 libconfig-tiny-perl all 2.30-1 [14.7 kB]
Get:312 http://archive.ubuntu.com/ubuntu noble/main amd64 libparams-util-perl amd64 1.102-2build3 [21.2 kB]
Get:313 http://archive.ubuntu.com/ubuntu noble/main amd64 libsub-install-perl all 0.929-1 [9764 B]
Get:314 http://archive.ubuntu.com/ubuntu noble/main amd64 libdata-optlist-perl all 0.114-1 [9708 B]
Get:315 http://archive.ubuntu.com/ubuntu noble/main amd64 libsub-exporter-perl all 0.990-1 [49.0 kB]
Get:316 http://archive.ubuntu.com/ubuntu noble/main amd64 libconst-fast-perl all 0.014-2 [8034 B]
Get:317 http://archive.ubuntu.com/ubuntu noble/main amd64 libcpanel-json-xs-perl amd64 4.37-1build3 [114 kB]
Get:318 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libcups2t64 amd64 2.4.7-1.2ubuntu7.3 [272 kB]
Get:319 http://archive.ubuntu.com/ubuntu noble/main amd64 libdevel-stacktrace-perl all 2.0500-1 [22.1 kB]
Get:320 http://archive.ubuntu.com/ubuntu noble/main amd64 libexception-class-perl all 1.45-1 [28.6 kB]
Get:321 http://archive.ubuntu.com/ubuntu noble/main amd64 libiterator-perl all 0.03+ds1-2 [18.8 kB]
Get:322 http://archive.ubuntu.com/ubuntu noble/main amd64 libiterator-util-perl all 0.02+ds1-2 [14.1 kB]
Get:323 http://archive.ubuntu.com/ubuntu noble/main amd64 libdata-dpath-perl all 0.59-1 [39.2 kB]
Get:324 http://archive.ubuntu.com/ubuntu noble/main amd64 libdata-dump-perl all 1.25-1 [25.9 kB]
Get:325 http://archive.ubuntu.com/ubuntu noble/main amd64 libdata-messagepack-perl amd64 1.02-1build4 [31.1 kB]
Get:326 http://archive.ubuntu.com/ubuntu noble/main amd64 libnet-domain-tld-perl all 1.75-3 [29.4 kB]
Get:327 http://archive.ubuntu.com/ubuntu noble/main amd64 libdata-validate-domain-perl all 0.10-1.1 [9992 B]
Get:328 http://archive.ubuntu.com/ubuntu noble/main amd64 libnet-ipv6addr-perl all 1.02-1 [21.0 kB]
Get:329 http://archive.ubuntu.com/ubuntu noble/main amd64 libnet-netmask-perl all 2.0002-2 [24.8 kB]
Get:330 http://archive.ubuntu.com/ubuntu noble/main amd64 libnetaddr-ip-perl amd64 4.079+dfsg-2build4 [79.9 kB]
Get:331 http://archive.ubuntu.com/ubuntu noble/main amd64 libdata-validate-ip-perl all 0.31-1 [17.2 kB]
Get:332 http://archive.ubuntu.com/ubuntu noble/main amd64 libdata-validate-uri-perl all 0.07-3 [10.8 kB]
Get:333 http://archive.ubuntu.com/ubuntu noble/main amd64 libdistro-info-perl all 1.7build1 [5616 B]
Get:334 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 zlib1g-dev amd64 1:1.3.dfsg-3.1ubuntu2.1 [894 kB]
Get:335 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libzstd-dev amd64 1.5.5+dfsg2-2build1.1 [364 kB]
Get:336 http://archive.ubuntu.com/ubuntu noble/main amd64 libelf-dev amd64 0.190-1.1build4 [68.5 kB]
Get:337 http://archive.ubuntu.com/ubuntu noble/main amd64 libemail-address-xs-perl amd64 1.05-1build4 [29.1 kB]
Get:338 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libexpat1-dev amd64 2.6.1-2ubuntu0.2 [998 kB]
Get:339 http://archive.ubuntu.com/ubuntu noble/main amd64 libexporter-tiny-perl all 1.006002-1 [36.8 kB]
Get:340 http://archive.ubuntu.com/ubuntu noble/main amd64 libfcgi-bin amd64 2.4.2-2.1build1 [11.2 kB]
Get:341 http://archive.ubuntu.com/ubuntu noble/main amd64 libipc-system-simple-perl all 1.30-2 [22.3 kB]
Get:342 http://archive.ubuntu.com/ubuntu noble/main amd64 libfile-basedir-perl all 0.09-2 [14.4 kB]
Get:343 http://archive.ubuntu.com/ubuntu noble/main amd64 libfile-chdir-perl all 0.1008-1.1 [10.6 kB]
Get:344 http://archive.ubuntu.com/ubuntu noble/main amd64 libfile-fcntllock-perl amd64 0.22-4ubuntu5 [30.7 kB]
Get:345 http://archive.ubuntu.com/ubuntu noble/main amd64 libnumber-compare-perl all 0.03-3 [5974 B]
Get:346 http://archive.ubuntu.com/ubuntu noble/main amd64 libtext-glob-perl all 0.11-3 [6780 B]
Get:347 http://archive.ubuntu.com/ubuntu noble/main amd64 libfile-find-rule-perl all 0.34-3 [24.4 kB]
Get:348 http://archive.ubuntu.com/ubuntu noble/main amd64 libfont-afm-perl all 1.20-4 [13.0 kB]
Get:349 http://archive.ubuntu.com/ubuntu noble/main amd64 libio-string-perl all 1.08-4 [11.1 kB]
Get:350 http://archive.ubuntu.com/ubuntu noble/main amd64 libfont-ttf-perl all 1.06-2 [323 kB]
Get:351 http://archive.ubuntu.com/ubuntu noble/main amd64 libfreezethaw-perl all 0.5001-3 [14.6 kB]
Get:352 http://archive.ubuntu.com/ubuntu noble/main amd64 libsort-versions-perl all 1.62-3 [7378 B]
Get:353 http://archive.ubuntu.com/ubuntu noble/main amd64 libgit-wrapper-perl all 0.048-2 [29.5 kB]
Get:354 http://archive.ubuntu.com/ubuntu noble/main amd64 libgraphite2-3 amd64 1.3.14-2build1 [73.0 kB]
Get:355 http://archive.ubuntu.com/ubuntu noble/main amd64 libharfbuzz0b amd64 8.3.0-2build2 [469 kB]
Get:356 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libheif-plugin-aomenc amd64 1.17.6-1ubuntu4.1 [14.7 kB]
Get:357 http://archive.ubuntu.com/ubuntu noble/main amd64 libhtml-form-perl all 6.11-1 [32.1 kB]
Get:358 http://archive.ubuntu.com/ubuntu noble/main amd64 libhtml-format-perl all 2.16-2 [36.9 kB]
Get:359 http://archive.ubuntu.com/ubuntu noble/main amd64 libhtml-html5-entities-perl all 0.004-3 [21.6 kB]
Get:360 http://archive.ubuntu.com/ubuntu noble/main amd64 libhtml-tokeparser-simple-perl all 3.16-4 [38.0 kB]
Get:361 http://archive.ubuntu.com/ubuntu noble/main amd64 libhttp-daemon-perl all 6.16-1 [22.4 kB]
Get:362 http://archive.ubuntu.com/ubuntu noble/main amd64 libindirect-perl amd64 0.39-2build4 [22.1 kB]
Get:363 http://archive.ubuntu.com/ubuntu noble/main amd64 libio-interactive-perl all 1.025-1 [10.4 kB]
Get:364 http://archive.ubuntu.com/ubuntu noble/main amd64 libjs-jquery all 3.6.1+dfsg+~3.5.14-1 [328 kB]
Get:365 http://archive.ubuntu.com/ubuntu noble/main amd64 libjs-underscore all 1.13.4~dfsg+~1.11.4-3 [118 kB]
Get:366 http://archive.ubuntu.com/ubuntu noble/main amd64 libjs-sphinxdoc all 7.2.6-6 [149 kB]
Get:367 http://archive.ubuntu.com/ubuntu noble/main amd64 libtypes-serialiser-perl all 1.01-1 [11.6 kB]
Get:368 http://archive.ubuntu.com/ubuntu noble/main amd64 libjson-xs-perl amd64 4.030-2build3 [83.6 kB]
Get:369 http://archive.ubuntu.com/ubuntu noble/main amd64 libjson-maybexs-perl all 1.004005-1 [11.3 kB]
Get:370 http://archive.ubuntu.com/ubuntu noble/main amd64 libjson-perl all 4.10000-1 [81.9 kB]
Get:371 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libldap-common all 2.6.7+dfsg-1~exp1ubuntu8.1 [31.5 kB]
Get:372 http://archive.ubuntu.com/ubuntu noble/main amd64 liblist-compare-perl all 0.55-2 [62.9 kB]
Get:373 http://archive.ubuntu.com/ubuntu noble/main amd64 liblist-someutils-perl all 0.59-1 [30.4 kB]
Get:374 http://archive.ubuntu.com/ubuntu noble/main amd64 liblist-someutils-xs-perl amd64 0.58-3build4 [35.0 kB]
Get:375 http://archive.ubuntu.com/ubuntu noble/main amd64 liblist-utilsby-perl all 0.12-2 [14.9 kB]
Get:376 http://archive.ubuntu.com/ubuntu noble/main amd64 liblog-any-perl all 1.717-1 [73.2 kB]
Get:377 http://archive.ubuntu.com/ubuntu noble/main amd64 liblog-any-adapter-screen-perl all 0.140-2 [12.4 kB]
Get:378 http://archive.ubuntu.com/ubuntu noble/main amd64 libltdl7 amd64 2.4.7-7build1 [40.3 kB]
Get:379 http://archive.ubuntu.com/ubuntu noble/main amd64 libltdl-dev amd64 2.4.7-7build1 [168 kB]
Get:380 http://archive.ubuntu.com/ubuntu noble/main amd64 liblzo2-2 amd64 2.10-2build4 [54.1 kB]
Get:381 http://archive.ubuntu.com/ubuntu noble/main amd64 libsys-hostname-long-perl all 1.5-3 [10.6 kB]
Get:382 http://archive.ubuntu.com/ubuntu noble/main amd64 libmail-sendmail-perl all 0.80-3 [21.7 kB]
Get:383 http://archive.ubuntu.com/ubuntu noble/main amd64 libnet-smtp-ssl-perl all 1.04-2 [6218 B]
Get:384 http://archive.ubuntu.com/ubuntu noble/main amd64 libmailtools-perl all 2.21-2 [80.4 kB]
Get:385 http://archive.ubuntu.com/ubuntu noble/main amd64 libmarkdown2 amd64 2.2.7-2build1 [37.5 kB]
Get:386 http://archive.ubuntu.com/ubuntu noble/main amd64 libmath-base85-perl all 0.5+dfsg-2 [6124 B]
Get:387 http://archive.ubuntu.com/ubuntu noble/main amd64 libmldbm-perl all 2.05-4 [16.0 kB]
Get:388 http://archive.ubuntu.com/ubuntu noble/main amd64 libstrictures-perl all 2.000006-1 [16.3 kB]
Get:389 http://archive.ubuntu.com/ubuntu noble/main amd64 libmoox-aliases-perl all 0.001006-2 [6796 B]
Get:390 http://archive.ubuntu.com/ubuntu noble/main amd64 libmouse-perl amd64 2.5.10-1build8 [133 kB]
Get:391 http://archive.ubuntu.com/ubuntu noble/main amd64 libpackage-stash-perl all 0.40-1 [19.5 kB]
Get:392 http://archive.ubuntu.com/ubuntu noble/main amd64 libsub-identify-perl amd64 0.14-3build3 [9786 B]
Get:393 http://archive.ubuntu.com/ubuntu noble/main amd64 libsub-name-perl amd64 0.27-1build3 [10.8 kB]
Get:394 http://archive.ubuntu.com/ubuntu noble/main amd64 libnamespace-clean-perl all 0.27-2 [14.0 kB]
Get:395 http://archive.ubuntu.com/ubuntu noble/main amd64 libncurses-dev amd64 6.4+20240113-1ubuntu2 [384 kB]
Get:396 http://archive.ubuntu.com/ubuntu noble/main amd64 libxs-parse-keyword-perl amd64 0.39-1build3 [54.7 kB]
Get:397 http://archive.ubuntu.com/ubuntu noble/main amd64 libxs-parse-sublike-perl amd64 0.21-2build3 [39.9 kB]
Get:398 http://archive.ubuntu.com/ubuntu noble/main amd64 libobject-pad-perl amd64 0.808-1build3 [108 kB]
Get:399 http://archive.ubuntu.com/ubuntu noble/main amd64 libpackage-stash-xs-perl amd64 0.30-1build4 [18.7 kB]
Get:400 http://archive.ubuntu.com/ubuntu noble/main amd64 libpath-iterator-rule-perl all 1.015-2 [39.9 kB]
Get:401 http://archive.ubuntu.com/ubuntu noble/main amd64 libpath-tiny-perl all 0.144-1 [47.7 kB]
Get:402 http://archive.ubuntu.com/ubuntu noble/main amd64 libperlio-gzip-perl amd64 0.20-1build4 [14.6 kB]
Get:403 http://archive.ubuntu.com/ubuntu noble/main amd64 libperlio-utf8-strict-perl amd64 0.010-1build3 [11.1 kB]
Get:404 http://archive.ubuntu.com/ubuntu noble/main amd64 libpod-parser-perl all 1.67-1 [80.6 kB]
Get:405 http://archive.ubuntu.com/ubuntu noble/main amd64 libpod-constants-perl all 0.19-2 [16.3 kB]
Get:406 http://archive.ubuntu.com/ubuntu noble/main amd64 libproc-processtable-perl amd64 0.636-1build3 [35.7 kB]
Get:407 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libpython3.12t64 amd64 3.12.3-1ubuntu0.4 [2346 kB]
Get:408 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libpython3.12-dev amd64 3.12.3-1ubuntu0.4 [5675 kB]
Get:409 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libpython3-dev amd64 3.12.3-0ubuntu2 [10.3 kB]
Get:410 http://archive.ubuntu.com/ubuntu noble/main amd64 libre2-10 amd64 20230301-3build1 [168 kB]
Get:411 http://archive.ubuntu.com/ubuntu noble/main amd64 libre-engine-re2-perl amd64 0.18+ds-1build3 [18.6 kB]
Get:412 http://archive.ubuntu.com/ubuntu noble/main amd64 libregexp-pattern-license-perl all 3.11.0-1 [85.8 kB]
Get:413 http://archive.ubuntu.com/ubuntu noble/main amd64 libregexp-pattern-perl all 0.2.14-2 [17.6 kB]
Get:414 http://archive.ubuntu.com/ubuntu noble/main amd64 libregexp-wildcards-perl all 1.05-3 [12.9 kB]
Get:415 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libsasl2-modules amd64 2.1.28+dfsg1-5ubuntu3.1 [69.9 kB]
Get:416 http://archive.ubuntu.com/ubuntu noble/main amd64 libsereal-decoder-perl amd64 5.004+ds-1build3 [99.5 kB]
Get:417 http://archive.ubuntu.com/ubuntu noble/main amd64 libsereal-encoder-perl amd64 5.004+ds-1build3 [103 kB]
Get:418 http://archive.ubuntu.com/ubuntu noble-updates/universe amd64 libserf-1-1 amd64 1.3.10-1ubuntu0.24.04.1 [48.2 kB]
Get:419 http://archive.ubuntu.com/ubuntu noble/main amd64 libset-intspan-perl all 1.19-3 [24.8 kB]
Get:420 http://archive.ubuntu.com/ubuntu noble/main amd64 libsocket6-perl amd64 0.29-3build3 [17.5 kB]
Get:421 http://archive.ubuntu.com/ubuntu noble/main amd64 libsodium23 amd64 1.0.18-1build3 [161 kB]
Get:422 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libssl-dev amd64 3.0.13-0ubuntu3.4 [2408 kB]
Get:423 http://archive.ubuntu.com/ubuntu noble/main amd64 libstring-copyright-perl all 0.003014-1 [20.5 kB]
Get:424 http://archive.ubuntu.com/ubuntu noble/main amd64 libstring-escape-perl all 2010.002-3 [16.1 kB]
Get:425 http://archive.ubuntu.com/ubuntu noble/main amd64 libstring-license-perl all 0.0.9-2ubuntu1 [35.0 kB]
Get:426 http://archive.ubuntu.com/ubuntu noble/main amd64 libstring-shellquote-perl all 1.04-3 [11.3 kB]
Get:427 http://archive.ubuntu.com/ubuntu noble/universe amd64 libutf8proc3 amd64 2.9.0-1build1 [70.6 kB]
Get:428 http://archive.ubuntu.com/ubuntu noble/universe amd64 libsvn1 amd64 1.14.3-1build4 [1345 kB]
Get:429 http://archive.ubuntu.com/ubuntu noble/main amd64 libsyntax-keyword-try-perl amd64 0.29-1build3 [24.3 kB]
Get:430 http://archive.ubuntu.com/ubuntu noble/main amd64 libterm-readkey-perl amd64 2.38-2build4 [23.1 kB]
Get:431 http://archive.ubuntu.com/ubuntu noble/main amd64 libtext-levenshteinxs-perl amd64 0.03-5build4 [7966 B]
Get:432 http://archive.ubuntu.com/ubuntu noble/main amd64 libtext-markdown-discount-perl amd64 0.16-1build3 [12.1 kB]
Get:433 http://archive.ubuntu.com/ubuntu noble/main amd64 libtext-xslate-perl amd64 3.5.9-1build5 [161 kB]
Get:434 http://archive.ubuntu.com/ubuntu noble/main amd64 libtime-duration-perl all 1.21-2 [12.3 kB]
Get:435 http://archive.ubuntu.com/ubuntu noble/main amd64 libtime-moment-perl amd64 0.44-2build4 [70.9 kB]
Get:436 http://archive.ubuntu.com/ubuntu noble/main amd64 libunicode-utf8-perl amd64 0.62-2build3 [18.1 kB]
Get:437 http://archive.ubuntu.com/ubuntu noble/main amd64 libwww-mechanize-perl all 2.18-1ubuntu1 [93.1 kB]
Get:438 http://archive.ubuntu.com/ubuntu noble/main amd64 libxml-namespacesupport-perl all 1.12-2 [13.5 kB]
Get:439 http://archive.ubuntu.com/ubuntu noble/main amd64 libxml-sax-base-perl all 1.09-3 [18.9 kB]
Get:440 http://archive.ubuntu.com/ubuntu noble/main amd64 libxml-sax-perl all 1.02+dfsg-3 [57.0 kB]
Get:441 http://archive.ubuntu.com/ubuntu noble/main amd64 libxml-libxml-perl amd64 2.0207+dfsg+really+2.0134-1build4 [304 kB]
Get:442 http://archive.ubuntu.com/ubuntu noble/main amd64 libxml-parser-perl amd64 2.47-1build3 [204 kB]
Get:443 http://archive.ubuntu.com/ubuntu noble/main amd64 libxml-sax-expat-perl all 0.51-2 [8644 B]
Get:444 http://archive.ubuntu.com/ubuntu noble/main amd64 libxslt1.1 amd64 1.1.39-0exp1build1 [167 kB]
Get:445 http://archive.ubuntu.com/ubuntu noble/main amd64 libyaml-libyaml-perl amd64 0.89+ds-1build2 [30.5 kB]
Get:446 http://archive.ubuntu.com/ubuntu noble/main amd64 licensecheck all 3.3.9-1ubuntu1 [37.7 kB]
Get:447 http://archive.ubuntu.com/ubuntu noble/main amd64 libdevel-size-perl amd64 0.83-2build4 [19.6 kB]
Get:448 http://archive.ubuntu.com/ubuntu noble/main amd64 libipc-run3-perl all 0.049-1 [28.8 kB]
Get:449 http://archive.ubuntu.com/ubuntu noble/main amd64 lzip amd64 1.24.1-1build1 [83.1 kB]
Get:450 http://archive.ubuntu.com/ubuntu noble/main amd64 lzop amd64 1.04-2build3 [82.2 kB]
Get:451 http://archive.ubuntu.com/ubuntu noble/main amd64 t1utils amd64 1.41-4build3 [61.3 kB]
Get:452 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 lintian all 2.117.0ubuntu1.2 [1066 kB]
Get:453 http://archive.ubuntu.com/ubuntu noble/main amd64 manpages-dev all 6.7-2 [2013 kB]
Get:454 http://archive.ubuntu.com/ubuntu noble/main amd64 python3-certifi all 2023.11.17-1 [165 kB]
Get:455 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 python3-cryptography amd64 41.0.7-4ubuntu0.1 [810 kB]
Get:456 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 python3.12-dev amd64 3.12.3-1ubuntu0.4 [498 kB]
Get:457 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 python3-dev amd64 3.12.3-0ubuntu2 [26.7 kB]
Get:458 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 python3-idna all 3.6-2ubuntu0.1 [49.0 kB]
Get:459 http://archive.ubuntu.com/ubuntu noble/main amd64 python3-nacl amd64 1.5.0-4build1 [57.9 kB]
Get:460 http://archive.ubuntu.com/ubuntu noble/main amd64 python3-bcrypt amd64 3.2.2-1build1 [33.0 kB]
Get:461 http://archive.ubuntu.com/ubuntu noble/main amd64 python3-six all 1.16.0-4 [12.4 kB]
Get:462 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 python3-paramiko all 2.12.0-2ubuntu4.1 [137 kB]
Get:463 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 python3-urllib3 all 2.0.7-1ubuntu0.1 [93.1 kB]
Get:464 http://archive.ubuntu.com/ubuntu noble/main amd64 python3-requests all 2.31.0+dfsg-1ubuntu1 [50.7 kB]
Get:465 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 python3-setuptools all 68.1.2-2ubuntu1.1 [396 kB]
Get:466 http://archive.ubuntu.com/ubuntu noble/main amd64 python3-unidiff all 0.7.3-1 [11.0 kB]
Get:467 http://archive.ubuntu.com/ubuntu noble/universe amd64 subversion amd64 1.14.3-1build4 [908 kB]
Get:468 http://archive.ubuntu.com/ubuntu noble/universe amd64 swig amd64 4.2.0-2ubuntu1 [1187 kB]
Get:469 http://archive.ubuntu.com/ubuntu noble/main amd64 xsltproc amd64 1.1.39-0exp1build1 [15.0 kB]
Get:470 http://archive.ubuntu.com/ubuntu noble/main amd64 libauthen-sasl-perl all 2.1700-1 [42.9 kB]
Get:471 http://archive.ubuntu.com/ubuntu noble/main amd64 python3-magic all 2:0.4.27-3 [13.4 kB]
Fetched 227 MB in 24s (9325 kB/s)
debconf: delaying package configuration, since apt-utils is not installed
(Reading database ... 4379 files and directories currently installed.)
Preparing to unpack .../bsdutils_1%3a2.39.3-9ubuntu6.2_amd64.deb ...
Unpacking bsdutils (1:2.39.3-9ubuntu6.2) over (1:2.39.3-9ubuntu6.1) ...
Setting up bsdutils (1:2.39.3-9ubuntu6.2) ...
(Reading database ... 4379 files and directories currently installed.)
Preparing to unpack .../util-linux_2.39.3-9ubuntu6.2_amd64.deb ...
Unpacking util-linux (2.39.3-9ubuntu6.2) over (2.39.3-9ubuntu6.1) ...
Setting up util-linux (2.39.3-9ubuntu6.2) ...
(Reading database ... 4379 files and directories currently installed.)
Preparing to unpack .../mount_2.39.3-9ubuntu6.2_amd64.deb ...
Unpacking mount (2.39.3-9ubuntu6.2) over (2.39.3-9ubuntu6.1) ...
Selecting previously unselected package liblocale-gettext-perl.
Preparing to unpack .../liblocale-gettext-perl_1.07-6ubuntu5_amd64.deb ...
Unpacking liblocale-gettext-perl (1.07-6ubuntu5) ...
Selecting previously unselected package libpython3.12-minimal:amd64.
Preparing to unpack .../libpython3.12-minimal_3.12.3-1ubuntu0.4_amd64.deb ...
Unpacking libpython3.12-minimal:amd64 (3.12.3-1ubuntu0.4) ...
Selecting previously unselected package libexpat1:amd64.
Preparing to unpack .../libexpat1_2.6.1-2ubuntu0.2_amd64.deb ...
Unpacking libexpat1:amd64 (2.6.1-2ubuntu0.2) ...
Selecting previously unselected package python3.12-minimal.
Preparing to unpack .../python3.12-minimal_3.12.3-1ubuntu0.4_amd64.deb ...
Unpacking python3.12-minimal (3.12.3-1ubuntu0.4) ...
Setting up libpython3.12-minimal:amd64 (3.12.3-1ubuntu0.4) ...
Setting up libexpat1:amd64 (2.6.1-2ubuntu0.2) ...
Setting up python3.12-minimal (3.12.3-1ubuntu0.4) ...
Selecting previously unselected package python3-minimal.
(Reading database ... 4712 files and directories currently installed.)
Preparing to unpack .../0-python3-minimal_3.12.3-0ubuntu2_amd64.deb ...
Unpacking python3-minimal (3.12.3-0ubuntu2) ...
Selecting previously unselected package media-types.
Preparing to unpack .../1-media-types_10.1.0_all.deb ...
Unpacking media-types (10.1.0) ...
Selecting previously unselected package netbase.
Preparing to unpack .../2-netbase_6.4_all.deb ...
Unpacking netbase (6.4) ...
Selecting previously unselected package tzdata.
Preparing to unpack .../3-tzdata_2024a-3ubuntu1.1_all.deb ...
Unpacking tzdata (2024a-3ubuntu1.1) ...
Selecting previously unselected package readline-common.
Preparing to unpack .../4-readline-common_8.2-4build1_all.deb ...
Unpacking readline-common (8.2-4build1) ...
Selecting previously unselected package libreadline8t64:amd64.
Preparing to unpack .../5-libreadline8t64_8.2-4build1_amd64.deb ...
Adding 'diversion of /lib/x86_64-linux-gnu/libhistory.so.8 to /lib/x86_64-linux-gnu/libhistory.so.8.usr-is-merged by libreadline8t64'
Adding 'diversion of /lib/x86_64-linux-gnu/libhistory.so.8.2 to /lib/x86_64-linux-gnu/libhistory.so.8.2.usr-is-merged by libreadline8t64'
Adding 'diversion of /lib/x86_64-linux-gnu/libreadline.so.8 to /lib/x86_64-linux-gnu/libreadline.so.8.usr-is-merged by libreadline8t64'
Adding 'diversion of /lib/x86_64-linux-gnu/libreadline.so.8.2 to /lib/x86_64-linux-gnu/libreadline.so.8.2.usr-is-merged by libreadline8t64'
Unpacking libreadline8t64:amd64 (8.2-4build1) ...
Selecting previously unselected package libsqlite3-0:amd64.
Preparing to unpack .../6-libsqlite3-0_3.45.1-1ubuntu2_amd64.deb ...
Unpacking libsqlite3-0:amd64 (3.45.1-1ubuntu2) ...
Selecting previously unselected package libpython3.12-stdlib:amd64.
Preparing to unpack .../7-libpython3.12-stdlib_3.12.3-1ubuntu0.4_amd64.deb ...
Unpacking libpython3.12-stdlib:amd64 (3.12.3-1ubuntu0.4) ...
Selecting previously unselected package python3.12.
Preparing to unpack .../8-python3.12_3.12.3-1ubuntu0.4_amd64.deb ...
Unpacking python3.12 (3.12.3-1ubuntu0.4) ...
Selecting previously unselected package libpython3-stdlib:amd64.
Preparing to unpack .../9-libpython3-stdlib_3.12.3-0ubuntu2_amd64.deb ...
Unpacking libpython3-stdlib:amd64 (3.12.3-0ubuntu2) ...
Setting up python3-minimal (3.12.3-0ubuntu2) ...
Selecting previously unselected package python3.
(Reading database ... 5717 files and directories currently installed.)
Preparing to unpack .../0-python3_3.12.3-0ubuntu2_amd64.deb ...
Unpacking python3 (3.12.3-0ubuntu2) ...
Selecting previously unselected package libpopt0:amd64.
Preparing to unpack .../1-libpopt0_1.19+dfsg-1build1_amd64.deb ...
Unpacking libpopt0:amd64 (1.19+dfsg-1build1) ...
Selecting previously unselected package rsync.
Preparing to unpack .../2-rsync_3.2.7-1ubuntu1.2_amd64.deb ...
Unpacking rsync (3.2.7-1ubuntu1.2) ...
Selecting previously unselected package libpipeline1:amd64.
Preparing to unpack .../3-libpipeline1_1.5.7-2_amd64.deb ...
Unpacking libpipeline1:amd64 (1.5.7-2) ...
Selecting previously unselected package binfmt-support.
Preparing to unpack .../4-binfmt-support_2.2.2-7_amd64.deb ...
Unpacking binfmt-support (2.2.2-7) ...
Selecting previously unselected package libmpfr6:amd64.
Preparing to unpack .../5-libmpfr6_4.2.1-1build1_amd64.deb ...
Unpacking libmpfr6:amd64 (4.2.1-1build1) ...
Selecting previously unselected package libsigsegv2:amd64.
Preparing to unpack .../6-libsigsegv2_2.14-1ubuntu2_amd64.deb ...
Unpacking libsigsegv2:amd64 (2.14-1ubuntu2) ...
Setting up libmpfr6:amd64 (4.2.1-1build1) ...
Setting up readline-common (8.2-4build1) ...
Setting up libreadline8t64:amd64 (8.2-4build1) ...
Setting up libsigsegv2:amd64 (2.14-1ubuntu2) ...
Selecting previously unselected package gawk.
(Reading database ... 5815 files and directories currently installed.)
Preparing to unpack .../0-gawk_1%3a5.2.1-2build3_amd64.deb ...
Unpacking gawk (1:5.2.1-2build3) ...
Selecting previously unselected package perl-modules-5.38.
Preparing to unpack .../1-perl-modules-5.38_5.38.2-3.2build2_all.deb ...
Unpacking perl-modules-5.38 (5.38.2-3.2build2) ...
Selecting previously unselected package libgdbm6t64:amd64.
Preparing to unpack .../2-libgdbm6t64_1.23-5.1build1_amd64.deb ...
Unpacking libgdbm6t64:amd64 (1.23-5.1build1) ...
Selecting previously unselected package libgdbm-compat4t64:amd64.
Preparing to unpack .../3-libgdbm-compat4t64_1.23-5.1build1_amd64.deb ...
Unpacking libgdbm-compat4t64:amd64 (1.23-5.1build1) ...
Selecting previously unselected package libperl5.38t64:amd64.
Preparing to unpack .../4-libperl5.38t64_5.38.2-3.2build2_amd64.deb ...
Unpacking libperl5.38t64:amd64 (5.38.2-3.2build2) ...
Selecting previously unselected package perl.
Preparing to unpack .../5-perl_5.38.2-3.2build2_amd64.deb ...
Unpacking perl (5.38.2-3.2build2) ...
Preparing to unpack .../6-libblkid1_2.39.3-9ubuntu6.2_amd64.deb ...
Unpacking libblkid1:amd64 (2.39.3-9ubuntu6.2) over (2.39.3-9ubuntu6.1) ...
Setting up libblkid1:amd64 (2.39.3-9ubuntu6.2) ...
(Reading database ... 7993 files and directories currently installed.)
Preparing to unpack .../libmount1_2.39.3-9ubuntu6.2_amd64.deb ...
Unpacking libmount1:amd64 (2.39.3-9ubuntu6.2) over (2.39.3-9ubuntu6.1) ...
Setting up libmount1:amd64 (2.39.3-9ubuntu6.2) ...
(Reading database ... 7993 files and directories currently installed.)
Preparing to unpack .../libsmartcols1_2.39.3-9ubuntu6.2_amd64.deb ...
Unpacking libsmartcols1:amd64 (2.39.3-9ubuntu6.2) over (2.39.3-9ubuntu6.1) ...
Setting up libsmartcols1:amd64 (2.39.3-9ubuntu6.2) ...
(Reading database ... 7993 files and directories currently installed.)
Preparing to unpack .../libuuid1_2.39.3-9ubuntu6.2_amd64.deb ...
Unpacking libuuid1:amd64 (2.39.3-9ubuntu6.2) over (2.39.3-9ubuntu6.1) ...
Setting up libuuid1:amd64 (2.39.3-9ubuntu6.2) ...
Selecting previously unselected package adduser.
(Reading database ... 7993 files and directories currently installed.)
Preparing to unpack .../adduser_3.137ubuntu1_all.deb ...
Unpacking adduser (3.137ubuntu1) ...
Setting up adduser (3.137ubuntu1) ...
Selecting previously unselected package openssl.
(Reading database ... 8041 files and directories currently installed.)
Preparing to unpack .../000-openssl_3.0.13-0ubuntu3.4_amd64.deb ...
Unpacking openssl (3.0.13-0ubuntu3.4) ...
Selecting previously unselected package ca-certificates.
Preparing to unpack .../001-ca-certificates_20240203_all.deb ...
Unpacking ca-certificates (20240203) ...
Selecting previously unselected package libdbus-1-3:amd64.
Preparing to unpack .../002-libdbus-1-3_1.14.10-4ubuntu4.1_amd64.deb ...
Unpacking libdbus-1-3:amd64 (1.14.10-4ubuntu4.1) ...
Selecting previously unselected package dbus-bin.
Preparing to unpack .../003-dbus-bin_1.14.10-4ubuntu4.1_amd64.deb ...
Unpacking dbus-bin (1.14.10-4ubuntu4.1) ...
Selecting previously unselected package dbus-session-bus-common.
Preparing to unpack .../004-dbus-session-bus-common_1.14.10-4ubuntu4.1_all.deb ...
Unpacking dbus-session-bus-common (1.14.10-4ubuntu4.1) ...
Selecting previously unselected package libapparmor1:amd64.
Preparing to unpack .../005-libapparmor1_4.0.1really4.0.1-0ubuntu0.24.04.3_amd64.deb ...
Unpacking libapparmor1:amd64 (4.0.1really4.0.1-0ubuntu0.24.04.3) ...
Selecting previously unselected package dbus-daemon.
Preparing to unpack .../006-dbus-daemon_1.14.10-4ubuntu4.1_amd64.deb ...
Unpacking dbus-daemon (1.14.10-4ubuntu4.1) ...
Selecting previously unselected package dbus-system-bus-common.
Preparing to unpack .../007-dbus-system-bus-common_1.14.10-4ubuntu4.1_all.deb ...
Unpacking dbus-system-bus-common (1.14.10-4ubuntu4.1) ...
Selecting previously unselected package dbus.
Preparing to unpack .../008-dbus_1.14.10-4ubuntu4.1_amd64.deb ...
Unpacking dbus (1.14.10-4ubuntu4.1) ...
Selecting previously unselected package distro-info-data.
Preparing to unpack .../009-distro-info-data_0.60ubuntu0.2_all.deb ...
Unpacking distro-info-data (0.60ubuntu0.2) ...
Selecting previously unselected package iso-codes.
Preparing to unpack .../010-iso-codes_4.16.0-1_all.deb ...
Unpacking iso-codes (4.16.0-1) ...
Selecting previously unselected package krb5-locales.
Preparing to unpack .../011-krb5-locales_1.20.1-6ubuntu2.2_all.deb ...
Unpacking krb5-locales (1.20.1-6ubuntu2.2) ...
Selecting previously unselected package less.
Preparing to unpack .../012-less_590-2ubuntu2.1_amd64.deb ...
Unpacking less (590-2ubuntu2.1) ...
Selecting previously unselected package libbsd0:amd64.
Preparing to unpack .../013-libbsd0_0.12.1-1build1_amd64.deb ...
Unpacking libbsd0:amd64 (0.12.1-1build1) ...
Selecting previously unselected package libelf1t64:amd64.
Preparing to unpack .../014-libelf1t64_0.190-1.1build4_amd64.deb ...
Unpacking libelf1t64:amd64 (0.190-1.1build4) ...
Selecting previously unselected package libglib2.0-0t64:amd64.
Preparing to unpack .../015-libglib2.0-0t64_2.80.0-6ubuntu3.2_amd64.deb ...
Unpacking libglib2.0-0t64:amd64 (2.80.0-6ubuntu3.2) ...
Selecting previously unselected package libglib2.0-data.
Preparing to unpack .../016-libglib2.0-data_2.80.0-6ubuntu3.2_all.deb ...
Unpacking libglib2.0-data (2.80.0-6ubuntu3.2) ...
Selecting previously unselected package libkrb5support0:amd64.
Preparing to unpack .../017-libkrb5support0_1.20.1-6ubuntu2.2_amd64.deb ...
Unpacking libkrb5support0:amd64 (1.20.1-6ubuntu2.2) ...
Selecting previously unselected package libk5crypto3:amd64.
Preparing to unpack .../018-libk5crypto3_1.20.1-6ubuntu2.2_amd64.deb ...
Unpacking libk5crypto3:amd64 (1.20.1-6ubuntu2.2) ...
Selecting previously unselected package libkeyutils1:amd64.
Preparing to unpack .../019-libkeyutils1_1.6.3-3build1_amd64.deb ...
Unpacking libkeyutils1:amd64 (1.6.3-3build1) ...
Selecting previously unselected package libkrb5-3:amd64.
Preparing to unpack .../020-libkrb5-3_1.20.1-6ubuntu2.2_amd64.deb ...
Unpacking libkrb5-3:amd64 (1.20.1-6ubuntu2.2) ...
Selecting previously unselected package libgssapi-krb5-2:amd64.
Preparing to unpack .../021-libgssapi-krb5-2_1.20.1-6ubuntu2.2_amd64.deb ...
Unpacking libgssapi-krb5-2:amd64 (1.20.1-6ubuntu2.2) ...
Selecting previously unselected package libicu74:amd64.
Preparing to unpack .../022-libicu74_74.2-1ubuntu3.1_amd64.deb ...
Unpacking libicu74:amd64 (74.2-1ubuntu3.1) ...
Selecting previously unselected package libxml2:amd64.
Preparing to unpack .../023-libxml2_2.9.14+dfsg-1.3ubuntu3_amd64.deb ...
Unpacking libxml2:amd64 (2.9.14+dfsg-1.3ubuntu3) ...
Selecting previously unselected package libyaml-0-2:amd64.
Preparing to unpack .../024-libyaml-0-2_0.2.5-1build1_amd64.deb ...
Unpacking libyaml-0-2:amd64 (0.2.5-1build1) ...
Selecting previously unselected package lsb-release.
Preparing to unpack .../025-lsb-release_12.0-2_all.deb ...
Unpacking lsb-release (12.0-2) ...
Selecting previously unselected package python-apt-common.
Preparing to unpack .../026-python-apt-common_2.7.7ubuntu3_all.deb ...
Unpacking python-apt-common (2.7.7ubuntu3) ...
Selecting previously unselected package python3-apt.
Preparing to unpack .../027-python3-apt_2.7.7ubuntu3_amd64.deb ...
Unpacking python3-apt (2.7.7ubuntu3) ...
Selecting previously unselected package python3-cffi-backend:amd64.
Preparing to unpack .../028-python3-cffi-backend_1.16.0-2build1_amd64.deb ...
Unpacking python3-cffi-backend:amd64 (1.16.0-2build1) ...
Selecting previously unselected package python3-pkg-resources.
Preparing to unpack .../029-python3-pkg-resources_68.1.2-2ubuntu1.1_all.deb ...
Unpacking python3-pkg-resources (68.1.2-2ubuntu1.1) ...
Selecting previously unselected package shared-mime-info.
Preparing to unpack .../030-shared-mime-info_2.4-4_amd64.deb ...
Unpacking shared-mime-info (2.4-4) ...
Selecting previously unselected package ucf.
Preparing to unpack .../031-ucf_3.0043+nmu1_all.deb ...
Moving old data out of the way
Unpacking ucf (3.0043+nmu1) ...
Selecting previously unselected package xdg-user-dirs.
Preparing to unpack .../032-xdg-user-dirs_0.18-1build1_amd64.deb ...
Unpacking xdg-user-dirs (0.18-1build1) ...
Selecting previously unselected package bsdextrautils.
Preparing to unpack .../033-bsdextrautils_2.39.3-9ubuntu6.2_amd64.deb ...
Unpacking bsdextrautils (2.39.3-9ubuntu6.2) ...
Selecting previously unselected package libmagic-mgc.
Preparing to unpack .../034-libmagic-mgc_1%3a5.45-3build1_amd64.deb ...
Unpacking libmagic-mgc (1:5.45-3build1) ...
Selecting previously unselected package libmagic1t64:amd64.
Preparing to unpack .../035-libmagic1t64_1%3a5.45-3build1_amd64.deb ...
Unpacking libmagic1t64:amd64 (1:5.45-3build1) ...
Selecting previously unselected package file.
Preparing to unpack .../036-file_1%3a5.45-3build1_amd64.deb ...
Unpacking file (1:5.45-3build1) ...
Selecting previously unselected package gettext-base.
Preparing to unpack .../037-gettext-base_0.21-14ubuntu2_amd64.deb ...
Unpacking gettext-base (0.21-14ubuntu2) ...
Selecting previously unselected package libuchardet0:amd64.
Preparing to unpack .../038-libuchardet0_0.0.8-1build1_amd64.deb ...
Unpacking libuchardet0:amd64 (0.0.8-1build1) ...
Selecting previously unselected package groff-base.
Preparing to unpack .../039-groff-base_1.23.0-3build2_amd64.deb ...
Unpacking groff-base (1.23.0-3build2) ...
Selecting previously unselected package libcbor0.10:amd64.
Preparing to unpack .../040-libcbor0.10_0.10.2-1.2ubuntu2_amd64.deb ...
Unpacking libcbor0.10:amd64 (0.10.2-1.2ubuntu2) ...
Selecting previously unselected package libedit2:amd64.
Preparing to unpack .../041-libedit2_3.1-20230828-1build1_amd64.deb ...
Unpacking libedit2:amd64 (3.1-20230828-1build1) ...
Selecting previously unselected package libfido2-1:amd64.
Preparing to unpack .../042-libfido2-1_1.14.0-1build3_amd64.deb ...
Unpacking libfido2-1:amd64 (1.14.0-1build3) ...
Selecting previously unselected package libgpm2:amd64.
Preparing to unpack .../043-libgpm2_1.20.7-11_amd64.deb ...
Unpacking libgpm2:amd64 (1.20.7-11) ...
Selecting previously unselected package libjansson4:amd64.
Preparing to unpack .../044-libjansson4_2.14-2build2_amd64.deb ...
Unpacking libjansson4:amd64 (2.14-2build2) ...
Selecting previously unselected package libncurses6:amd64.
Preparing to unpack .../045-libncurses6_6.4+20240113-1ubuntu2_amd64.deb ...
Unpacking libncurses6:amd64 (6.4+20240113-1ubuntu2) ...
Selecting previously unselected package libnghttp2-14:amd64.
Preparing to unpack .../046-libnghttp2-14_1.59.0-1ubuntu0.1_amd64.deb ...
Unpacking libnghttp2-14:amd64 (1.59.0-1ubuntu0.1) ...
Selecting previously unselected package libpng16-16t64:amd64.
Preparing to unpack .../047-libpng16-16t64_1.6.43-5build1_amd64.deb ...
Unpacking libpng16-16t64:amd64 (1.6.43-5build1) ...
Selecting previously unselected package libpsl5t64:amd64.
Preparing to unpack .../048-libpsl5t64_0.21.2-1.1build1_amd64.deb ...
Unpacking libpsl5t64:amd64 (0.21.2-1.1build1) ...
Selecting previously unselected package libxau6:amd64.
Preparing to unpack .../049-libxau6_1%3a1.0.9-1build6_amd64.deb ...
Unpacking libxau6:amd64 (1:1.0.9-1build6) ...
Selecting previously unselected package libxdmcp6:amd64.
Preparing to unpack .../050-libxdmcp6_1%3a1.1.3-0ubuntu6_amd64.deb ...
Unpacking libxdmcp6:amd64 (1:1.1.3-0ubuntu6) ...
Selecting previously unselected package libxcb1:amd64.
Preparing to unpack .../051-libxcb1_1.15-1ubuntu2_amd64.deb ...
Unpacking libxcb1:amd64 (1.15-1ubuntu2) ...
Selecting previously unselected package libx11-data.
Preparing to unpack .../052-libx11-data_2%3a1.8.7-1build1_all.deb ...
Unpacking libx11-data (2:1.8.7-1build1) ...
Selecting previously unselected package libx11-6:amd64.
Preparing to unpack .../053-libx11-6_2%3a1.8.7-1build1_amd64.deb ...
Unpacking libx11-6:amd64 (2:1.8.7-1build1) ...
Selecting previously unselected package libxext6:amd64.
Preparing to unpack .../054-libxext6_2%3a1.3.4-1build2_amd64.deb ...
Unpacking libxext6:amd64 (2:1.3.4-1build2) ...
Selecting previously unselected package libxmuu1:amd64.
Preparing to unpack .../055-libxmuu1_2%3a1.1.3-3build2_amd64.deb ...
Unpacking libxmuu1:amd64 (2:1.1.3-3build2) ...
Selecting previously unselected package man-db.
Preparing to unpack .../056-man-db_2.12.0-4build2_amd64.deb ...
Unpacking man-db (2.12.0-4build2) ...
Selecting previously unselected package manpages.
Preparing to unpack .../057-manpages_6.7-2_all.deb ...
Unpacking manpages (6.7-2) ...
Selecting previously unselected package openssh-client.
Preparing to unpack .../058-openssh-client_1%3a9.6p1-3ubuntu13.5_amd64.deb ...
Unpacking openssh-client (1:9.6p1-3ubuntu13.5) ...
Selecting previously unselected package publicsuffix.
Preparing to unpack .../059-publicsuffix_20231001.0357-0.1_all.deb ...
Unpacking publicsuffix (20231001.0357-0.1) ...
Selecting previously unselected package libunwind8:amd64.
Preparing to unpack .../060-libunwind8_1.6.2-3build1_amd64.deb ...
Unpacking libunwind8:amd64 (1.6.2-3build1) ...
Selecting previously unselected package strace.
Preparing to unpack .../061-strace_6.8-0ubuntu2_amd64.deb ...
Unpacking strace (6.8-0ubuntu2) ...
Selecting previously unselected package time.
Preparing to unpack .../062-time_1.9-0.2build1_amd64.deb ...
Unpacking time (1.9-0.2build1) ...
Selecting previously unselected package wget.
Preparing to unpack .../063-wget_1.21.4-1ubuntu4.1_amd64.deb ...
Unpacking wget (1.21.4-1ubuntu4.1) ...
Selecting previously unselected package xauth.
Preparing to unpack .../064-xauth_1%3a1.1.2-1build1_amd64.deb ...
Unpacking xauth (1:1.1.2-1build1) ...
Selecting previously unselected package xz-utils.
Preparing to unpack .../065-xz-utils_5.6.1+really5.4.5-1build0.1_amd64.deb ...
Unpacking xz-utils (5.6.1+really5.4.5-1build0.1) ...
Selecting previously unselected package alsa-topology-conf.
Preparing to unpack .../066-alsa-topology-conf_1.2.5.1-2_all.deb ...
Unpacking alsa-topology-conf (1.2.5.1-2) ...
Selecting previously unselected package libasound2-data.
Preparing to unpack .../067-libasound2-data_1.2.11-1build2_all.deb ...
Unpacking libasound2-data (1.2.11-1build2) ...
Selecting previously unselected package libasound2t64:amd64.
Preparing to unpack .../068-libasound2t64_1.2.11-1build2_amd64.deb ...
Unpacking libasound2t64:amd64 (1.2.11-1build2) ...
Selecting previously unselected package alsa-ucm-conf.
Preparing to unpack .../069-alsa-ucm-conf_1.2.10-1ubuntu5.4_all.deb ...
Unpacking alsa-ucm-conf (1.2.10-1ubuntu5.4) ...
Selecting previously unselected package m4.
Preparing to unpack .../070-m4_1.4.19-4build1_amd64.deb ...
Unpacking m4 (1.4.19-4build1) ...
Selecting previously unselected package autoconf.
Preparing to unpack .../071-autoconf_2.71-3_all.deb ...
Unpacking autoconf (2.71-3) ...
Selecting previously unselected package autotools-dev.
Preparing to unpack .../072-autotools-dev_20220109.1_all.deb ...
Unpacking autotools-dev (20220109.1) ...
Selecting previously unselected package automake.
Preparing to unpack .../073-automake_1%3a1.16.5-1.3ubuntu1_all.deb ...
Unpacking automake (1:1.16.5-1.3ubuntu1) ...
Selecting previously unselected package autopoint.
Preparing to unpack .../074-autopoint_0.21-14ubuntu2_all.deb ...
Unpacking autopoint (0.21-14ubuntu2) ...
Selecting previously unselected package binutils-common:amd64.
Preparing to unpack .../075-binutils-common_2.42-4ubuntu2.3_amd64.deb ...
Unpacking binutils-common:amd64 (2.42-4ubuntu2.3) ...
Selecting previously unselected package libsframe1:amd64.
Preparing to unpack .../076-libsframe1_2.42-4ubuntu2.3_amd64.deb ...
Unpacking libsframe1:amd64 (2.42-4ubuntu2.3) ...
Selecting previously unselected package libbinutils:amd64.
Preparing to unpack .../077-libbinutils_2.42-4ubuntu2.3_amd64.deb ...
Unpacking libbinutils:amd64 (2.42-4ubuntu2.3) ...
Selecting previously unselected package libctf-nobfd0:amd64.
Preparing to unpack .../078-libctf-nobfd0_2.42-4ubuntu2.3_amd64.deb ...
Unpacking libctf-nobfd0:amd64 (2.42-4ubuntu2.3) ...
Selecting previously unselected package libctf0:amd64.
Preparing to unpack .../079-libctf0_2.42-4ubuntu2.3_amd64.deb ...
Unpacking libctf0:amd64 (2.42-4ubuntu2.3) ...
Selecting previously unselected package libgprofng0:amd64.
Preparing to unpack .../080-libgprofng0_2.42-4ubuntu2.3_amd64.deb ...
Unpacking libgprofng0:amd64 (2.42-4ubuntu2.3) ...
Selecting previously unselected package binutils-x86-64-linux-gnu.
Preparing to unpack .../081-binutils-x86-64-linux-gnu_2.42-4ubuntu2.3_amd64.deb ...
Unpacking binutils-x86-64-linux-gnu (2.42-4ubuntu2.3) ...
Selecting previously unselected package binutils.
Preparing to unpack .../082-binutils_2.42-4ubuntu2.3_amd64.deb ...
Unpacking binutils (2.42-4ubuntu2.3) ...
Selecting previously unselected package libc-dev-bin.
Preparing to unpack .../083-libc-dev-bin_2.39-0ubuntu8.3_amd64.deb ...
Unpacking libc-dev-bin (2.39-0ubuntu8.3) ...
Selecting previously unselected package linux-libc-dev:amd64.
Preparing to unpack .../084-linux-libc-dev_6.8.0-51.52_amd64.deb ...
Unpacking linux-libc-dev:amd64 (6.8.0-51.52) ...
Selecting previously unselected package libcrypt-dev:amd64.
Preparing to unpack .../085-libcrypt-dev_1%3a4.4.36-4build1_amd64.deb ...
Unpacking libcrypt-dev:amd64 (1:4.4.36-4build1) ...
Selecting previously unselected package rpcsvc-proto.
Preparing to unpack .../086-rpcsvc-proto_1.4.2-0ubuntu7_amd64.deb ...
Unpacking rpcsvc-proto (1.4.2-0ubuntu7) ...
Selecting previously unselected package libc6-dev:amd64.
Preparing to unpack .../087-libc6-dev_2.39-0ubuntu8.3_amd64.deb ...
Unpacking libc6-dev:amd64 (2.39-0ubuntu8.3) ...
Selecting previously unselected package gcc-13-base:amd64.
Preparing to unpack .../088-gcc-13-base_13.3.0-6ubuntu2~24.04_amd64.deb ...
Unpacking gcc-13-base:amd64 (13.3.0-6ubuntu2~24.04) ...
Selecting previously unselected package libisl23:amd64.
Preparing to unpack .../089-libisl23_0.26-3build1_amd64.deb ...
Unpacking libisl23:amd64 (0.26-3build1) ...
Selecting previously unselected package libmpc3:amd64.
Preparing to unpack .../090-libmpc3_1.3.1-1build1_amd64.deb ...
Unpacking libmpc3:amd64 (1.3.1-1build1) ...
Selecting previously unselected package cpp-13-x86-64-linux-gnu.
Preparing to unpack .../091-cpp-13-x86-64-linux-gnu_13.3.0-6ubuntu2~24.04_amd64.deb ...
Unpacking cpp-13-x86-64-linux-gnu (13.3.0-6ubuntu2~24.04) ...
Selecting previously unselected package cpp-13.
Preparing to unpack .../092-cpp-13_13.3.0-6ubuntu2~24.04_amd64.deb ...
Unpacking cpp-13 (13.3.0-6ubuntu2~24.04) ...
Selecting previously unselected package cpp-x86-64-linux-gnu.
Preparing to unpack .../093-cpp-x86-64-linux-gnu_4%3a13.2.0-7ubuntu1_amd64.deb ...
Unpacking cpp-x86-64-linux-gnu (4:13.2.0-7ubuntu1) ...
Selecting previously unselected package cpp.
Preparing to unpack .../094-cpp_4%3a13.2.0-7ubuntu1_amd64.deb ...
Unpacking cpp (4:13.2.0-7ubuntu1) ...
Selecting previously unselected package libcc1-0:amd64.
Preparing to unpack .../095-libcc1-0_14.2.0-4ubuntu2~24.04_amd64.deb ...
Unpacking libcc1-0:amd64 (14.2.0-4ubuntu2~24.04) ...
Selecting previously unselected package libgomp1:amd64.
Preparing to unpack .../096-libgomp1_14.2.0-4ubuntu2~24.04_amd64.deb ...
Unpacking libgomp1:amd64 (14.2.0-4ubuntu2~24.04) ...
Selecting previously unselected package libitm1:amd64.
Preparing to unpack .../097-libitm1_14.2.0-4ubuntu2~24.04_amd64.deb ...
Unpacking libitm1:amd64 (14.2.0-4ubuntu2~24.04) ...
Selecting previously unselected package libatomic1:amd64.
Preparing to unpack .../098-libatomic1_14.2.0-4ubuntu2~24.04_amd64.deb ...
Unpacking libatomic1:amd64 (14.2.0-4ubuntu2~24.04) ...
Selecting previously unselected package libasan8:amd64.
Preparing to unpack .../099-libasan8_14.2.0-4ubuntu2~24.04_amd64.deb ...
Unpacking libasan8:amd64 (14.2.0-4ubuntu2~24.04) ...
Selecting previously unselected package liblsan0:amd64.
Preparing to unpack .../100-liblsan0_14.2.0-4ubuntu2~24.04_amd64.deb ...
Unpacking liblsan0:amd64 (14.2.0-4ubuntu2~24.04) ...
Selecting previously unselected package libtsan2:amd64.
Preparing to unpack .../101-libtsan2_14.2.0-4ubuntu2~24.04_amd64.deb ...
Unpacking libtsan2:amd64 (14.2.0-4ubuntu2~24.04) ...
Selecting previously unselected package libubsan1:amd64.
Preparing to unpack .../102-libubsan1_14.2.0-4ubuntu2~24.04_amd64.deb ...
Unpacking libubsan1:amd64 (14.2.0-4ubuntu2~24.04) ...
Selecting previously unselected package libhwasan0:amd64.
Preparing to unpack .../103-libhwasan0_14.2.0-4ubuntu2~24.04_amd64.deb ...
Unpacking libhwasan0:amd64 (14.2.0-4ubuntu2~24.04) ...
Selecting previously unselected package libquadmath0:amd64.
Preparing to unpack .../104-libquadmath0_14.2.0-4ubuntu2~24.04_amd64.deb ...
Unpacking libquadmath0:amd64 (14.2.0-4ubuntu2~24.04) ...
Selecting previously unselected package libgcc-13-dev:amd64.
Preparing to unpack .../105-libgcc-13-dev_13.3.0-6ubuntu2~24.04_amd64.deb ...
Unpacking libgcc-13-dev:amd64 (13.3.0-6ubuntu2~24.04) ...
Selecting previously unselected package gcc-13-x86-64-linux-gnu.
Preparing to unpack .../106-gcc-13-x86-64-linux-gnu_13.3.0-6ubuntu2~24.04_amd64.deb ...
Unpacking gcc-13-x86-64-linux-gnu (13.3.0-6ubuntu2~24.04) ...
Selecting previously unselected package gcc-13.
Preparing to unpack .../107-gcc-13_13.3.0-6ubuntu2~24.04_amd64.deb ...
Unpacking gcc-13 (13.3.0-6ubuntu2~24.04) ...
Selecting previously unselected package gcc-x86-64-linux-gnu.
Preparing to unpack .../108-gcc-x86-64-linux-gnu_4%3a13.2.0-7ubuntu1_amd64.deb ...
Unpacking gcc-x86-64-linux-gnu (4:13.2.0-7ubuntu1) ...
Selecting previously unselected package gcc.
Preparing to unpack .../109-gcc_4%3a13.2.0-7ubuntu1_amd64.deb ...
Unpacking gcc (4:13.2.0-7ubuntu1) ...
Selecting previously unselected package libstdc++-13-dev:amd64.
Preparing to unpack .../110-libstdc++-13-dev_13.3.0-6ubuntu2~24.04_amd64.deb ...
Unpacking libstdc++-13-dev:amd64 (13.3.0-6ubuntu2~24.04) ...
Selecting previously unselected package g++-13-x86-64-linux-gnu.
Preparing to unpack .../111-g++-13-x86-64-linux-gnu_13.3.0-6ubuntu2~24.04_amd64.deb ...
Unpacking g++-13-x86-64-linux-gnu (13.3.0-6ubuntu2~24.04) ...
Selecting previously unselected package g++-13.
Preparing to unpack .../112-g++-13_13.3.0-6ubuntu2~24.04_amd64.deb ...
Unpacking g++-13 (13.3.0-6ubuntu2~24.04) ...
Selecting previously unselected package g++-x86-64-linux-gnu.
Preparing to unpack .../113-g++-x86-64-linux-gnu_4%3a13.2.0-7ubuntu1_amd64.deb ...
Unpacking g++-x86-64-linux-gnu (4:13.2.0-7ubuntu1) ...
Selecting previously unselected package g++.
Preparing to unpack .../114-g++_4%3a13.2.0-7ubuntu1_amd64.deb ...
Unpacking g++ (4:13.2.0-7ubuntu1) ...
Selecting previously unselected package make.
Preparing to unpack .../115-make_4.3-4.1build2_amd64.deb ...
Unpacking make (4.3-4.1build2) ...
Selecting previously unselected package libdpkg-perl.
Preparing to unpack .../116-libdpkg-perl_1.22.6ubuntu6.1_all.deb ...
Unpacking libdpkg-perl (1.22.6ubuntu6.1) ...
Selecting previously unselected package bzip2.
Preparing to unpack .../117-bzip2_1.0.8-5.1build0.1_amd64.deb ...
Unpacking bzip2 (1.0.8-5.1build0.1) ...
Selecting previously unselected package patch.
Preparing to unpack .../118-patch_2.7.6-7build3_amd64.deb ...
Unpacking patch (2.7.6-7build3) ...
Selecting previously unselected package lto-disabled-list.
Preparing to unpack .../119-lto-disabled-list_47_all.deb ...
Unpacking lto-disabled-list (47) ...
Selecting previously unselected package dpkg-dev.
Preparing to unpack .../120-dpkg-dev_1.22.6ubuntu6.1_all.deb ...
Unpacking dpkg-dev (1.22.6ubuntu6.1) ...
Selecting previously unselected package build-essential.
Preparing to unpack .../121-build-essential_12.10ubuntu1_amd64.deb ...
Unpacking build-essential (12.10ubuntu1) ...
Selecting previously unselected package ca-certificates-java.
Preparing to unpack .../122-ca-certificates-java_20240118_all.deb ...
Unpacking ca-certificates-java (20240118) ...
Selecting previously unselected package libhiredis1.1.0:amd64.
Preparing to unpack .../123-libhiredis1.1.0_1.2.0-6ubuntu3_amd64.deb ...
Unpacking libhiredis1.1.0:amd64 (1.2.0-6ubuntu3) ...
Selecting previously unselected package ccache.
Preparing to unpack .../124-ccache_4.9.1-1_amd64.deb ...
Unpacking ccache (4.9.1-1) ...
Selecting previously unselected package dctrl-tools.
Preparing to unpack .../125-dctrl-tools_2.24-3build3_amd64.deb ...
Unpacking dctrl-tools (2.24-3build3) ...
Selecting previously unselected package libdebhelper-perl.
Preparing to unpack .../126-libdebhelper-perl_13.14.1ubuntu5_all.deb ...
Unpacking libdebhelper-perl (13.14.1ubuntu5) ...
Selecting previously unselected package libtool.
Preparing to unpack .../127-libtool_2.4.7-7build1_all.deb ...
Unpacking libtool (2.4.7-7build1) ...
Selecting previously unselected package dh-autoreconf.
Preparing to unpack .../128-dh-autoreconf_20_all.deb ...
Unpacking dh-autoreconf (20) ...
Selecting previously unselected package libarchive-zip-perl.
Preparing to unpack .../129-libarchive-zip-perl_1.68-1_all.deb ...
Unpacking libarchive-zip-perl (1.68-1) ...
Selecting previously unselected package libsub-override-perl.
Preparing to unpack .../130-libsub-override-perl_0.10-1_all.deb ...
Unpacking libsub-override-perl (0.10-1) ...
Selecting previously unselected package libfile-stripnondeterminism-perl.
Preparing to unpack .../131-libfile-stripnondeterminism-perl_1.13.1-1_all.deb ...
Unpacking libfile-stripnondeterminism-perl (1.13.1-1) ...
Selecting previously unselected package dh-strip-nondeterminism.
Preparing to unpack .../132-dh-strip-nondeterminism_1.13.1-1_all.deb ...
Unpacking dh-strip-nondeterminism (1.13.1-1) ...
Selecting previously unselected package libdw1t64:amd64.
Preparing to unpack .../133-libdw1t64_0.190-1.1build4_amd64.deb ...
Unpacking libdw1t64:amd64 (0.190-1.1build4) ...
Selecting previously unselected package debugedit.
Preparing to unpack .../134-debugedit_1%3a5.0-5build2_amd64.deb ...
Unpacking debugedit (1:5.0-5build2) ...
Selecting previously unselected package dwz.
Preparing to unpack .../135-dwz_0.15-1build6_amd64.deb ...
Unpacking dwz (0.15-1build6) ...
Selecting previously unselected package gettext.
Preparing to unpack .../136-gettext_0.21-14ubuntu2_amd64.deb ...
Unpacking gettext (0.21-14ubuntu2) ...
Selecting previously unselected package intltool-debian.
Preparing to unpack .../137-intltool-debian_0.35.0+20060710.6_all.deb ...
Unpacking intltool-debian (0.35.0+20060710.6) ...
Selecting previously unselected package po-debconf.
Preparing to unpack .../138-po-debconf_1.0.21+nmu1_all.deb ...
Unpacking po-debconf (1.0.21+nmu1) ...
Selecting previously unselected package debhelper.
Preparing to unpack .../139-debhelper_13.14.1ubuntu5_all.deb ...
Unpacking debhelper (13.14.1ubuntu5) ...
Selecting previously unselected package java-common.
Preparing to unpack .../140-java-common_0.75+exp1_all.deb ...
Unpacking java-common (0.75+exp1) ...
Selecting previously unselected package liblcms2-2:amd64.
Preparing to unpack .../141-liblcms2-2_2.14-2build1_amd64.deb ...
Unpacking liblcms2-2:amd64 (2.14-2build1) ...
Selecting previously unselected package libjpeg-turbo8:amd64.
Preparing to unpack .../142-libjpeg-turbo8_2.1.5-2ubuntu2_amd64.deb ...
Unpacking libjpeg-turbo8:amd64 (2.1.5-2ubuntu2) ...
Selecting previously unselected package libjpeg8:amd64.
Preparing to unpack .../143-libjpeg8_8c-2ubuntu11_amd64.deb ...
Unpacking libjpeg8:amd64 (8c-2ubuntu11) ...
Selecting previously unselected package libnspr4:amd64.
Preparing to unpack .../144-libnspr4_2%3a4.35-1.1build1_amd64.deb ...
Unpacking libnspr4:amd64 (2:4.35-1.1build1) ...
Selecting previously unselected package libnss3:amd64.
Preparing to unpack .../145-libnss3_2%3a3.98-1build1_amd64.deb ...
Unpacking libnss3:amd64 (2:3.98-1build1) ...
Selecting previously unselected package libpcsclite1:amd64.
Preparing to unpack .../146-libpcsclite1_2.0.3-1build1_amd64.deb ...
Unpacking libpcsclite1:amd64 (2.0.3-1build1) ...
Selecting previously unselected package openjdk-21-jre-headless:amd64.
Preparing to unpack .../147-openjdk-21-jre-headless_21.0.5+11-1ubuntu1~24.04_amd64.deb ...
Unpacking openjdk-21-jre-headless:amd64 (21.0.5+11-1ubuntu1~24.04) ...
Selecting previously unselected package default-jre-headless.
Preparing to unpack .../148-default-jre-headless_2%3a1.21-75+exp1_amd64.deb ...
Unpacking default-jre-headless (2:1.21-75+exp1) ...
Selecting previously unselected package gpgconf.
Preparing to unpack .../149-gpgconf_2.4.4-2ubuntu17_amd64.deb ...
Unpacking gpgconf (2.4.4-2ubuntu17) ...
Selecting previously unselected package libksba8:amd64.
Preparing to unpack .../150-libksba8_1.6.6-1build1_amd64.deb ...
Unpacking libksba8:amd64 (1.6.6-1build1) ...
Selecting previously unselected package libsasl2-modules-db:amd64.
Preparing to unpack .../151-libsasl2-modules-db_2.1.28+dfsg1-5ubuntu3.1_amd64.deb ...
Unpacking libsasl2-modules-db:amd64 (2.1.28+dfsg1-5ubuntu3.1) ...
Selecting previously unselected package libsasl2-2:amd64.
Preparing to unpack .../152-libsasl2-2_2.1.28+dfsg1-5ubuntu3.1_amd64.deb ...
Unpacking libsasl2-2:amd64 (2.1.28+dfsg1-5ubuntu3.1) ...
Selecting previously unselected package libldap2:amd64.
Preparing to unpack .../153-libldap2_2.6.7+dfsg-1~exp1ubuntu8.1_amd64.deb ...
Unpacking libldap2:amd64 (2.6.7+dfsg-1~exp1ubuntu8.1) ...
Selecting previously unselected package dirmngr.
Preparing to unpack .../154-dirmngr_2.4.4-2ubuntu17_amd64.deb ...
Unpacking dirmngr (2.4.4-2ubuntu17) ...
Selecting previously unselected package gnupg-utils.
Preparing to unpack .../155-gnupg-utils_2.4.4-2ubuntu17_amd64.deb ...
Unpacking gnupg-utils (2.4.4-2ubuntu17) ...
Selecting previously unselected package gpg.
Preparing to unpack .../156-gpg_2.4.4-2ubuntu17_amd64.deb ...
Unpacking gpg (2.4.4-2ubuntu17) ...
Selecting previously unselected package pinentry-curses.
Preparing to unpack .../157-pinentry-curses_1.2.1-3ubuntu5_amd64.deb ...
Unpacking pinentry-curses (1.2.1-3ubuntu5) ...
Selecting previously unselected package gpg-agent.
Preparing to unpack .../158-gpg-agent_2.4.4-2ubuntu17_amd64.deb ...
Unpacking gpg-agent (2.4.4-2ubuntu17) ...
Selecting previously unselected package gpgsm.
Preparing to unpack .../159-gpgsm_2.4.4-2ubuntu17_amd64.deb ...
Unpacking gpgsm (2.4.4-2ubuntu17) ...
Selecting previously unselected package keyboxd.
Preparing to unpack .../160-keyboxd_2.4.4-2ubuntu17_amd64.deb ...
Unpacking keyboxd (2.4.4-2ubuntu17) ...
Selecting previously unselected package gnupg.
Preparing to unpack .../161-gnupg_2.4.4-2ubuntu17_all.deb ...
Unpacking gnupg (2.4.4-2ubuntu17) ...
Selecting previously unselected package libfile-dirlist-perl.
Preparing to unpack .../162-libfile-dirlist-perl_0.05-3_all.deb ...
Unpacking libfile-dirlist-perl (0.05-3) ...
Selecting previously unselected package libfile-which-perl.
Preparing to unpack .../163-libfile-which-perl_1.27-2_all.deb ...
Unpacking libfile-which-perl (1.27-2) ...
Selecting previously unselected package libfile-homedir-perl.
Preparing to unpack .../164-libfile-homedir-perl_1.006-2_all.deb ...
Unpacking libfile-homedir-perl (1.006-2) ...
Selecting previously unselected package libfile-touch-perl.
Preparing to unpack .../165-libfile-touch-perl_0.12-2_all.deb ...
Unpacking libfile-touch-perl (0.12-2) ...
Selecting previously unselected package libio-pty-perl.
Preparing to unpack .../166-libio-pty-perl_1%3a1.20-1build2_amd64.deb ...
Unpacking libio-pty-perl (1:1.20-1build2) ...
Selecting previously unselected package libipc-run-perl.
Preparing to unpack .../167-libipc-run-perl_20231003.0-1_all.deb ...
Unpacking libipc-run-perl (20231003.0-1) ...
Selecting previously unselected package libclass-method-modifiers-perl.
Preparing to unpack .../168-libclass-method-modifiers-perl_2.15-1_all.deb ...
Unpacking libclass-method-modifiers-perl (2.15-1) ...
Selecting previously unselected package libclass-xsaccessor-perl.
Preparing to unpack .../169-libclass-xsaccessor-perl_1.19-4build4_amd64.deb ...
Unpacking libclass-xsaccessor-perl (1.19-4build4) ...
Selecting previously unselected package libb-hooks-op-check-perl:amd64.
Preparing to unpack .../170-libb-hooks-op-check-perl_0.22-3build1_amd64.deb ...
Unpacking libb-hooks-op-check-perl:amd64 (0.22-3build1) ...
Selecting previously unselected package libdynaloader-functions-perl.
Preparing to unpack .../171-libdynaloader-functions-perl_0.003-3_all.deb ...
Unpacking libdynaloader-functions-perl (0.003-3) ...
Selecting previously unselected package libdevel-callchecker-perl:amd64.
Preparing to unpack .../172-libdevel-callchecker-perl_0.008-2build3_amd64.deb ...
Unpacking libdevel-callchecker-perl:amd64 (0.008-2build3) ...
Selecting previously unselected package libparams-classify-perl:amd64.
Preparing to unpack .../173-libparams-classify-perl_0.015-2build5_amd64.deb ...
Unpacking libparams-classify-perl:amd64 (0.015-2build5) ...
Selecting previously unselected package libmodule-runtime-perl.
Preparing to unpack .../174-libmodule-runtime-perl_0.016-2_all.deb ...
Unpacking libmodule-runtime-perl (0.016-2) ...
Selecting previously unselected package libimport-into-perl.
Preparing to unpack .../175-libimport-into-perl_1.002005-2_all.deb ...
Unpacking libimport-into-perl (1.002005-2) ...
Selecting previously unselected package librole-tiny-perl.
Preparing to unpack .../176-librole-tiny-perl_2.002004-1_all.deb ...
Unpacking librole-tiny-perl (2.002004-1) ...
Selecting previously unselected package libsub-quote-perl.
Preparing to unpack .../177-libsub-quote-perl_2.006008-1ubuntu1_all.deb ...
Unpacking libsub-quote-perl (2.006008-1ubuntu1) ...
Selecting previously unselected package libmoo-perl.
Preparing to unpack .../178-libmoo-perl_2.005005-1_all.deb ...
Unpacking libmoo-perl (2.005005-1) ...
Selecting previously unselected package libencode-locale-perl.
Preparing to unpack .../179-libencode-locale-perl_1.05-3_all.deb ...
Unpacking libencode-locale-perl (1.05-3) ...
Selecting previously unselected package libtimedate-perl.
Preparing to unpack .../180-libtimedate-perl_2.3300-2_all.deb ...
Unpacking libtimedate-perl (2.3300-2) ...
Selecting previously unselected package libhttp-date-perl.
Preparing to unpack .../181-libhttp-date-perl_6.06-1_all.deb ...
Unpacking libhttp-date-perl (6.06-1) ...
Selecting previously unselected package libfile-listing-perl.
Preparing to unpack .../182-libfile-listing-perl_6.16-1_all.deb ...
Unpacking libfile-listing-perl (6.16-1) ...
Selecting previously unselected package libhtml-tagset-perl.
Preparing to unpack .../183-libhtml-tagset-perl_3.20-6_all.deb ...
Unpacking libhtml-tagset-perl (3.20-6) ...
Selecting previously unselected package liburi-perl.
Preparing to unpack .../184-liburi-perl_5.27-1_all.deb ...
Unpacking liburi-perl (5.27-1) ...
Selecting previously unselected package libhtml-parser-perl:amd64.
Preparing to unpack .../185-libhtml-parser-perl_3.81-1build3_amd64.deb ...
Unpacking libhtml-parser-perl:amd64 (3.81-1build3) ...
Selecting previously unselected package libhtml-tree-perl.
Preparing to unpack .../186-libhtml-tree-perl_5.07-3_all.deb ...
Unpacking libhtml-tree-perl (5.07-3) ...
Selecting previously unselected package libclone-perl:amd64.
Preparing to unpack .../187-libclone-perl_0.46-1build3_amd64.deb ...
Unpacking libclone-perl:amd64 (0.46-1build3) ...
Selecting previously unselected package libio-html-perl.
Preparing to unpack .../188-libio-html-perl_1.004-3_all.deb ...
Unpacking libio-html-perl (1.004-3) ...
Selecting previously unselected package liblwp-mediatypes-perl.
Preparing to unpack .../189-liblwp-mediatypes-perl_6.04-2_all.deb ...
Unpacking liblwp-mediatypes-perl (6.04-2) ...
Selecting previously unselected package libhttp-message-perl.
Preparing to unpack .../190-libhttp-message-perl_6.45-1ubuntu1_all.deb ...
Unpacking libhttp-message-perl (6.45-1ubuntu1) ...
Selecting previously unselected package libhttp-cookies-perl.
Preparing to unpack .../191-libhttp-cookies-perl_6.11-1_all.deb ...
Unpacking libhttp-cookies-perl (6.11-1) ...
Selecting previously unselected package libhttp-negotiate-perl.
Preparing to unpack .../192-libhttp-negotiate-perl_6.01-2_all.deb ...
Unpacking libhttp-negotiate-perl (6.01-2) ...
Selecting previously unselected package perl-openssl-defaults:amd64.
Preparing to unpack .../193-perl-openssl-defaults_7build3_amd64.deb ...
Unpacking perl-openssl-defaults:amd64 (7build3) ...
Selecting previously unselected package libnet-ssleay-perl:amd64.
Preparing to unpack .../194-libnet-ssleay-perl_1.94-1build4_amd64.deb ...
Unpacking libnet-ssleay-perl:amd64 (1.94-1build4) ...
Selecting previously unselected package libio-socket-ssl-perl.
Preparing to unpack .../195-libio-socket-ssl-perl_2.085-1_all.deb ...
Unpacking libio-socket-ssl-perl (2.085-1) ...
Selecting previously unselected package libnet-http-perl.
Preparing to unpack .../196-libnet-http-perl_6.23-1_all.deb ...
Unpacking libnet-http-perl (6.23-1) ...
Selecting previously unselected package liblwp-protocol-https-perl.
Preparing to unpack .../197-liblwp-protocol-https-perl_6.13-1_all.deb ...
Unpacking liblwp-protocol-https-perl (6.13-1) ...
Selecting previously unselected package libtry-tiny-perl.
Preparing to unpack .../198-libtry-tiny-perl_0.31-2_all.deb ...
Unpacking libtry-tiny-perl (0.31-2) ...
Selecting previously unselected package libwww-robotrules-perl.
Preparing to unpack .../199-libwww-robotrules-perl_6.02-1_all.deb ...
Unpacking libwww-robotrules-perl (6.02-1) ...
Selecting previously unselected package libwww-perl.
Preparing to unpack .../200-libwww-perl_6.76-1_all.deb ...
Unpacking libwww-perl (6.76-1) ...
Selecting previously unselected package patchutils.
Preparing to unpack .../201-patchutils_0.4.2-1build3_amd64.deb ...
Unpacking patchutils (0.4.2-1build3) ...
Selecting previously unselected package wdiff.
Preparing to unpack .../202-wdiff_1.2.2-6build1_amd64.deb ...
Unpacking wdiff (1.2.2-6build1) ...
Selecting previously unselected package devscripts.
Preparing to unpack .../203-devscripts_2.23.7_all.deb ...
Unpacking devscripts (2.23.7) ...
Selecting previously unselected package diffstat.
Preparing to unpack .../204-diffstat_1.66-1build1_amd64.deb ...
Unpacking diffstat (1.66-1build1) ...
Selecting previously unselected package python3-chardet.
Preparing to unpack .../205-python3-chardet_5.2.0+dfsg-1_all.deb ...
Unpacking python3-chardet (5.2.0+dfsg-1) ...
Selecting previously unselected package zstd.
Preparing to unpack .../206-zstd_1.5.5+dfsg2-2build1.1_amd64.deb ...
Unpacking zstd (1.5.5+dfsg2-2build1.1) ...
Selecting previously unselected package python3-debian.
Preparing to unpack .../207-python3-debian_0.1.49ubuntu2_all.deb ...
Unpacking python3-debian (0.1.49ubuntu2) ...
Selecting previously unselected package libgpgme11t64:amd64.
Preparing to unpack .../208-libgpgme11t64_1.18.0-4.1ubuntu4_amd64.deb ...
Unpacking libgpgme11t64:amd64 (1.18.0-4.1ubuntu4) ...
Selecting previously unselected package python3-gpg.
Preparing to unpack .../209-python3-gpg_1.18.0-4.1ubuntu4_amd64.deb ...
Unpacking python3-gpg (1.18.0-4.1ubuntu4) ...
Selecting previously unselected package python3-xdg.
Preparing to unpack .../210-python3-xdg_0.28-2_all.deb ...
Unpacking python3-xdg (0.28-2) ...
Selecting previously unselected package dput.
Preparing to unpack .../211-dput_1.1.3ubuntu3_all.deb ...
Unpacking dput (1.1.3ubuntu3) ...
Selecting previously unselected package libeclipse-jdt-core-java.
Preparing to unpack .../212-libeclipse-jdt-core-java_3.32.0+eclipse4.26-2_all.deb ...
Unpacking libeclipse-jdt-core-java (3.32.0+eclipse4.26-2) ...
Selecting previously unselected package unzip.
Preparing to unpack .../213-unzip_6.0-28ubuntu4.1_amd64.deb ...
Unpacking unzip (6.0-28ubuntu4.1) ...
Selecting previously unselected package java-wrappers.
Preparing to unpack .../214-java-wrappers_0.4_all.deb ...
Unpacking java-wrappers (0.4) ...
Selecting previously unselected package ecj.
Preparing to unpack .../215-ecj_3.32.0+eclipse4.26-2_all.deb ...
Unpacking ecj (3.32.0+eclipse4.26-2) ...
Selecting previously unselected package libfakeroot:amd64.
Preparing to unpack .../216-libfakeroot_1.33-1_amd64.deb ...
Unpacking libfakeroot:amd64 (1.33-1) ...
Selecting previously unselected package fakeroot.
Preparing to unpack .../217-fakeroot_1.33-1_amd64.deb ...
Unpacking fakeroot (1.33-1) ...
Selecting previously unselected package fonts-dejavu-mono.
Preparing to unpack .../218-fonts-dejavu-mono_2.37-8_all.deb ...
Unpacking fonts-dejavu-mono (2.37-8) ...
Selecting previously unselected package fonts-dejavu-core.
Preparing to unpack .../219-fonts-dejavu-core_2.37-8_all.deb ...
Unpacking fonts-dejavu-core (2.37-8) ...
Selecting previously unselected package fontconfig-config.
Preparing to unpack .../220-fontconfig-config_2.15.0-1.1ubuntu2_amd64.deb ...
Unpacking fontconfig-config (2.15.0-1.1ubuntu2) ...
Selecting previously unselected package libbrotli1:amd64.
Preparing to unpack .../221-libbrotli1_1.1.0-2build2_amd64.deb ...
Unpacking libbrotli1:amd64 (1.1.0-2build2) ...
Selecting previously unselected package librtmp1:amd64.
Preparing to unpack .../222-librtmp1_2.4+20151223.gitfa8646d.1-2build7_amd64.deb ...
Unpacking librtmp1:amd64 (2.4+20151223.gitfa8646d.1-2build7) ...
Selecting previously unselected package libssh-4:amd64.
Preparing to unpack .../223-libssh-4_0.10.6-2build2_amd64.deb ...
Unpacking libssh-4:amd64 (0.10.6-2build2) ...
Selecting previously unselected package libcurl3t64-gnutls:amd64.
Preparing to unpack .../224-libcurl3t64-gnutls_8.5.0-2ubuntu10.6_amd64.deb ...
Unpacking libcurl3t64-gnutls:amd64 (8.5.0-2ubuntu10.6) ...
Selecting previously unselected package liberror-perl.
Preparing to unpack .../225-liberror-perl_0.17029-2_all.deb ...
Unpacking liberror-perl (0.17029-2) ...
Selecting previously unselected package git-man.
Preparing to unpack .../226-git-man_1%3a2.43.0-1ubuntu7.2_all.deb ...
Unpacking git-man (1:2.43.0-1ubuntu7.2) ...
Selecting previously unselected package git.
Preparing to unpack .../227-git_1%3a2.43.0-1ubuntu7.2_amd64.deb ...
Unpacking git (1:2.43.0-1ubuntu7.2) ...
Selecting previously unselected package gnupg-l10n.
Preparing to unpack .../228-gnupg-l10n_2.4.4-2ubuntu17_all.deb ...
Unpacking gnupg-l10n (2.4.4-2ubuntu17) ...
Selecting previously unselected package gpg-wks-client.
Preparing to unpack .../229-gpg-wks-client_2.4.4-2ubuntu17_amd64.deb ...
Unpacking gpg-wks-client (2.4.4-2ubuntu17) ...
Selecting previously unselected package fastjar.
Preparing to unpack .../230-fastjar_2%3a0.98-7_amd64.deb ...
Unpacking fastjar (2:0.98-7) ...
Selecting previously unselected package jarwrapper.
Preparing to unpack .../231-jarwrapper_0.79_all.deb ...
Unpacking jarwrapper (0.79) ...
Selecting previously unselected package javahelper.
Preparing to unpack .../232-javahelper_0.79_all.deb ...
Unpacking javahelper (0.79) ...
Selecting previously unselected package java-propose-classpath.
Preparing to unpack .../233-java-propose-classpath_0.79_all.deb ...
Unpacking java-propose-classpath (0.79) ...
Selecting previously unselected package javascript-common.
Preparing to unpack .../234-javascript-common_11+nmu1_all.deb ...
Unpacking javascript-common (11+nmu1) ...
Selecting previously unselected package libalgorithm-diff-perl.
Preparing to unpack .../235-libalgorithm-diff-perl_1.201-1_all.deb ...
Unpacking libalgorithm-diff-perl (1.201-1) ...
Selecting previously unselected package libalgorithm-diff-xs-perl:amd64.
Preparing to unpack .../236-libalgorithm-diff-xs-perl_0.04-8build3_amd64.deb ...
Unpacking libalgorithm-diff-xs-perl:amd64 (0.04-8build3) ...
Selecting previously unselected package libalgorithm-merge-perl.
Preparing to unpack .../237-libalgorithm-merge-perl_0.08-5_all.deb ...
Unpacking libalgorithm-merge-perl (0.08-5) ...
Selecting previously unselected package libaliased-perl.
Preparing to unpack .../238-libaliased-perl_0.34-3_all.deb ...
Unpacking libaliased-perl (0.34-3) ...
Selecting previously unselected package libaom3:amd64.
Preparing to unpack .../239-libaom3_3.8.2-2ubuntu0.1_amd64.deb ...
Unpacking libaom3:amd64 (3.8.2-2ubuntu0.1) ...
Selecting previously unselected package libapr1t64:amd64.
Preparing to unpack .../240-libapr1t64_1.7.2-3.1ubuntu0.1_amd64.deb ...
Unpacking libapr1t64:amd64 (1.7.2-3.1ubuntu0.1) ...
Selecting previously unselected package libaprutil1t64:amd64.
Preparing to unpack .../241-libaprutil1t64_1.6.3-1.1ubuntu7_amd64.deb ...
Unpacking libaprutil1t64:amd64 (1.6.3-1.1ubuntu7) ...
Selecting previously unselected package libapt-pkg-perl.
Preparing to unpack .../242-libapt-pkg-perl_0.1.40build7_amd64.deb ...
Unpacking libapt-pkg-perl (0.1.40build7) ...
Selecting previously unselected package libarchive-cpio-perl.
Preparing to unpack .../243-libarchive-cpio-perl_0.10-3_all.deb ...
Unpacking libarchive-cpio-perl (0.10-3) ...
Selecting previously unselected package libarray-intspan-perl.
Preparing to unpack .../244-libarray-intspan-perl_2.004-2_all.deb ...
Unpacking libarray-intspan-perl (2.004-2) ...
Selecting previously unselected package libavahi-common-data:amd64.
Preparing to unpack .../245-libavahi-common-data_0.8-13ubuntu6_amd64.deb ...
Unpacking libavahi-common-data:amd64 (0.8-13ubuntu6) ...
Selecting previously unselected package libavahi-common3:amd64.
Preparing to unpack .../246-libavahi-common3_0.8-13ubuntu6_amd64.deb ...
Unpacking libavahi-common3:amd64 (0.8-13ubuntu6) ...
Selecting previously unselected package libavahi-client3:amd64.
Preparing to unpack .../247-libavahi-client3_0.8-13ubuntu6_amd64.deb ...
Unpacking libavahi-client3:amd64 (0.8-13ubuntu6) ...
Selecting previously unselected package libmodule-implementation-perl.
Preparing to unpack .../248-libmodule-implementation-perl_0.09-2_all.deb ...
Unpacking libmodule-implementation-perl (0.09-2) ...
Selecting previously unselected package libsub-exporter-progressive-perl.
Preparing to unpack .../249-libsub-exporter-progressive-perl_0.001013-3_all.deb ...
Unpacking libsub-exporter-progressive-perl (0.001013-3) ...
Selecting previously unselected package libvariable-magic-perl.
Preparing to unpack .../250-libvariable-magic-perl_0.63-1build3_amd64.deb ...
Unpacking libvariable-magic-perl (0.63-1build3) ...
Selecting previously unselected package libb-hooks-endofscope-perl.
Preparing to unpack .../251-libb-hooks-endofscope-perl_0.28-1_all.deb ...
Unpacking libb-hooks-endofscope-perl (0.28-1) ...
Selecting previously unselected package libberkeleydb-perl:amd64.
Preparing to unpack .../252-libberkeleydb-perl_0.64-2build4_amd64.deb ...
Unpacking libberkeleydb-perl:amd64 (0.64-2build4) ...
Selecting previously unselected package libfreetype6:amd64.
Preparing to unpack .../253-libfreetype6_2.13.2+dfsg-1build3_amd64.deb ...
Unpacking libfreetype6:amd64 (2.13.2+dfsg-1build3) ...
Selecting previously unselected package libfontconfig1:amd64.
Preparing to unpack .../254-libfontconfig1_2.15.0-1.1ubuntu2_amd64.deb ...
Unpacking libfontconfig1:amd64 (2.15.0-1.1ubuntu2) ...
Selecting previously unselected package libsharpyuv0:amd64.
Preparing to unpack .../255-libsharpyuv0_1.3.2-0.4build3_amd64.deb ...
Unpacking libsharpyuv0:amd64 (1.3.2-0.4build3) ...
Selecting previously unselected package libheif-plugin-aomdec:amd64.
Preparing to unpack .../256-libheif-plugin-aomdec_1.17.6-1ubuntu4.1_amd64.deb ...
Unpacking libheif-plugin-aomdec:amd64 (1.17.6-1ubuntu4.1) ...
Selecting previously unselected package libde265-0:amd64.
Preparing to unpack .../257-libde265-0_1.0.15-1build3_amd64.deb ...
Unpacking libde265-0:amd64 (1.0.15-1build3) ...
Selecting previously unselected package libheif-plugin-libde265:amd64.
Preparing to unpack .../258-libheif-plugin-libde265_1.17.6-1ubuntu4.1_amd64.deb ...
Unpacking libheif-plugin-libde265:amd64 (1.17.6-1ubuntu4.1) ...
Selecting previously unselected package libheif1:amd64.
Preparing to unpack .../259-libheif1_1.17.6-1ubuntu4.1_amd64.deb ...
Unpacking libheif1:amd64 (1.17.6-1ubuntu4.1) ...
Selecting previously unselected package libdeflate0:amd64.
Preparing to unpack .../260-libdeflate0_1.19-1build1.1_amd64.deb ...
Unpacking libdeflate0:amd64 (1.19-1build1.1) ...
Selecting previously unselected package libjbig0:amd64.
Preparing to unpack .../261-libjbig0_2.1-6.1ubuntu2_amd64.deb ...
Unpacking libjbig0:amd64 (2.1-6.1ubuntu2) ...
Selecting previously unselected package liblerc4:amd64.
Preparing to unpack .../262-liblerc4_4.0.0+ds-4ubuntu2_amd64.deb ...
Unpacking liblerc4:amd64 (4.0.0+ds-4ubuntu2) ...
Selecting previously unselected package libwebp7:amd64.
Preparing to unpack .../263-libwebp7_1.3.2-0.4build3_amd64.deb ...
Unpacking libwebp7:amd64 (1.3.2-0.4build3) ...
Selecting previously unselected package libtiff6:amd64.
Preparing to unpack .../264-libtiff6_4.5.1+git230720-4ubuntu2.2_amd64.deb ...
Unpacking libtiff6:amd64 (4.5.1+git230720-4ubuntu2.2) ...
Selecting previously unselected package libxpm4:amd64.
Preparing to unpack .../265-libxpm4_1%3a3.5.17-1build2_amd64.deb ...
Unpacking libxpm4:amd64 (1:3.5.17-1build2) ...
Selecting previously unselected package libgd3:amd64.
Preparing to unpack .../266-libgd3_2.3.3-9ubuntu5_amd64.deb ...
Unpacking libgd3:amd64 (2.3.3-9ubuntu5) ...
Selecting previously unselected package libc-devtools.
Preparing to unpack .../267-libc-devtools_2.39-0ubuntu8.3_amd64.deb ...
Unpacking libc-devtools (2.39-0ubuntu8.3) ...
Selecting previously unselected package libcapture-tiny-perl.
Preparing to unpack .../268-libcapture-tiny-perl_0.48-2_all.deb ...
Unpacking libcapture-tiny-perl (0.48-2) ...
Selecting previously unselected package libcgi-pm-perl.
Preparing to unpack .../269-libcgi-pm-perl_4.63-1_all.deb ...
Unpacking libcgi-pm-perl (4.63-1) ...
Selecting previously unselected package libfcgi0t64:amd64.
Preparing to unpack .../270-libfcgi0t64_2.4.2-2.1build1_amd64.deb ...
Unpacking libfcgi0t64:amd64 (2.4.2-2.1build1) ...
Selecting previously unselected package libfcgi-perl.
Preparing to unpack .../271-libfcgi-perl_0.82+ds-3build2_amd64.deb ...
Unpacking libfcgi-perl (0.82+ds-3build2) ...
Selecting previously unselected package libcgi-fast-perl.
Preparing to unpack .../272-libcgi-fast-perl_1%3a2.17-1_all.deb ...
Unpacking libcgi-fast-perl (1:2.17-1) ...
Selecting previously unselected package libclass-data-inheritable-perl.
Preparing to unpack .../273-libclass-data-inheritable-perl_0.08-3_all.deb ...
Unpacking libclass-data-inheritable-perl (0.08-3) ...
Selecting previously unselected package libcommon-sense-perl:amd64.
Preparing to unpack .../274-libcommon-sense-perl_3.75-3build3_amd64.deb ...
Unpacking libcommon-sense-perl:amd64 (3.75-3build3) ...
Selecting previously unselected package libconfig-tiny-perl.
Preparing to unpack .../275-libconfig-tiny-perl_2.30-1_all.deb ...
Unpacking libconfig-tiny-perl (2.30-1) ...
Selecting previously unselected package libparams-util-perl.
Preparing to unpack .../276-libparams-util-perl_1.102-2build3_amd64.deb ...
Unpacking libparams-util-perl (1.102-2build3) ...
Selecting previously unselected package libsub-install-perl.
Preparing to unpack .../277-libsub-install-perl_0.929-1_all.deb ...
Unpacking libsub-install-perl (0.929-1) ...
Selecting previously unselected package libdata-optlist-perl.
Preparing to unpack .../278-libdata-optlist-perl_0.114-1_all.deb ...
Unpacking libdata-optlist-perl (0.114-1) ...
Selecting previously unselected package libsub-exporter-perl.
Preparing to unpack .../279-libsub-exporter-perl_0.990-1_all.deb ...
Unpacking libsub-exporter-perl (0.990-1) ...
Selecting previously unselected package libconst-fast-perl.
Preparing to unpack .../280-libconst-fast-perl_0.014-2_all.deb ...
Unpacking libconst-fast-perl (0.014-2) ...
Selecting previously unselected package libcpanel-json-xs-perl:amd64.
Preparing to unpack .../281-libcpanel-json-xs-perl_4.37-1build3_amd64.deb ...
Unpacking libcpanel-json-xs-perl:amd64 (4.37-1build3) ...
Selecting previously unselected package libcups2t64:amd64.
Preparing to unpack .../282-libcups2t64_2.4.7-1.2ubuntu7.3_amd64.deb ...
Unpacking libcups2t64:amd64 (2.4.7-1.2ubuntu7.3) ...
Selecting previously unselected package libdevel-stacktrace-perl.
Preparing to unpack .../283-libdevel-stacktrace-perl_2.0500-1_all.deb ...
Unpacking libdevel-stacktrace-perl (2.0500-1) ...
Selecting previously unselected package libexception-class-perl.
Preparing to unpack .../284-libexception-class-perl_1.45-1_all.deb ...
Unpacking libexception-class-perl (1.45-1) ...
Selecting previously unselected package libiterator-perl.
Preparing to unpack .../285-libiterator-perl_0.03+ds1-2_all.deb ...
Unpacking libiterator-perl (0.03+ds1-2) ...
Selecting previously unselected package libiterator-util-perl.
Preparing to unpack .../286-libiterator-util-perl_0.02+ds1-2_all.deb ...
Unpacking libiterator-util-perl (0.02+ds1-2) ...
Selecting previously unselected package libdata-dpath-perl.
Preparing to unpack .../287-libdata-dpath-perl_0.59-1_all.deb ...
Unpacking libdata-dpath-perl (0.59-1) ...
Selecting previously unselected package libdata-dump-perl.
Preparing to unpack .../288-libdata-dump-perl_1.25-1_all.deb ...
Unpacking libdata-dump-perl (1.25-1) ...
Selecting previously unselected package libdata-messagepack-perl.
Preparing to unpack .../289-libdata-messagepack-perl_1.02-1build4_amd64.deb ...
Unpacking libdata-messagepack-perl (1.02-1build4) ...
Selecting previously unselected package libnet-domain-tld-perl.
Preparing to unpack .../290-libnet-domain-tld-perl_1.75-3_all.deb ...
Unpacking libnet-domain-tld-perl (1.75-3) ...
Selecting previously unselected package libdata-validate-domain-perl.
Preparing to unpack .../291-libdata-validate-domain-perl_0.10-1.1_all.deb ...
Unpacking libdata-validate-domain-perl (0.10-1.1) ...
Selecting previously unselected package libnet-ipv6addr-perl.
Preparing to unpack .../292-libnet-ipv6addr-perl_1.02-1_all.deb ...
Unpacking libnet-ipv6addr-perl (1.02-1) ...
Selecting previously unselected package libnet-netmask-perl.
Preparing to unpack .../293-libnet-netmask-perl_2.0002-2_all.deb ...
Unpacking libnet-netmask-perl (2.0002-2) ...
Selecting previously unselected package libnetaddr-ip-perl.
Preparing to unpack .../294-libnetaddr-ip-perl_4.079+dfsg-2build4_amd64.deb ...
Unpacking libnetaddr-ip-perl (4.079+dfsg-2build4) ...
Selecting previously unselected package libdata-validate-ip-perl.
Preparing to unpack .../295-libdata-validate-ip-perl_0.31-1_all.deb ...
Unpacking libdata-validate-ip-perl (0.31-1) ...
Selecting previously unselected package libdata-validate-uri-perl.
Preparing to unpack .../296-libdata-validate-uri-perl_0.07-3_all.deb ...
Unpacking libdata-validate-uri-perl (0.07-3) ...
Selecting previously unselected package libdistro-info-perl.
Preparing to unpack .../297-libdistro-info-perl_1.7build1_all.deb ...
Unpacking libdistro-info-perl (1.7build1) ...
Selecting previously unselected package zlib1g-dev:amd64.
Preparing to unpack .../298-zlib1g-dev_1%3a1.3.dfsg-3.1ubuntu2.1_amd64.deb ...
Unpacking zlib1g-dev:amd64 (1:1.3.dfsg-3.1ubuntu2.1) ...
Selecting previously unselected package libzstd-dev:amd64.
Preparing to unpack .../299-libzstd-dev_1.5.5+dfsg2-2build1.1_amd64.deb ...
Unpacking libzstd-dev:amd64 (1.5.5+dfsg2-2build1.1) ...
Selecting previously unselected package libelf-dev:amd64.
Preparing to unpack .../300-libelf-dev_0.190-1.1build4_amd64.deb ...
Unpacking libelf-dev:amd64 (0.190-1.1build4) ...
Selecting previously unselected package libemail-address-xs-perl.
Preparing to unpack .../301-libemail-address-xs-perl_1.05-1build4_amd64.deb ...
Unpacking libemail-address-xs-perl (1.05-1build4) ...
Selecting previously unselected package libexpat1-dev:amd64.
Preparing to unpack .../302-libexpat1-dev_2.6.1-2ubuntu0.2_amd64.deb ...
Unpacking libexpat1-dev:amd64 (2.6.1-2ubuntu0.2) ...
Selecting previously unselected package libexporter-tiny-perl.
Preparing to unpack .../303-libexporter-tiny-perl_1.006002-1_all.deb ...
Unpacking libexporter-tiny-perl (1.006002-1) ...
Selecting previously unselected package libfcgi-bin.
Preparing to unpack .../304-libfcgi-bin_2.4.2-2.1build1_amd64.deb ...
Unpacking libfcgi-bin (2.4.2-2.1build1) ...
Selecting previously unselected package libipc-system-simple-perl.
Preparing to unpack .../305-libipc-system-simple-perl_1.30-2_all.deb ...
Unpacking libipc-system-simple-perl (1.30-2) ...
Selecting previously unselected package libfile-basedir-perl.
Preparing to unpack .../306-libfile-basedir-perl_0.09-2_all.deb ...
Unpacking libfile-basedir-perl (0.09-2) ...
Selecting previously unselected package libfile-chdir-perl.
Preparing to unpack .../307-libfile-chdir-perl_0.1008-1.1_all.deb ...
Unpacking libfile-chdir-perl (0.1008-1.1) ...
Selecting previously unselected package libfile-fcntllock-perl.
Preparing to unpack .../308-libfile-fcntllock-perl_0.22-4ubuntu5_amd64.deb ...
Unpacking libfile-fcntllock-perl (0.22-4ubuntu5) ...
Selecting previously unselected package libnumber-compare-perl.
Preparing to unpack .../309-libnumber-compare-perl_0.03-3_all.deb ...
Unpacking libnumber-compare-perl (0.03-3) ...
Selecting previously unselected package libtext-glob-perl.
Preparing to unpack .../310-libtext-glob-perl_0.11-3_all.deb ...
Unpacking libtext-glob-perl (0.11-3) ...
Selecting previously unselected package libfile-find-rule-perl.
Preparing to unpack .../311-libfile-find-rule-perl_0.34-3_all.deb ...
Unpacking libfile-find-rule-perl (0.34-3) ...
Selecting previously unselected package libfont-afm-perl.
Preparing to unpack .../312-libfont-afm-perl_1.20-4_all.deb ...
Unpacking libfont-afm-perl (1.20-4) ...
Selecting previously unselected package libio-string-perl.
Preparing to unpack .../313-libio-string-perl_1.08-4_all.deb ...
Unpacking libio-string-perl (1.08-4) ...
Selecting previously unselected package libfont-ttf-perl.
Preparing to unpack .../314-libfont-ttf-perl_1.06-2_all.deb ...
Unpacking libfont-ttf-perl (1.06-2) ...
Selecting previously unselected package libfreezethaw-perl.
Preparing to unpack .../315-libfreezethaw-perl_0.5001-3_all.deb ...
Unpacking libfreezethaw-perl (0.5001-3) ...
Selecting previously unselected package libsort-versions-perl.
Preparing to unpack .../316-libsort-versions-perl_1.62-3_all.deb ...
Unpacking libsort-versions-perl (1.62-3) ...
Selecting previously unselected package libgit-wrapper-perl.
Preparing to unpack .../317-libgit-wrapper-perl_0.048-2_all.deb ...
Unpacking libgit-wrapper-perl (0.048-2) ...
Selecting previously unselected package libgraphite2-3:amd64.
Preparing to unpack .../318-libgraphite2-3_1.3.14-2build1_amd64.deb ...
Unpacking libgraphite2-3:amd64 (1.3.14-2build1) ...
Selecting previously unselected package libharfbuzz0b:amd64.
Preparing to unpack .../319-libharfbuzz0b_8.3.0-2build2_amd64.deb ...
Unpacking libharfbuzz0b:amd64 (8.3.0-2build2) ...
Selecting previously unselected package libheif-plugin-aomenc:amd64.
Preparing to unpack .../320-libheif-plugin-aomenc_1.17.6-1ubuntu4.1_amd64.deb ...
Unpacking libheif-plugin-aomenc:amd64 (1.17.6-1ubuntu4.1) ...
Selecting previously unselected package libhtml-form-perl.
Preparing to unpack .../321-libhtml-form-perl_6.11-1_all.deb ...
Unpacking libhtml-form-perl (6.11-1) ...
Selecting previously unselected package libhtml-format-perl.
Preparing to unpack .../322-libhtml-format-perl_2.16-2_all.deb ...
Unpacking libhtml-format-perl (2.16-2) ...
Selecting previously unselected package libhtml-html5-entities-perl.
Preparing to unpack .../323-libhtml-html5-entities-perl_0.004-3_all.deb ...
Unpacking libhtml-html5-entities-perl (0.004-3) ...
Selecting previously unselected package libhtml-tokeparser-simple-perl.
Preparing to unpack .../324-libhtml-tokeparser-simple-perl_3.16-4_all.deb ...
Unpacking libhtml-tokeparser-simple-perl (3.16-4) ...
Selecting previously unselected package libhttp-daemon-perl.
Preparing to unpack .../325-libhttp-daemon-perl_6.16-1_all.deb ...
Unpacking libhttp-daemon-perl (6.16-1) ...
Selecting previously unselected package libindirect-perl.
Preparing to unpack .../326-libindirect-perl_0.39-2build4_amd64.deb ...
Unpacking libindirect-perl (0.39-2build4) ...
Selecting previously unselected package libio-interactive-perl.
Preparing to unpack .../327-libio-interactive-perl_1.025-1_all.deb ...
Unpacking libio-interactive-perl (1.025-1) ...
Selecting previously unselected package libjs-jquery.
Preparing to unpack .../328-libjs-jquery_3.6.1+dfsg+~3.5.14-1_all.deb ...
Unpacking libjs-jquery (3.6.1+dfsg+~3.5.14-1) ...
Selecting previously unselected package libjs-underscore.
Preparing to unpack .../329-libjs-underscore_1.13.4~dfsg+~1.11.4-3_all.deb ...
Unpacking libjs-underscore (1.13.4~dfsg+~1.11.4-3) ...
Selecting previously unselected package libjs-sphinxdoc.
Preparing to unpack .../330-libjs-sphinxdoc_7.2.6-6_all.deb ...
Unpacking libjs-sphinxdoc (7.2.6-6) ...
Selecting previously unselected package libtypes-serialiser-perl.
Preparing to unpack .../331-libtypes-serialiser-perl_1.01-1_all.deb ...
Unpacking libtypes-serialiser-perl (1.01-1) ...
Selecting previously unselected package libjson-xs-perl.
Preparing to unpack .../332-libjson-xs-perl_4.030-2build3_amd64.deb ...
Unpacking libjson-xs-perl (4.030-2build3) ...
Selecting previously unselected package libjson-maybexs-perl.
Preparing to unpack .../333-libjson-maybexs-perl_1.004005-1_all.deb ...
Unpacking libjson-maybexs-perl (1.004005-1) ...
Selecting previously unselected package libjson-perl.
Preparing to unpack .../334-libjson-perl_4.10000-1_all.deb ...
Unpacking libjson-perl (4.10000-1) ...
Selecting previously unselected package libldap-common.
Preparing to unpack .../335-libldap-common_2.6.7+dfsg-1~exp1ubuntu8.1_all.deb ...
Unpacking libldap-common (2.6.7+dfsg-1~exp1ubuntu8.1) ...
Selecting previously unselected package liblist-compare-perl.
Preparing to unpack .../336-liblist-compare-perl_0.55-2_all.deb ...
Unpacking liblist-compare-perl (0.55-2) ...
Selecting previously unselected package liblist-someutils-perl.
Preparing to unpack .../337-liblist-someutils-perl_0.59-1_all.deb ...
Unpacking liblist-someutils-perl (0.59-1) ...
Selecting previously unselected package liblist-someutils-xs-perl:amd64.
Preparing to unpack .../338-liblist-someutils-xs-perl_0.58-3build4_amd64.deb ...
Unpacking liblist-someutils-xs-perl:amd64 (0.58-3build4) ...
Selecting previously unselected package liblist-utilsby-perl.
Preparing to unpack .../339-liblist-utilsby-perl_0.12-2_all.deb ...
Unpacking liblist-utilsby-perl (0.12-2) ...
Selecting previously unselected package liblog-any-perl.
Preparing to unpack .../340-liblog-any-perl_1.717-1_all.deb ...
Unpacking liblog-any-perl (1.717-1) ...
Selecting previously unselected package liblog-any-adapter-screen-perl.
Preparing to unpack .../341-liblog-any-adapter-screen-perl_0.140-2_all.deb ...
Unpacking liblog-any-adapter-screen-perl (0.140-2) ...
Selecting previously unselected package libltdl7:amd64.
Preparing to unpack .../342-libltdl7_2.4.7-7build1_amd64.deb ...
Unpacking libltdl7:amd64 (2.4.7-7build1) ...
Selecting previously unselected package libltdl-dev:amd64.
Preparing to unpack .../343-libltdl-dev_2.4.7-7build1_amd64.deb ...
Unpacking libltdl-dev:amd64 (2.4.7-7build1) ...
Selecting previously unselected package liblzo2-2:amd64.
Preparing to unpack .../344-liblzo2-2_2.10-2build4_amd64.deb ...
Unpacking liblzo2-2:amd64 (2.10-2build4) ...
Selecting previously unselected package libsys-hostname-long-perl.
Preparing to unpack .../345-libsys-hostname-long-perl_1.5-3_all.deb ...
Unpacking libsys-hostname-long-perl (1.5-3) ...
Selecting previously unselected package libmail-sendmail-perl.
Preparing to unpack .../346-libmail-sendmail-perl_0.80-3_all.deb ...
Unpacking libmail-sendmail-perl (0.80-3) ...
Selecting previously unselected package libnet-smtp-ssl-perl.
Preparing to unpack .../347-libnet-smtp-ssl-perl_1.04-2_all.deb ...
Unpacking libnet-smtp-ssl-perl (1.04-2) ...
Selecting previously unselected package libmailtools-perl.
Preparing to unpack .../348-libmailtools-perl_2.21-2_all.deb ...
Unpacking libmailtools-perl (2.21-2) ...
Selecting previously unselected package libmarkdown2:amd64.
Preparing to unpack .../349-libmarkdown2_2.2.7-2build1_amd64.deb ...
Unpacking libmarkdown2:amd64 (2.2.7-2build1) ...
Selecting previously unselected package libmath-base85-perl.
Preparing to unpack .../350-libmath-base85-perl_0.5+dfsg-2_all.deb ...
Unpacking libmath-base85-perl (0.5+dfsg-2) ...
Selecting previously unselected package libmldbm-perl.
Preparing to unpack .../351-libmldbm-perl_2.05-4_all.deb ...
Unpacking libmldbm-perl (2.05-4) ...
Selecting previously unselected package libstrictures-perl.
Preparing to unpack .../352-libstrictures-perl_2.000006-1_all.deb ...
Unpacking libstrictures-perl (2.000006-1) ...
Selecting previously unselected package libmoox-aliases-perl.
Preparing to unpack .../353-libmoox-aliases-perl_0.001006-2_all.deb ...
Unpacking libmoox-aliases-perl (0.001006-2) ...
Selecting previously unselected package libmouse-perl.
Preparing to unpack .../354-libmouse-perl_2.5.10-1build8_amd64.deb ...
Unpacking libmouse-perl (2.5.10-1build8) ...
Selecting previously unselected package libpackage-stash-perl.
Preparing to unpack .../355-libpackage-stash-perl_0.40-1_all.deb ...
Unpacking libpackage-stash-perl (0.40-1) ...
Selecting previously unselected package libsub-identify-perl.
Preparing to unpack .../356-libsub-identify-perl_0.14-3build3_amd64.deb ...
Unpacking libsub-identify-perl (0.14-3build3) ...
Selecting previously unselected package libsub-name-perl:amd64.
Preparing to unpack .../357-libsub-name-perl_0.27-1build3_amd64.deb ...
Unpacking libsub-name-perl:amd64 (0.27-1build3) ...
Selecting previously unselected package libnamespace-clean-perl.
Preparing to unpack .../358-libnamespace-clean-perl_0.27-2_all.deb ...
Unpacking libnamespace-clean-perl (0.27-2) ...
Selecting previously unselected package libncurses-dev:amd64.
Preparing to unpack .../359-libncurses-dev_6.4+20240113-1ubuntu2_amd64.deb ...
Unpacking libncurses-dev:amd64 (6.4+20240113-1ubuntu2) ...
Selecting previously unselected package libxs-parse-keyword-perl.
Preparing to unpack .../360-libxs-parse-keyword-perl_0.39-1build3_amd64.deb ...
Unpacking libxs-parse-keyword-perl (0.39-1build3) ...
Selecting previously unselected package libxs-parse-sublike-perl:amd64.
Preparing to unpack .../361-libxs-parse-sublike-perl_0.21-2build3_amd64.deb ...
Unpacking libxs-parse-sublike-perl:amd64 (0.21-2build3) ...
Selecting previously unselected package libobject-pad-perl.
Preparing to unpack .../362-libobject-pad-perl_0.808-1build3_amd64.deb ...
Unpacking libobject-pad-perl (0.808-1build3) ...
Selecting previously unselected package libpackage-stash-xs-perl:amd64.
Preparing to unpack .../363-libpackage-stash-xs-perl_0.30-1build4_amd64.deb ...
Unpacking libpackage-stash-xs-perl:amd64 (0.30-1build4) ...
Selecting previously unselected package libpath-iterator-rule-perl.
Preparing to unpack .../364-libpath-iterator-rule-perl_1.015-2_all.deb ...
Unpacking libpath-iterator-rule-perl (1.015-2) ...
Selecting previously unselected package libpath-tiny-perl.
Preparing to unpack .../365-libpath-tiny-perl_0.144-1_all.deb ...
Unpacking libpath-tiny-perl (0.144-1) ...
Selecting previously unselected package libperlio-gzip-perl.
Preparing to unpack .../366-libperlio-gzip-perl_0.20-1build4_amd64.deb ...
Unpacking libperlio-gzip-perl (0.20-1build4) ...
Selecting previously unselected package libperlio-utf8-strict-perl.
Preparing to unpack .../367-libperlio-utf8-strict-perl_0.010-1build3_amd64.deb ...
Unpacking libperlio-utf8-strict-perl (0.010-1build3) ...
Selecting previously unselected package libpod-parser-perl.
Preparing to unpack .../368-libpod-parser-perl_1.67-1_all.deb ...
Adding 'diversion of /usr/bin/podselect to /usr/bin/podselect.bundled by libpod-parser-perl'
Adding 'diversion of /usr/share/man/man1/podselect.1.gz to /usr/share/man/man1/podselect.bundled.1.gz by libpod-parser-perl'
Unpacking libpod-parser-perl (1.67-1) ...
Selecting previously unselected package libpod-constants-perl.
Preparing to unpack .../369-libpod-constants-perl_0.19-2_all.deb ...
Unpacking libpod-constants-perl (0.19-2) ...
Selecting previously unselected package libproc-processtable-perl:amd64.
Preparing to unpack .../370-libproc-processtable-perl_0.636-1build3_amd64.deb ...
Unpacking libproc-processtable-perl:amd64 (0.636-1build3) ...
Selecting previously unselected package libpython3.12t64:amd64.
Preparing to unpack .../371-libpython3.12t64_3.12.3-1ubuntu0.4_amd64.deb ...
Unpacking libpython3.12t64:amd64 (3.12.3-1ubuntu0.4) ...
Selecting previously unselected package libpython3.12-dev:amd64.
Preparing to unpack .../372-libpython3.12-dev_3.12.3-1ubuntu0.4_amd64.deb ...
Unpacking libpython3.12-dev:amd64 (3.12.3-1ubuntu0.4) ...
Selecting previously unselected package libpython3-dev:amd64.
Preparing to unpack .../373-libpython3-dev_3.12.3-0ubuntu2_amd64.deb ...
Unpacking libpython3-dev:amd64 (3.12.3-0ubuntu2) ...
Selecting previously unselected package libre2-10:amd64.
Preparing to unpack .../374-libre2-10_20230301-3build1_amd64.deb ...
Unpacking libre2-10:amd64 (20230301-3build1) ...
Selecting previously unselected package libre-engine-re2-perl:amd64.
Preparing to unpack .../375-libre-engine-re2-perl_0.18+ds-1build3_amd64.deb ...
Unpacking libre-engine-re2-perl:amd64 (0.18+ds-1build3) ...
Selecting previously unselected package libregexp-pattern-license-perl.
Preparing to unpack .../376-libregexp-pattern-license-perl_3.11.0-1_all.deb ...
Unpacking libregexp-pattern-license-perl (3.11.0-1) ...
Selecting previously unselected package libregexp-pattern-perl.
Preparing to unpack .../377-libregexp-pattern-perl_0.2.14-2_all.deb ...
Unpacking libregexp-pattern-perl (0.2.14-2) ...
Selecting previously unselected package libregexp-wildcards-perl.
Preparing to unpack .../378-libregexp-wildcards-perl_1.05-3_all.deb ...
Unpacking libregexp-wildcards-perl (1.05-3) ...
Selecting previously unselected package libsasl2-modules:amd64.
Preparing to unpack .../379-libsasl2-modules_2.1.28+dfsg1-5ubuntu3.1_amd64.deb ...
Unpacking libsasl2-modules:amd64 (2.1.28+dfsg1-5ubuntu3.1) ...
Selecting previously unselected package libsereal-decoder-perl.
Preparing to unpack .../380-libsereal-decoder-perl_5.004+ds-1build3_amd64.deb ...
Unpacking libsereal-decoder-perl (5.004+ds-1build3) ...
Selecting previously unselected package libsereal-encoder-perl.
Preparing to unpack .../381-libsereal-encoder-perl_5.004+ds-1build3_amd64.deb ...
Unpacking libsereal-encoder-perl (5.004+ds-1build3) ...
Selecting previously unselected package libserf-1-1:amd64.
Preparing to unpack .../382-libserf-1-1_1.3.10-1ubuntu0.24.04.1_amd64.deb ...
Unpacking libserf-1-1:amd64 (1.3.10-1ubuntu0.24.04.1) ...
Selecting previously unselected package libset-intspan-perl.
Preparing to unpack .../383-libset-intspan-perl_1.19-3_all.deb ...
Unpacking libset-intspan-perl (1.19-3) ...
Selecting previously unselected package libsocket6-perl.
Preparing to unpack .../384-libsocket6-perl_0.29-3build3_amd64.deb ...
Unpacking libsocket6-perl (0.29-3build3) ...
Selecting previously unselected package libsodium23:amd64.
Preparing to unpack .../385-libsodium23_1.0.18-1build3_amd64.deb ...
Unpacking libsodium23:amd64 (1.0.18-1build3) ...
Selecting previously unselected package libssl-dev:amd64.
Preparing to unpack .../386-libssl-dev_3.0.13-0ubuntu3.4_amd64.deb ...
Unpacking libssl-dev:amd64 (3.0.13-0ubuntu3.4) ...
Selecting previously unselected package libstring-copyright-perl.
Preparing to unpack .../387-libstring-copyright-perl_0.003014-1_all.deb ...
Unpacking libstring-copyright-perl (0.003014-1) ...
Selecting previously unselected package libstring-escape-perl.
Preparing to unpack .../388-libstring-escape-perl_2010.002-3_all.deb ...
Unpacking libstring-escape-perl (2010.002-3) ...
Selecting previously unselected package libstring-license-perl.
Preparing to unpack .../389-libstring-license-perl_0.0.9-2ubuntu1_all.deb ...
Unpacking libstring-license-perl (0.0.9-2ubuntu1) ...
Selecting previously unselected package libstring-shellquote-perl.
Preparing to unpack .../390-libstring-shellquote-perl_1.04-3_all.deb ...
Unpacking libstring-shellquote-perl (1.04-3) ...
Selecting previously unselected package libutf8proc3:amd64.
Preparing to unpack .../391-libutf8proc3_2.9.0-1build1_amd64.deb ...
Unpacking libutf8proc3:amd64 (2.9.0-1build1) ...
Selecting previously unselected package libsvn1:amd64.
Preparing to unpack .../392-libsvn1_1.14.3-1build4_amd64.deb ...
Unpacking libsvn1:amd64 (1.14.3-1build4) ...
Selecting previously unselected package libsyntax-keyword-try-perl.
Preparing to unpack .../393-libsyntax-keyword-try-perl_0.29-1build3_amd64.deb ...
Unpacking libsyntax-keyword-try-perl (0.29-1build3) ...
Selecting previously unselected package libterm-readkey-perl.
Preparing to unpack .../394-libterm-readkey-perl_2.38-2build4_amd64.deb ...
Unpacking libterm-readkey-perl (2.38-2build4) ...
Selecting previously unselected package libtext-levenshteinxs-perl.
Preparing to unpack .../395-libtext-levenshteinxs-perl_0.03-5build4_amd64.deb ...
Unpacking libtext-levenshteinxs-perl (0.03-5build4) ...
Selecting previously unselected package libtext-markdown-discount-perl.
Preparing to unpack .../396-libtext-markdown-discount-perl_0.16-1build3_amd64.deb ...
Unpacking libtext-markdown-discount-perl (0.16-1build3) ...
Selecting previously unselected package libtext-xslate-perl:amd64.
Preparing to unpack .../397-libtext-xslate-perl_3.5.9-1build5_amd64.deb ...
Unpacking libtext-xslate-perl:amd64 (3.5.9-1build5) ...
Selecting previously unselected package libtime-duration-perl.
Preparing to unpack .../398-libtime-duration-perl_1.21-2_all.deb ...
Unpacking libtime-duration-perl (1.21-2) ...
Selecting previously unselected package libtime-moment-perl.
Preparing to unpack .../399-libtime-moment-perl_0.44-2build4_amd64.deb ...
Unpacking libtime-moment-perl (0.44-2build4) ...
Selecting previously unselected package libunicode-utf8-perl.
Preparing to unpack .../400-libunicode-utf8-perl_0.62-2build3_amd64.deb ...
Unpacking libunicode-utf8-perl (0.62-2build3) ...
Selecting previously unselected package libwww-mechanize-perl.
Preparing to unpack .../401-libwww-mechanize-perl_2.18-1ubuntu1_all.deb ...
Unpacking libwww-mechanize-perl (2.18-1ubuntu1) ...
Selecting previously unselected package libxml-namespacesupport-perl.
Preparing to unpack .../402-libxml-namespacesupport-perl_1.12-2_all.deb ...
Unpacking libxml-namespacesupport-perl (1.12-2) ...
Selecting previously unselected package libxml-sax-base-perl.
Preparing to unpack .../403-libxml-sax-base-perl_1.09-3_all.deb ...
Unpacking libxml-sax-base-perl (1.09-3) ...
Selecting previously unselected package libxml-sax-perl.
Preparing to unpack .../404-libxml-sax-perl_1.02+dfsg-3_all.deb ...
Unpacking libxml-sax-perl (1.02+dfsg-3) ...
Selecting previously unselected package libxml-libxml-perl.
Preparing to unpack .../405-libxml-libxml-perl_2.0207+dfsg+really+2.0134-1build4_amd64.deb ...
Unpacking libxml-libxml-perl (2.0207+dfsg+really+2.0134-1build4) ...
Selecting previously unselected package libxml-parser-perl.
Preparing to unpack .../406-libxml-parser-perl_2.47-1build3_amd64.deb ...
Unpacking libxml-parser-perl (2.47-1build3) ...
Selecting previously unselected package libxml-sax-expat-perl.
Preparing to unpack .../407-libxml-sax-expat-perl_0.51-2_all.deb ...
Unpacking libxml-sax-expat-perl (0.51-2) ...
Selecting previously unselected package libxslt1.1:amd64.
Preparing to unpack .../408-libxslt1.1_1.1.39-0exp1build1_amd64.deb ...
Unpacking libxslt1.1:amd64 (1.1.39-0exp1build1) ...
Selecting previously unselected package libyaml-libyaml-perl.
Preparing to unpack .../409-libyaml-libyaml-perl_0.89+ds-1build2_amd64.deb ...
Unpacking libyaml-libyaml-perl (0.89+ds-1build2) ...
Selecting previously unselected package licensecheck.
Preparing to unpack .../410-licensecheck_3.3.9-1ubuntu1_all.deb ...
Unpacking licensecheck (3.3.9-1ubuntu1) ...
Selecting previously unselected package libdevel-size-perl.
Preparing to unpack .../411-libdevel-size-perl_0.83-2build4_amd64.deb ...
Unpacking libdevel-size-perl (0.83-2build4) ...
Selecting previously unselected package libipc-run3-perl.
Preparing to unpack .../412-libipc-run3-perl_0.049-1_all.deb ...
Unpacking libipc-run3-perl (0.049-1) ...
Selecting previously unselected package lzip.
Preparing to unpack .../413-lzip_1.24.1-1build1_amd64.deb ...
Unpacking lzip (1.24.1-1build1) ...
Selecting previously unselected package lzop.
Preparing to unpack .../414-lzop_1.04-2build3_amd64.deb ...
Unpacking lzop (1.04-2build3) ...
Selecting previously unselected package t1utils.
Preparing to unpack .../415-t1utils_1.41-4build3_amd64.deb ...
Unpacking t1utils (1.41-4build3) ...
Selecting previously unselected package lintian.
Preparing to unpack .../416-lintian_2.117.0ubuntu1.2_all.deb ...
Unpacking lintian (2.117.0ubuntu1.2) ...
Selecting previously unselected package manpages-dev.
Preparing to unpack .../417-manpages-dev_6.7-2_all.deb ...
Unpacking manpages-dev (6.7-2) ...
Selecting previously unselected package python3-certifi.
Preparing to unpack .../418-python3-certifi_2023.11.17-1_all.deb ...
Unpacking python3-certifi (2023.11.17-1) ...
Selecting previously unselected package python3-cryptography.
Preparing to unpack .../419-python3-cryptography_41.0.7-4ubuntu0.1_amd64.deb ...
Unpacking python3-cryptography (41.0.7-4ubuntu0.1) ...
Selecting previously unselected package python3.12-dev.
Preparing to unpack .../420-python3.12-dev_3.12.3-1ubuntu0.4_amd64.deb ...
Unpacking python3.12-dev (3.12.3-1ubuntu0.4) ...
Selecting previously unselected package python3-dev.
Preparing to unpack .../421-python3-dev_3.12.3-0ubuntu2_amd64.deb ...
Unpacking python3-dev (3.12.3-0ubuntu2) ...
Selecting previously unselected package python3-idna.
Preparing to unpack .../422-python3-idna_3.6-2ubuntu0.1_all.deb ...
Unpacking python3-idna (3.6-2ubuntu0.1) ...
Selecting previously unselected package python3-nacl.
Preparing to unpack .../423-python3-nacl_1.5.0-4build1_amd64.deb ...
Unpacking python3-nacl (1.5.0-4build1) ...
Selecting previously unselected package python3-bcrypt.
Preparing to unpack .../424-python3-bcrypt_3.2.2-1build1_amd64.deb ...
Unpacking python3-bcrypt (3.2.2-1build1) ...
Selecting previously unselected package python3-six.
Preparing to unpack .../425-python3-six_1.16.0-4_all.deb ...
Unpacking python3-six (1.16.0-4) ...
Selecting previously unselected package python3-paramiko.
Preparing to unpack .../426-python3-paramiko_2.12.0-2ubuntu4.1_all.deb ...
Unpacking python3-paramiko (2.12.0-2ubuntu4.1) ...
Selecting previously unselected package python3-urllib3.
Preparing to unpack .../427-python3-urllib3_2.0.7-1ubuntu0.1_all.deb ...
Unpacking python3-urllib3 (2.0.7-1ubuntu0.1) ...
Selecting previously unselected package python3-requests.
Preparing to unpack .../428-python3-requests_2.31.0+dfsg-1ubuntu1_all.deb ...
Unpacking python3-requests (2.31.0+dfsg-1ubuntu1) ...
Selecting previously unselected package python3-setuptools.
Preparing to unpack .../429-python3-setuptools_68.1.2-2ubuntu1.1_all.deb ...
Unpacking python3-setuptools (68.1.2-2ubuntu1.1) ...
Selecting previously unselected package python3-unidiff.
Preparing to unpack .../430-python3-unidiff_0.7.3-1_all.deb ...
Unpacking python3-unidiff (0.7.3-1) ...
Selecting previously unselected package subversion.
Preparing to unpack .../431-subversion_1.14.3-1build4_amd64.deb ...
Unpacking subversion (1.14.3-1build4) ...
Selecting previously unselected package swig.
Preparing to unpack .../432-swig_4.2.0-2ubuntu1_amd64.deb ...
Unpacking swig (4.2.0-2ubuntu1) ...
Selecting previously unselected package xsltproc.
Preparing to unpack .../433-xsltproc_1.1.39-0exp1build1_amd64.deb ...
Unpacking xsltproc (1.1.39-0exp1build1) ...
Selecting previously unselected package libauthen-sasl-perl.
Preparing to unpack .../434-libauthen-sasl-perl_2.1700-1_all.deb ...
Unpacking libauthen-sasl-perl (2.1700-1) ...
Selecting previously unselected package python3-magic.
Preparing to unpack .../435-python3-magic_2%3a0.4.27-3_all.deb ...
Unpacking python3-magic (2:0.4.27-3) ...
Setting up libksba8:amd64 (1.6.6-1build1) ...
Setting up pinentry-curses (1.2.1-3ubuntu5) ...
Setting up media-types (10.1.0) ...
Setting up libpipeline1:amd64 (1.5.7-2) ...
Setting up fastjar (2:0.98-7) ...
Setting up javascript-common (11+nmu1) ...
Setting up libgraphite2-3:amd64 (1.3.14-2build1) ...
Setting up liblcms2-2:amd64 (2.14-2build1) ...
Setting up wdiff (1.2.2-6build1) ...
Setting up libsharpyuv0:amd64 (1.3.2-0.4build3) ...
Setting up libaom3:amd64 (3.8.2-2ubuntu0.1) ...
Setting up libxau6:amd64 (1:1.0.9-1build6) ...
Setting up time (1.9-0.2build1) ...
Setting up libkeyutils1:amd64 (1.6.3-3build1) ...
Setting up lto-disabled-list (47) ...
Setting up libapparmor1:amd64 (4.0.1really4.0.1-0ubuntu0.24.04.3) ...
Setting up libsodium23:amd64 (1.0.18-1build3) ...
Setting up swig (4.2.0-2ubuntu1) ...
Setting up libgpm2:amd64 (1.20.7-11) ...
Setting up libzstd-dev:amd64 (1.5.5+dfsg2-2build1.1) ...
Setting up liblerc4:amd64 (4.0.0+ds-4ubuntu2) ...
Setting up libgdbm6t64:amd64 (1.23-5.1build1) ...
Setting up bsdextrautils (2.39.3-9ubuntu6.2) ...
Setting up libre2-10:amd64 (20230301-3build1) ...
Setting up java-common (0.75+exp1) ...
Setting up libtext-glob-perl (0.11-3) ...
Setting up libgdbm-compat4t64:amd64 (1.23-5.1build1) ...
Setting up xdg-user-dirs (0.18-1build1) ...
Setting up libmagic-mgc (1:5.45-3build1) ...
Setting up libutf8proc3:amd64 (2.9.0-1build1) ...
Setting up gawk (1:5.2.1-2build3) ...
Setting up libcbor0.10:amd64 (0.10.2-1.2ubuntu2) ...
Setting up libyaml-0-2:amd64 (0.2.5-1build1) ...
Setting up distro-info-data (0.60ubuntu0.2) ...
Setting up manpages (6.7-2) ...
Setting up libfcgi0t64:amd64 (2.4.2-2.1build1) ...
Setting up unzip (6.0-28ubuntu4.1) ...
Setting up libbrotli1:amd64 (1.1.0-2build2) ...
Setting up libsqlite3-0:amd64 (3.45.1-1ubuntu2) ...
Setting up libsasl2-modules:amd64 (2.1.28+dfsg1-5ubuntu3.1) ...
Setting up libmagic1t64:amd64 (1:5.45-3build1) ...
Setting up libfcgi-bin (2.4.2-2.1build1) ...
Setting up binutils-common:amd64 (2.42-4ubuntu2.3) ...
Setting up libpsl5t64:amd64 (0.21.2-1.1build1) ...
Setting up libnghttp2-14:amd64 (1.59.0-1ubuntu0.1) ...
Setting up libdeflate0:amd64 (1.19-1build1.1) ...
Setting up less (590-2ubuntu2.1) ...
Setting up perl-openssl-defaults:amd64 (7build3) ...
Setting up linux-libc-dev:amd64 (6.8.0-51.52) ...
Setting up libctf-nobfd0:amd64 (2.42-4ubuntu2.3) ...
Setting up gettext-base (0.21-14ubuntu2) ...
Setting up m4 (1.4.19-4build1) ...
Setting up liblzo2-2:amd64 (2.10-2build4) ...
Setting up krb5-locales (1.20.1-6ubuntu2.2) ...
Setting up file (1:5.45-3build1) ...
Setting up libgomp1:amd64 (14.2.0-4ubuntu2~24.04) ...
Setting up bzip2 (1.0.8-5.1build0.1) ...
Setting up libldap-common (2.6.7+dfsg-1~exp1ubuntu8.1) ...
Setting up libunwind8:amd64 (1.6.2-3build1) ...
Setting up libjbig0:amd64 (2.1-6.1ubuntu2) ...
Setting up libsframe1:amd64 (2.42-4ubuntu2.3) ...
Setting up libfakeroot:amd64 (1.33-1) ...
Setting up libelf1t64:amd64 (0.190-1.1build4) ...
Setting up libjansson4:amd64 (2.14-2build2) ...
Setting up libeclipse-jdt-core-java (3.32.0+eclipse4.26-2) ...
Setting up libkrb5support0:amd64 (1.20.1-6ubuntu2.2) ...
Setting up libnumber-compare-perl (0.03-3) ...
Setting up libdw1t64:amd64 (0.190-1.1build4) ...
Setting up libsasl2-modules-db:amd64 (2.1.28+dfsg1-5ubuntu3.1) ...
Setting up tzdata (2024a-3ubuntu1.1) ...
debconf: unable to initialize frontend: Dialog
debconf: (No usable dialog-like program is installed, so the dialog based frontend cannot be used. at /usr/share/perl5/Debconf/FrontEnd/Dialog.pm line 79.)
debconf: falling back to frontend: Readline
Configuring tzdata
------------------

Please select the geographic area in which you live. Subsequent configuration questions will narrow this down by presenting a list of cities, representing the time zones in which they are located.

  1. Africa  2. America  3. Antarctica  4. Arctic  5. Asia  6. Atlantic  7. Australia  8. Europe  9. Indian  10. Pacific  11. Etc
Geographic area: 8

Please select the city or region corresponding to your time zone.

  1. Amsterdam  6. Belgrade    11. Budapest    16. Gibraltar    21. Jersey       26. Ljubljana   31. Mariehamn  36. Oslo       41. Rome        46. Simferopol  51. Tirane     56. Vienna     61. Zurich
  2. Andorra    7. Berlin      12. Busingen    17. Guernsey     22. Kaliningrad  27. London      32. Minsk      37. Paris      42. Samara      47. Skopje      52. Tiraspol   57. Vilnius
  3. Astrakhan  8. Bratislava  13. Chisinau    18. Helsinki     23. Kirov        28. Luxembourg  33. Monaco     38. Podgorica  43. San_Marino  48. Sofia       53. Ulyanovsk  58. Volgograd
  4. Athens     9. Brussels    14. Copenhagen  19. Isle_of_Man  24. Kyiv         29. Madrid      34. Moscow     39. Prague     44. Sarajevo    49. Stockholm   54. Vaduz      59. Warsaw
  5. Belfast    10. Bucharest  15. Dublin      20. Istanbul     25. Lisbon       30. Malta       35. Nicosia    40. Riga       45. Saratov     50. Tallinn     55. Vatican    60. Zagreb
Time zone: 25


Current default time zone: 'Europe/Lisbon'
Local time is now:      Sun Jan 26 01:08:37 WET 2025.
Universal Time is now:  Sun Jan 26 01:08:37 UTC 2025.
Run 'dpkg-reconfigure tzdata' if you wish to change it.

Setting up fakeroot (1.33-1) ...
update-alternatives: using /usr/bin/fakeroot-sysv to provide /usr/bin/fakeroot (fakeroot) in auto mode
update-alternatives: warning: skip creation of /usr/share/man/man1/fakeroot.1.gz because associated file /usr/share/man/man1/fakeroot-sysv.1.gz (of link group fakeroot) doesn't exist
update-alternatives: warning: skip creation of /usr/share/man/man1/faked.1.gz because associated file /usr/share/man/man1/faked-sysv.1.gz (of link group fakeroot) doesn't exist
update-alternatives: warning: skip creation of /usr/share/man/es/man1/fakeroot.1.gz because associated file /usr/share/man/es/man1/fakeroot-sysv.1.gz (of link group fakeroot) doesn't exist
update-alternatives: warning: skip creation of /usr/share/man/es/man1/faked.1.gz because associated file /usr/share/man/es/man1/faked-sysv.1.gz (of link group fakeroot) doesn't exist
update-alternatives: warning: skip creation of /usr/share/man/fr/man1/fakeroot.1.gz because associated file /usr/share/man/fr/man1/fakeroot-sysv.1.gz (of link group fakeroot) doesn't exist
update-alternatives: warning: skip creation of /usr/share/man/fr/man1/faked.1.gz because associated file /usr/share/man/fr/man1/faked-sysv.1.gz (of link group fakeroot) doesn't exist
update-alternatives: warning: skip creation of /usr/share/man/sv/man1/fakeroot.1.gz because associated file /usr/share/man/sv/man1/fakeroot-sysv.1.gz (of link group fakeroot) doesn't exist
update-alternatives: warning: skip creation of /usr/share/man/sv/man1/faked.1.gz because associated file /usr/share/man/sv/man1/faked-sysv.1.gz (of link group fakeroot) doesn't exist
Setting up libasound2-data (1.2.11-1build2) ...
Setting up autotools-dev (20220109.1) ...
Setting up libglib2.0-0t64:amd64 (2.80.0-6ubuntu3.2) ...
No schema files found: doing nothing.
Setting up libglib2.0-data (2.80.0-6ubuntu3.2) ...
Setting up rpcsvc-proto (1.4.2-0ubuntu7) ...
Setting up libasound2t64:amd64 (1.2.11-1build2) ...
Setting up gcc-13-base:amd64 (13.3.0-6ubuntu2~24.04) ...
Setting up libx11-data (2:1.8.7-1build1) ...
Setting up make (4.3-4.1build2) ...
Setting up libnspr4:amd64 (2:4.35-1.1build1) ...
Setting up gnupg-l10n (2.4.4-2ubuntu17) ...
Setting up librtmp1:amd64 (2.4+20151223.gitfa8646d.1-2build7) ...
Setting up lzip (1.24.1-1build1) ...
update-alternatives: using /usr/bin/lzip.lzip to provide /usr/bin/lzip (lzip) in auto mode
update-alternatives: warning: skip creation of /usr/share/man/man1/lzip.1.gz because associated file /usr/share/man/man1/lzip.lzip.1.gz (of link group lzip) doesn't exist
update-alternatives: using /usr/bin/lzip.lzip to provide /usr/bin/lzip-compressor (lzip-compressor) in auto mode
update-alternatives: warning: skip creation of /usr/share/man/man1/lzip-compressor.1.gz because associated file /usr/share/man/man1/lzip.lzip.1.gz (of link group lzip-compressor) doesn't exist
update-alternatives: using /usr/bin/lzip.lzip to provide /usr/bin/lzip-decompressor (lzip-decompressor) in auto mode
update-alternatives: warning: skip creation of /usr/share/man/man1/lzip-decompressor.1.gz because associated file /usr/share/man/man1/lzip.lzip.1.gz (of link group lzip-decompressor) doesn't exist
Setting up libavahi-common-data:amd64 (0.8-13ubuntu6) ...
Setting up libncurses6:amd64 (6.4+20240113-1ubuntu2) ...
Setting up strace (6.8-0ubuntu2) ...
Setting up libdbus-1-3:amd64 (1.14.10-4ubuntu4.1) ...
Setting up xz-utils (5.6.1+really5.4.5-1build0.1) ...
update-alternatives: using /usr/bin/xz to provide /usr/bin/lzma (lzma) in auto mode
update-alternatives: warning: skip creation of /usr/share/man/man1/lzma.1.gz because associated file /usr/share/man/man1/xz.1.gz (of link group lzma) doesn't exist
update-alternatives: warning: skip creation of /usr/share/man/man1/unlzma.1.gz because associated file /usr/share/man/man1/unxz.1.gz (of link group lzma) doesn't exist
update-alternatives: warning: skip creation of /usr/share/man/man1/lzcat.1.gz because associated file /usr/share/man/man1/xzcat.1.gz (of link group lzma) doesn't exist
update-alternatives: warning: skip creation of /usr/share/man/man1/lzmore.1.gz because associated file /usr/share/man/man1/xzmore.1.gz (of link group lzma) doesn't exist
update-alternatives: warning: skip creation of /usr/share/man/man1/lzless.1.gz because associated file /usr/share/man/man1/xzless.1.gz (of link group lzma) doesn't exist
update-alternatives: warning: skip creation of /usr/share/man/man1/lzdiff.1.gz because associated file /usr/share/man/man1/xzdiff.1.gz (of link group lzma) doesn't exist
update-alternatives: warning: skip creation of /usr/share/man/man1/lzcmp.1.gz because associated file /usr/share/man/man1/xzcmp.1.gz (of link group lzma) doesn't exist
update-alternatives: warning: skip creation of /usr/share/man/man1/lzgrep.1.gz because associated file /usr/share/man/man1/xzgrep.1.gz (of link group lzma) doesn't exist
update-alternatives: warning: skip creation of /usr/share/man/man1/lzegrep.1.gz because associated file /usr/share/man/man1/xzegrep.1.gz (of link group lzma) doesn't exist
update-alternatives: warning: skip creation of /usr/share/man/man1/lzfgrep.1.gz because associated file /usr/share/man/man1/xzfgrep.1.gz (of link group lzma) doesn't exist
Setting up perl-modules-5.38 (5.38.2-3.2build2) ...
Setting up t1utils (1.41-4build3) ...
Setting up libquadmath0:amd64 (14.2.0-4ubuntu2~24.04) ...
Setting up diffstat (1.66-1build1) ...
Setting up fonts-dejavu-mono (2.37-8) ...
Setting up libssl-dev:amd64 (3.0.13-0ubuntu3.4) ...
Setting up libpng16-16t64:amd64 (1.6.43-5build1) ...
Setting up libmpc3:amd64 (1.3.1-1build1) ...
Setting up libatomic1:amd64 (14.2.0-4ubuntu2~24.04) ...
Setting up patch (2.7.6-7build3) ...
Setting up autopoint (0.21-14ubuntu2) ...
Setting up binfmt-support (2.2.2-7) ...
invoke-rc.d: could not determine current runlevel
invoke-rc.d: policy-rc.d denied execution of start.
Setting up fonts-dejavu-core (2.37-8) ...
Setting up libpcsclite1:amd64 (2.0.3-1build1) ...
Setting up ucf (3.0043+nmu1) ...
debconf: unable to initialize frontend: Dialog
debconf: (No usable dialog-like program is installed, so the dialog based frontend cannot be used. at /usr/share/perl5/Debconf/FrontEnd/Dialog.pm line 79.)
debconf: falling back to frontend: Readline
Setting up libk5crypto3:amd64 (1.20.1-6ubuntu2.2) ...
Setting up libjpeg-turbo8:amd64 (2.1.5-2ubuntu2) ...
Setting up libltdl7:amd64 (2.4.7-7build1) ...
Setting up libsasl2-2:amd64 (2.1.28+dfsg1-5ubuntu3.1) ...
Setting up libwebp7:amd64 (1.3.2-0.4build3) ...
Setting up libubsan1:amd64 (14.2.0-4ubuntu2~24.04) ...
Setting up libicu74:amd64 (74.2-1ubuntu3.1) ...
Setting up dwz (0.15-1build6) ...
Setting up alsa-topology-conf (1.2.5.1-2) ...
Setting up python-apt-common (2.7.7ubuntu3) ...
Setting up mount (2.39.3-9ubuntu6.2) ...
Setting up libhwasan0:amd64 (14.2.0-4ubuntu2~24.04) ...
Setting up libcrypt-dev:amd64 (1:4.4.36-4build1) ...
Setting up dbus-session-bus-common (1.14.10-4ubuntu4.1) ...
Setting up libasan8:amd64 (14.2.0-4ubuntu2~24.04) ...
Setting up libuchardet0:amd64 (0.0.8-1build1) ...
Setting up lzop (1.04-2build3) ...
Setting up libapr1t64:amd64 (1.7.2-3.1ubuntu0.1) ...
Setting up gpgconf (2.4.4-2ubuntu17) ...
Setting up debugedit (1:5.0-5build2) ...
Setting up git-man (1:2.43.0-1ubuntu7.2) ...
Setting up netbase (6.4) ...
Setting up libkrb5-3:amd64 (1.20.1-6ubuntu2.2) ...
Setting up libperl5.38t64:amd64 (5.38.2-3.2build2) ...
Setting up libtsan2:amd64 (14.2.0-4ubuntu2~24.04) ...
Setting up libjs-jquery (3.6.1+dfsg+~3.5.14-1) ...
Setting up libbinutils:amd64 (2.42-4ubuntu2.3) ...
Setting up lsb-release (12.0-2) ...
Setting up dbus-system-bus-common (1.14.10-4ubuntu4.1) ...
Setting up libfido2-1:amd64 (1.14.0-1build3) ...
Setting up libisl23:amd64 (0.26-3build1) ...
Setting up libde265-0:amd64 (1.0.15-1build3) ...
Setting up libc-dev-bin (2.39-0ubuntu8.3) ...
Setting up openssl (3.0.13-0ubuntu3.4) ...
Setting up libbsd0:amd64 (0.12.1-1build1) ...
Setting up libhiredis1.1.0:amd64 (1.2.0-6ubuntu3) ...
Setting up publicsuffix (20231001.0357-0.1) ...
Setting up libxml2:amd64 (2.9.14+dfsg-1.3ubuntu3) ...
Setting up zstd (1.5.5+dfsg2-2build1.1) ...
Setting up libmarkdown2:amd64 (2.2.7-2build1) ...
Setting up libcc1-0:amd64 (14.2.0-4ubuntu2~24.04) ...
Setting up libldap2:amd64 (2.6.7+dfsg-1~exp1ubuntu8.1) ...
Setting up iso-codes (4.16.0-1) ...
Setting up dbus-bin (1.14.10-4ubuntu4.1) ...
Setting up liblocale-gettext-perl (1.07-6ubuntu5) ...
Setting up gpg (2.4.4-2ubuntu17) ...
Setting up liblsan0:amd64 (14.2.0-4ubuntu2~24.04) ...
Setting up dctrl-tools (2.24-3build3) ...
Setting up libitm1:amd64 (14.2.0-4ubuntu2~24.04) ...
Setting up libjs-underscore (1.13.4~dfsg+~1.11.4-3) ...
Setting up libpopt0:amd64 (1.19+dfsg-1build1) ...
Setting up gnupg-utils (2.4.4-2ubuntu17) ...
Setting up libctf0:amd64 (2.42-4ubuntu2.3) ...
Setting up libjpeg8:amd64 (8c-2ubuntu11) ...
Setting up libaprutil1t64:amd64 (1.6.3-1.1ubuntu7) ...
Setting up manpages-dev (6.7-2) ...
Setting up libxdmcp6:amd64 (1:1.1.3-0ubuntu6) ...
Setting up libxcb1:amd64 (1.15-1ubuntu2) ...
Setting up gettext (0.21-14ubuntu2) ...
Setting up gpg-agent (2.4.4-2ubuntu17) ...
Setting up java-wrappers (0.4) ...
Setting up libpython3.12-stdlib:amd64 (3.12.3-1ubuntu0.4) ...
Setting up wget (1.21.4-1ubuntu4.1) ...
Setting up jarwrapper (0.79) ...
Setting up alsa-ucm-conf (1.2.10-1ubuntu5.4) ...
Setting up cpp-13-x86-64-linux-gnu (13.3.0-6ubuntu2~24.04) ...
Setting up fontconfig-config (2.15.0-1.1ubuntu2) ...
Setting up python3.12 (3.12.3-1ubuntu0.4) ...
Setting up libedit2:amd64 (3.1-20230828-1build1) ...
Setting up gpgsm (2.4.4-2ubuntu17) ...
Setting up ccache (4.9.1-1) ...
Updating symlinks in /usr/lib/ccache ...
Setting up libavahi-common3:amd64 (0.8-13ubuntu6) ...
Setting up libnss3:amd64 (2:3.98-1build1) ...
Setting up dbus-daemon (1.14.10-4ubuntu4.1) ...
Setting up libpython3.12t64:amd64 (3.12.3-1ubuntu0.4) ...
Setting up dirmngr (2.4.4-2ubuntu17) ...
Setting up ca-certificates (20240203) ...
debconf: unable to initialize frontend: Dialog
debconf: (No usable dialog-like program is installed, so the dialog based frontend cannot be used. at /usr/share/perl5/Debconf/FrontEnd/Dialog.pm line 79.)
debconf: falling back to frontend: Readline
Updating certificates in /etc/ssl/certs...
146 added, 0 removed; done.
Setting up perl (5.38.2-3.2build2) ...
Setting up libpackage-stash-xs-perl:amd64 (0.30-1build4) ...
Setting up libgprofng0:amd64 (2.42-4ubuntu2.3) ...
Setting up libclass-data-inheritable-perl (0.08-3) ...
Setting up libxs-parse-keyword-perl (0.39-1build3) ...
Setting up libfreetype6:amd64 (2.13.2+dfsg-1build3) ...
Setting up libdata-dump-perl (1.25-1) ...
Setting up libfile-find-rule-perl (0.34-3) ...
Setting up libipc-system-simple-perl (1.30-2) ...
Setting up libnet-domain-tld-perl (1.75-3) ...
Setting up libperlio-utf8-strict-perl (0.010-1build3) ...
Setting up libsocket6-perl (0.29-3build3) ...
Setting up dbus (1.14.10-4ubuntu4.1) ...
Setting up shared-mime-info (2.4-4) ...
Setting up libgssapi-krb5-2:amd64 (1.20.1-6ubuntu2.2) ...
Setting up libvariable-magic-perl (0.63-1build3) ...
Setting up libio-html-perl (1.004-3) ...
Setting up libpod-parser-perl (1.67-1) ...
Setting up libb-hooks-op-check-perl:amd64 (0.22-3build1) ...
Setting up keyboxd (2.4.4-2ubuntu17) ...
Setting up libjs-sphinxdoc (7.2.6-6) ...
Setting up libparams-util-perl (1.102-2build3) ...
Setting up libdpkg-perl (1.22.6ubuntu6.1) ...
Setting up libssh-4:amd64 (0.10.6-2build2) ...
Setting up libtime-duration-perl (1.21-2) ...
Setting up autoconf (2.71-3) ...
Setting up libsub-exporter-progressive-perl (0.001013-3) ...
Setting up libarray-intspan-perl (2.004-2) ...
Setting up libcapture-tiny-perl (0.48-2) ...
Setting up libtimedate-perl (2.3300-2) ...
Setting up libsub-name-perl:amd64 (0.27-1build3) ...
Setting up libsyntax-keyword-try-perl (0.29-1build3) ...
Setting up libdata-validate-domain-perl (0.10-1.1) ...
Setting up libproc-processtable-perl:amd64 (0.636-1build3) ...
Setting up libfile-chdir-perl (0.1008-1.1) ...
Setting up libgcc-13-dev:amd64 (13.3.0-6ubuntu2~24.04) ...
Setting up groff-base (1.23.0-3build2) ...
Setting up libtiff6:amd64 (4.5.1+git230720-4ubuntu2.2) ...
Setting up libpath-tiny-perl (0.144-1) ...
Setting up libarchive-cpio-perl (0.10-3) ...
Setting up libjson-perl (4.10000-1) ...
Setting up libxslt1.1:amd64 (1.1.39-0exp1build1) ...
Setting up gnupg (2.4.4-2ubuntu17) ...
Setting up libipc-run3-perl (0.049-1) ...
Setting up libregexp-wildcards-perl (1.05-3) ...
Setting up libfcgi-perl (0.82+ds-3build2) ...
Setting up libsub-override-perl (0.10-1) ...
Setting up libc6-dev:amd64 (2.39-0ubuntu8.3) ...
Setting up libx11-6:amd64 (2:1.8.7-1build1) ...
Setting up libaliased-perl (0.34-3) ...
Setting up libharfbuzz0b:amd64 (8.3.0-2build2) ...
Setting up libstrictures-perl (2.000006-1) ...
Setting up libsub-quote-perl (2.006008-1ubuntu1) ...
Setting up libdevel-stacktrace-perl (2.0500-1) ...
Setting up libclass-xsaccessor-perl (1.19-4build4) ...
Setting up libgpgme11t64:amd64 (1.18.0-4.1ubuntu4) ...
Setting up libfontconfig1:amd64 (2.15.0-1.1ubuntu2) ...
Setting up libsort-versions-perl (1.62-3) ...
Setting up ca-certificates-java (20240118) ...
No JRE found. Skipping Java certificates setup.
Setting up libexporter-tiny-perl (1.006002-1) ...
Setting up libre-engine-re2-perl:amd64 (0.18+ds-1build3) ...
Setting up libfile-dirlist-perl (0.05-3) ...
Setting up libterm-readkey-perl (2.38-2build4) ...
Setting up libtext-levenshteinxs-perl (0.03-5build4) ...
Setting up libperlio-gzip-perl (0.20-1build4) ...
Setting up libsys-hostname-long-perl (1.5-3) ...
Setting up libhtml-html5-entities-perl (0.004-3) ...
Setting up libavahi-client3:amd64 (0.8-13ubuntu6) ...
Setting up libsereal-decoder-perl (5.004+ds-1build3) ...
Setting up liburi-perl (5.27-1) ...
Setting up libxmuu1:amd64 (2:1.1.3-3build2) ...
Setting up libnet-ipv6addr-perl (1.02-1) ...
Setting up libfile-touch-perl (0.12-2) ...
Setting up rsync (3.2.7-1ubuntu1.2) ...
invoke-rc.d: could not determine current runlevel
invoke-rc.d: policy-rc.d denied execution of start.
Setting up libstdc++-13-dev:amd64 (13.3.0-6ubuntu2~24.04) ...
Setting up libpython3-stdlib:amd64 (3.12.3-0ubuntu2) ...
Setting up binutils-x86-64-linux-gnu (2.42-4ubuntu2.3) ...
Setting up libemail-address-xs-perl (1.05-1build4) ...
Setting up libnet-ssleay-perl:amd64 (1.94-1build4) ...
Setting up cpp-x86-64-linux-gnu (4:13.2.0-7ubuntu1) ...
Setting up automake (1:1.16.5-1.3ubuntu1) ...
update-alternatives: using /usr/bin/automake-1.16 to provide /usr/bin/automake (automake) in auto mode
update-alternatives: warning: skip creation of /usr/share/man/man1/automake.1.gz because associated file /usr/share/man/man1/automake-1.16.1.gz (of link group automake) doesn't exist
update-alternatives: warning: skip creation of /usr/share/man/man1/aclocal.1.gz because associated file /usr/share/man/man1/aclocal-1.16.1.gz (of link group automake) doesn't exist
Setting up libapt-pkg-perl (0.1.40build7) ...
Setting up libstring-escape-perl (2010.002-3) ...
Setting up libberkeleydb-perl:amd64 (0.64-2build4) ...
Setting up libhttp-date-perl (6.06-1) ...
Setting up libfile-which-perl (1.27-2) ...
Setting up libncurses-dev:amd64 (6.4+20240113-1ubuntu2) ...
Setting up libfile-basedir-perl (0.09-2) ...
Setting up libunicode-utf8-perl (0.62-2build3) ...
Setting up libset-intspan-perl (1.19-3) ...
Setting up libmouse-perl (2.5.10-1build8) ...
Setting up libfile-listing-perl (6.16-1) ...
Setting up libxpm4:amd64 (1:3.5.17-1build2) ...
Setting up openjdk-21-jre-headless:amd64 (21.0.5+11-1ubuntu1~24.04) ...
update-alternatives: using /usr/lib/jvm/java-21-openjdk-amd64/bin/java to provide /usr/bin/java (java) in auto mode
update-alternatives: using /usr/lib/jvm/java-21-openjdk-amd64/bin/jpackage to provide /usr/bin/jpackage (jpackage) in auto mode
update-alternatives: using /usr/lib/jvm/java-21-openjdk-amd64/bin/keytool to provide /usr/bin/keytool (keytool) in auto mode
update-alternatives: using /usr/lib/jvm/java-21-openjdk-amd64/bin/rmiregistry to provide /usr/bin/rmiregistry (rmiregistry) in auto mode
update-alternatives: using /usr/lib/jvm/java-21-openjdk-amd64/lib/jexec to provide /usr/bin/jexec (jexec) in auto mode
Setting up gpg-wks-client (2.4.4-2ubuntu17) ...
Setting up libregexp-pattern-perl (0.2.14-2) ...
Setting up cpp-13 (13.3.0-6ubuntu2~24.04) ...
Setting up libdata-messagepack-perl (1.02-1build4) ...
Setting up libfont-afm-perl (1.20-4) ...
Setting up libdynaloader-functions-perl (0.003-3) ...
Setting up libclass-method-modifiers-perl (2.15-1) ...
Setting up liblist-compare-perl (0.55-2) ...
Setting up libio-pty-perl (1:1.20-1build2) ...
Setting up libfile-fcntllock-perl (0.22-4ubuntu5) ...
Setting up libclone-perl:amd64 (0.46-1build3) ...
Setting up libalgorithm-diff-perl (1.201-1) ...
Setting up libarchive-zip-perl (1.68-1) ...
Setting up libsub-identify-perl (0.14-3build3) ...
Setting up libdistro-info-perl (1.7build1) ...
Setting up libcpanel-json-xs-perl:amd64 (4.37-1build3) ...
Setting up openssh-client (1:9.6p1-3ubuntu13.5) ...
Setting up gcc-13-x86-64-linux-gnu (13.3.0-6ubuntu2~24.04) ...
Setting up libhtml-tagset-perl (3.20-6) ...
Setting up liblog-any-perl (1.717-1) ...
Setting up libauthen-sasl-perl (2.1700-1) ...
Setting up libdevel-size-perl (0.83-2build4) ...
Setting up libdebhelper-perl (13.14.1ubuntu5) ...
Setting up libpod-constants-perl (0.19-2) ...
Setting up libregexp-pattern-license-perl (3.11.0-1) ...
Setting up liblwp-mediatypes-perl (6.04-2) ...
Setting up xsltproc (1.1.39-0exp1build1) ...
Setting up libyaml-libyaml-perl (0.89+ds-1build2) ...
Setting up libio-interactive-perl (1.025-1) ...
Setting up libserf-1-1:amd64 (1.3.10-1ubuntu0.24.04.1) ...
Setting up libtry-tiny-perl (0.31-2) ...
Setting up libcurl3t64-gnutls:amd64 (8.5.0-2ubuntu10.6) ...
Setting up libxext6:amd64 (2:1.3.4-1build2) ...
Setting up libcommon-sense-perl:amd64 (3.75-3build3) ...
Setting up libmldbm-perl (2.05-4) ...
Setting up libxml-namespacesupport-perl (1.12-2) ...
Setting up libnet-http-perl (6.23-1) ...
Setting up libpath-iterator-rule-perl (1.015-2) ...
Setting up libtext-markdown-discount-perl (0.16-1build3) ...
Setting up libtime-moment-perl (0.44-2build4) ...
Setting up libencode-locale-perl (1.05-3) ...
Setting up python3 (3.12.3-0ubuntu2) ...
running python rtupdate hooks for python3.12...
running python post-rtupdate hooks for python3.12...
Setting up libexception-class-perl (1.45-1) ...
Setting up binutils (2.42-4ubuntu2.3) ...
Setting up libmath-base85-perl (0.5+dfsg-2) ...
Setting up python3-xdg (0.28-2) ...
Setting up libconfig-tiny-perl (2.30-1) ...
Setting up libsereal-encoder-perl (5.004+ds-1build3) ...
Setting up libdevel-callchecker-perl:amd64 (0.008-2build3) ...
Setting up man-db (2.12.0-4build2) ...
debconf: unable to initialize frontend: Dialog
debconf: (No usable dialog-like program is installed, so the dialog based frontend cannot be used. at /usr/share/perl5/Debconf/FrontEnd/Dialog.pm line 79.)
debconf: falling back to frontend: Readline
Building database of manual pages ...
Setting up liblist-utilsby-perl (0.12-2) ...
Setting up libstring-shellquote-perl (1.04-3) ...
Setting up libnet-netmask-perl (2.0002-2) ...
Setting up libsub-install-perl (0.929-1) ...
Setting up dpkg-dev (1.22.6ubuntu6.1) ...
Setting up libindirect-perl (0.39-2build4) ...
Setting up libxs-parse-sublike-perl:amd64 (0.21-2build3) ...
Setting up intltool-debian (0.35.0+20060710.6) ...
Setting up libfreezethaw-perl (0.5001-3) ...
Setting up libobject-pad-perl (0.808-1build3) ...
Setting up liberror-perl (0.17029-2) ...
Setting up python3-six (1.16.0-4) ...
Setting up patchutils (0.4.2-1build3) ...
Setting up libmail-sendmail-perl (0.80-3) ...
Setting up libltdl-dev:amd64 (2.4.7-7build1) ...
Setting up libjson-maybexs-perl (1.004005-1) ...
Setting up libxml-sax-base-perl (1.09-3) ...
Setting up libio-string-perl (1.08-4) ...
Setting up libnetaddr-ip-perl (4.079+dfsg-2build4) ...
Setting up libexpat1-dev:amd64 (2.6.1-2ubuntu0.2) ...
Setting up python3-gpg (1.18.0-4.1ubuntu4) ...
Setting up python3-certifi (2023.11.17-1) ...
Setting up libstring-copyright-perl (0.003014-1) ...
Setting up gcc-13 (13.3.0-6ubuntu2~24.04) ...
Setting up python3-idna (3.6-2ubuntu0.1) ...
Setting up libdata-optlist-perl (0.114-1) ...
Setting up libipc-run-perl (20231003.0-1) ...
Setting up git (1:2.43.0-1ubuntu7.2) ...
Setting up libtext-xslate-perl:amd64 (3.5.9-1build5) ...
Setting up python3-urllib3 (2.0.7-1ubuntu0.1) ...
Setting up zlib1g-dev:amd64 (1:1.3.dfsg-3.1ubuntu2.1) ...
Setting up libwww-robotrules-perl (6.02-1) ...
Setting up libtypes-serialiser-perl (1.01-1) ...
Setting up xauth (1:1.1.2-1build1) ...
Setting up cpp (4:13.2.0-7ubuntu1) ...
Setting up libhtml-parser-perl:amd64 (3.81-1build3) ...
Setting up libgit-wrapper-perl (0.048-2) ...
Setting up liblog-any-adapter-screen-perl (0.140-2) ...
Setting up librole-tiny-perl (2.002004-1) ...
Setting up python3-unidiff (0.7.3-1) ...
Setting up libsvn1:amd64 (1.14.3-1build4) ...
Setting up libfont-ttf-perl (1.06-2) ...
Setting up libfile-homedir-perl (1.006-2) ...
Setting up python3-magic (2:0.4.27-3) ...
Setting up libalgorithm-diff-xs-perl:amd64 (0.04-8build3) ...
Setting up libcups2t64:amd64 (2.4.7-1.2ubuntu7.3) ...
Setting up libio-socket-ssl-perl (2.085-1) ...
Setting up python3-cffi-backend:amd64 (1.16.0-2build1) ...
Setting up libsub-exporter-perl (0.990-1) ...
Setting up libalgorithm-merge-perl (0.08-5) ...
Setting up libhttp-message-perl (6.45-1ubuntu1) ...
Setting up libdata-validate-ip-perl (0.31-1) ...
Setting up libhtml-form-perl (6.11-1) ...
Setting up python3-pkg-resources (68.1.2-2ubuntu1.1) ...
Setting up libiterator-perl (0.03+ds1-2) ...
Setting up libfile-stripnondeterminism-perl (1.13.1-1) ...
Setting up libjson-xs-perl (4.030-2build3) ...
Setting up libhttp-negotiate-perl (6.01-2) ...
Setting up python3-setuptools (68.1.2-2ubuntu1.1) ...
Setting up g++-13-x86-64-linux-gnu (13.3.0-6ubuntu2~24.04) ...
Setting up gcc-x86-64-linux-gnu (4:13.2.0-7ubuntu1) ...
Setting up libiterator-util-perl (0.02+ds1-2) ...
Setting up libhttp-cookies-perl (6.11-1) ...
Setting up libtool (2.4.7-7build1) ...
Setting up python3-apt (2.7.7ubuntu3) ...
Setting up po-debconf (1.0.21+nmu1) ...
Setting up libhtml-tree-perl (5.07-3) ...
Setting up python3-bcrypt (3.2.2-1build1) ...
Setting up libparams-classify-perl:amd64 (0.015-2build5) ...
Setting up libcgi-pm-perl (4.63-1) ...
Setting up subversion (1.14.3-1build4) ...
Setting up libpython3.12-dev:amd64 (3.12.3-1ubuntu0.4) ...
Setting up libhtml-format-perl (2.16-2) ...
Setting up libxml-sax-perl (1.02+dfsg-3) ...
update-perl-sax-parsers: Registering Perl SAX parser XML::SAX::PurePerl with priority 10...
update-perl-sax-parsers: Updating overall Perl SAX parser modules info file...
debconf: unable to initialize frontend: Dialog
debconf: (No usable dialog-like program is installed, so the dialog based frontend cannot be used. at /usr/share/perl5/Debconf/FrontEnd/Dialog.pm line 79.)
debconf: falling back to frontend: Readline

Creating config file /etc/perl/XML/SAX/ParserDetails.ini with new version
Setting up gcc (4:13.2.0-7ubuntu1) ...
Setting up dh-autoreconf (20) ...
Setting up libdata-validate-uri-perl (0.07-3) ...
Setting up python3-chardet (5.2.0+dfsg-1) ...
Setting up libnet-smtp-ssl-perl (1.04-2) ...
Setting up libmodule-runtime-perl (0.016-2) ...
Setting up libmailtools-perl (2.21-2) ...
Setting up python3-cryptography (41.0.7-4ubuntu0.1) ...
Setting up python3-debian (0.1.49ubuntu2) ...
Setting up python3-requests (2.31.0+dfsg-1ubuntu1) ...
Setting up python3.12-dev (3.12.3-1ubuntu0.4) ...
Setting up libelf-dev:amd64 (0.190-1.1build4) ...
Setting up libxml-libxml-perl (2.0207+dfsg+really+2.0134-1build4) ...
update-perl-sax-parsers: Registering Perl SAX parser XML::LibXML::SAX::Parser with priority 50...
update-perl-sax-parsers: Registering Perl SAX parser XML::LibXML::SAX with priority 50...
update-perl-sax-parsers: Updating overall Perl SAX parser modules info file...
debconf: unable to initialize frontend: Dialog
debconf: (No usable dialog-like program is installed, so the dialog based frontend cannot be used. at /usr/share/perl5/Debconf/FrontEnd/Dialog.pm line 79.)
debconf: falling back to frontend: Readline
Replacing config file /etc/perl/XML/SAX/ParserDetails.ini with new version
Setting up dh-strip-nondeterminism (1.13.1-1) ...
Setting up libconst-fast-perl (0.014-2) ...
Setting up libhttp-daemon-perl (6.16-1) ...
Setting up g++-x86-64-linux-gnu (4:13.2.0-7ubuntu1) ...
Setting up libdata-dpath-perl (0.59-1) ...
Setting up python3-nacl (1.5.0-4build1) ...
Setting up g++-13 (13.3.0-6ubuntu2~24.04) ...
Setting up libpython3-dev:amd64 (3.12.3-0ubuntu2) ...
Setting up libmodule-implementation-perl (0.09-2) ...
Setting up libcgi-fast-perl (1:2.17-1) ...
Setting up libpackage-stash-perl (0.40-1) ...
Setting up libimport-into-perl (1.002005-2) ...
Setting up libmoo-perl (2.005005-1) ...
Setting up liblist-someutils-perl (0.59-1) ...
Setting up debhelper (13.14.1ubuntu5) ...
Setting up dput (1.1.3ubuntu3) ...
Setting up liblist-someutils-xs-perl:amd64 (0.58-3build4) ...
Setting up python3-dev (3.12.3-0ubuntu2) ...
Setting up g++ (4:13.2.0-7ubuntu1) ...
update-alternatives: using /usr/bin/g++ to provide /usr/bin/c++ (c++) in auto mode
update-alternatives: warning: skip creation of /usr/share/man/man1/c++.1.gz because associated file /usr/share/man/man1/g++.1.gz (of link group c++) doesn't exist
Setting up build-essential (12.10ubuntu1) ...
Setting up python3-paramiko (2.12.0-2ubuntu4.1) ...
Setting up libmoox-aliases-perl (0.001006-2) ...
Setting up libb-hooks-endofscope-perl (0.28-1) ...
Setting up libnamespace-clean-perl (0.27-2) ...
Setting up libstring-license-perl (0.0.9-2ubuntu1) ...
Setting up licensecheck (3.3.9-1ubuntu1) ...
Setting up libwww-perl (6.76-1) ...
Setting up libheif1:amd64 (1.17.6-1ubuntu4.1) ...
Setting up libhtml-tokeparser-simple-perl (3.16-4) ...
Setting up libwww-mechanize-perl (2.18-1ubuntu1) ...
Setting up devscripts (2.23.7) ...
Setting up libgd3:amd64 (2.3.3-9ubuntu5) ...
Setting up javahelper (0.79) ...
Setting up libc-devtools (2.39-0ubuntu8.3) ...
Setting up libheif-plugin-aomdec:amd64 (1.17.6-1ubuntu4.1) ...
Setting up liblwp-protocol-https-perl (6.13-1) ...
Setting up libheif-plugin-libde265:amd64 (1.17.6-1ubuntu4.1) ...
Setting up java-propose-classpath (0.79) ...
Setting up libxml-parser-perl (2.47-1build3) ...
Setting up libheif-plugin-aomenc:amd64 (1.17.6-1ubuntu4.1) ...
Setting up lintian (2.117.0ubuntu1.2) ...
Setting up libxml-sax-expat-perl (0.51-2) ...
update-perl-sax-parsers: Registering Perl SAX parser XML::SAX::Expat with priority 50...
update-perl-sax-parsers: Updating overall Perl SAX parser modules info file...
debconf: unable to initialize frontend: Dialog
debconf: (No usable dialog-like program is installed, so the dialog based frontend cannot be used. at /usr/share/perl5/Debconf/FrontEnd/Dialog.pm line 79.)
debconf: falling back to frontend: Readline
Replacing config file /etc/perl/XML/SAX/ParserDetails.ini with new version
Processing triggers for libc-bin (2.39-0ubuntu8.3) ...
Processing triggers for ca-certificates-java (20240118) ...
Adding debian:ACCVRAIZ1.pem
Adding debian:AC_RAIZ_FNMT-RCM.pem
Adding debian:AC_RAIZ_FNMT-RCM_SERVIDORES_SEGUROS.pem
Adding debian:ANF_Secure_Server_Root_CA.pem
Adding debian:Actalis_Authentication_Root_CA.pem
Adding debian:AffirmTrust_Commercial.pem
Adding debian:AffirmTrust_Networking.pem
Adding debian:AffirmTrust_Premium.pem
Adding debian:AffirmTrust_Premium_ECC.pem
Adding debian:Amazon_Root_CA_1.pem
Adding debian:Amazon_Root_CA_2.pem
Adding debian:Amazon_Root_CA_3.pem
Adding debian:Amazon_Root_CA_4.pem
Adding debian:Atos_TrustedRoot_2011.pem
Adding debian:Atos_TrustedRoot_Root_CA_ECC_TLS_2021.pem
Adding debian:Atos_TrustedRoot_Root_CA_RSA_TLS_2021.pem
Adding debian:Autoridad_de_Certificacion_Firmaprofesional_CIF_A62634068.pem
Adding debian:BJCA_Global_Root_CA1.pem
Adding debian:BJCA_Global_Root_CA2.pem
Adding debian:Baltimore_CyberTrust_Root.pem
Adding debian:Buypass_Class_2_Root_CA.pem
Adding debian:Buypass_Class_3_Root_CA.pem
Adding debian:CA_Disig_Root_R2.pem
Adding debian:CFCA_EV_ROOT.pem
Adding debian:COMODO_Certification_Authority.pem
Adding debian:COMODO_ECC_Certification_Authority.pem
Adding debian:COMODO_RSA_Certification_Authority.pem
Adding debian:Certainly_Root_E1.pem
Adding debian:Certainly_Root_R1.pem
Adding debian:Certigna.pem
Adding debian:Certigna_Root_CA.pem
Adding debian:Certum_EC-384_CA.pem
Adding debian:Certum_Trusted_Network_CA.pem
Adding debian:Certum_Trusted_Network_CA_2.pem
Adding debian:Certum_Trusted_Root_CA.pem
Adding debian:CommScope_Public_Trust_ECC_Root-01.pem
Adding debian:CommScope_Public_Trust_ECC_Root-02.pem
Adding debian:CommScope_Public_Trust_RSA_Root-01.pem
Adding debian:CommScope_Public_Trust_RSA_Root-02.pem
Adding debian:Comodo_AAA_Services_root.pem
Adding debian:D-TRUST_BR_Root_CA_1_2020.pem
Adding debian:D-TRUST_EV_Root_CA_1_2020.pem
Adding debian:D-TRUST_Root_Class_3_CA_2_2009.pem
Adding debian:D-TRUST_Root_Class_3_CA_2_EV_2009.pem
Adding debian:DigiCert_Assured_ID_Root_CA.pem
Adding debian:DigiCert_Assured_ID_Root_G2.pem
Adding debian:DigiCert_Assured_ID_Root_G3.pem
Adding debian:DigiCert_Global_Root_CA.pem
Adding debian:DigiCert_Global_Root_G2.pem
Adding debian:DigiCert_Global_Root_G3.pem
Adding debian:DigiCert_High_Assurance_EV_Root_CA.pem
Adding debian:DigiCert_TLS_ECC_P384_Root_G5.pem
Adding debian:DigiCert_TLS_RSA4096_Root_G5.pem
Adding debian:DigiCert_Trusted_Root_G4.pem
Adding debian:Entrust.net_Premium_2048_Secure_Server_CA.pem
Adding debian:Entrust_Root_Certification_Authority.pem
Adding debian:Entrust_Root_Certification_Authority_-_EC1.pem
Adding debian:Entrust_Root_Certification_Authority_-_G2.pem
Adding debian:Entrust_Root_Certification_Authority_-_G4.pem
Adding debian:GDCA_TrustAUTH_R5_ROOT.pem
Adding debian:GLOBALTRUST_2020.pem
Adding debian:GTS_Root_R1.pem
Adding debian:GTS_Root_R2.pem
Adding debian:GTS_Root_R3.pem
Adding debian:GTS_Root_R4.pem
Adding debian:GlobalSign_ECC_Root_CA_-_R4.pem
Adding debian:GlobalSign_ECC_Root_CA_-_R5.pem
Adding debian:GlobalSign_Root_CA.pem
Adding debian:GlobalSign_Root_CA_-_R3.pem
Adding debian:GlobalSign_Root_CA_-_R6.pem
Adding debian:GlobalSign_Root_E46.pem
Adding debian:GlobalSign_Root_R46.pem
Adding debian:Go_Daddy_Class_2_CA.pem
Adding debian:Go_Daddy_Root_Certificate_Authority_-_G2.pem
Adding debian:HARICA_TLS_ECC_Root_CA_2021.pem
Adding debian:HARICA_TLS_RSA_Root_CA_2021.pem
Adding debian:Hellenic_Academic_and_Research_Institutions_ECC_RootCA_2015.pem
Adding debian:Hellenic_Academic_and_Research_Institutions_RootCA_2015.pem
Adding debian:HiPKI_Root_CA_-_G1.pem
Adding debian:Hongkong_Post_Root_CA_3.pem
Adding debian:ISRG_Root_X1.pem
Adding debian:ISRG_Root_X2.pem
Adding debian:IdenTrust_Commercial_Root_CA_1.pem
Adding debian:IdenTrust_Public_Sector_Root_CA_1.pem
Adding debian:Izenpe.com.pem
Adding debian:Microsec_e-Szigno_Root_CA_2009.pem
Adding debian:Microsoft_ECC_Root_Certificate_Authority_2017.pem
Adding debian:Microsoft_RSA_Root_Certificate_Authority_2017.pem
Adding debian:NAVER_Global_Root_Certification_Authority.pem
Adding debian:NetLock_Arany_=Class_Gold=_Főtanúsítvány.pem
Adding debian:OISTE_WISeKey_Global_Root_GB_CA.pem
Adding debian:OISTE_WISeKey_Global_Root_GC_CA.pem
Adding debian:QuoVadis_Root_CA_1_G3.pem
Adding debian:QuoVadis_Root_CA_2.pem
Adding debian:QuoVadis_Root_CA_2_G3.pem
Adding debian:QuoVadis_Root_CA_3.pem
Adding debian:QuoVadis_Root_CA_3_G3.pem
Adding debian:SSL.com_EV_Root_Certification_Authority_ECC.pem
Adding debian:SSL.com_EV_Root_Certification_Authority_RSA_R2.pem
Adding debian:SSL.com_Root_Certification_Authority_ECC.pem
Adding debian:SSL.com_Root_Certification_Authority_RSA.pem
Adding debian:SSL.com_TLS_ECC_Root_CA_2022.pem
Adding debian:SSL.com_TLS_RSA_Root_CA_2022.pem
Adding debian:SZAFIR_ROOT_CA2.pem
Adding debian:Sectigo_Public_Server_Authentication_Root_E46.pem
Adding debian:Sectigo_Public_Server_Authentication_Root_R46.pem
Adding debian:SecureSign_RootCA11.pem
Adding debian:SecureTrust_CA.pem
Adding debian:Secure_Global_CA.pem
Adding debian:Security_Communication_ECC_RootCA1.pem
Adding debian:Security_Communication_RootCA2.pem
Adding debian:Security_Communication_RootCA3.pem
Adding debian:Security_Communication_Root_CA.pem
Adding debian:Starfield_Class_2_CA.pem
Adding debian:Starfield_Root_Certificate_Authority_-_G2.pem
Adding debian:Starfield_Services_Root_Certificate_Authority_-_G2.pem
Adding debian:SwissSign_Gold_CA_-_G2.pem
Adding debian:SwissSign_Silver_CA_-_G2.pem
Adding debian:T-TeleSec_GlobalRoot_Class_2.pem
Adding debian:T-TeleSec_GlobalRoot_Class_3.pem
Adding debian:TUBITAK_Kamu_SM_SSL_Kok_Sertifikasi_-_Surum_1.pem
Adding debian:TWCA_Global_Root_CA.pem
Adding debian:TWCA_Root_Certification_Authority.pem
Adding debian:TeliaSonera_Root_CA_v1.pem
Adding debian:Telia_Root_CA_v2.pem
Adding debian:TrustAsia_Global_Root_CA_G3.pem
Adding debian:TrustAsia_Global_Root_CA_G4.pem
Adding debian:Trustwave_Global_Certification_Authority.pem
Adding debian:Trustwave_Global_ECC_P256_Certification_Authority.pem
Adding debian:Trustwave_Global_ECC_P384_Certification_Authority.pem
Adding debian:TunTrust_Root_CA.pem
Adding debian:UCA_Extended_Validation_Root.pem
Adding debian:UCA_Global_G2_Root.pem
Adding debian:USERTrust_ECC_Certification_Authority.pem
Adding debian:USERTrust_RSA_Certification_Authority.pem
Adding debian:XRamp_Global_CA_Root.pem
Adding debian:certSIGN_ROOT_CA.pem
Adding debian:certSIGN_Root_CA_G2.pem
Adding debian:e-Szigno_Root_CA_2017.pem
Adding debian:ePKI_Root_Certification_Authority.pem
Adding debian:emSign_ECC_Root_CA_-_C3.pem
Adding debian:emSign_ECC_Root_CA_-_G3.pem
Adding debian:emSign_Root_CA_-_C1.pem
Adding debian:emSign_Root_CA_-_G1.pem
Adding debian:vTrus_ECC_Root_CA.pem
Adding debian:vTrus_Root_CA.pem
done.
Setting up ecj (3.32.0+eclipse4.26-2) ...
Setting up default-jre-headless (2:1.21-75+exp1) ...
Processing triggers for ca-certificates (20240203) ...
Updating certificates in /etc/ssl/certs...
0 added, 0 removed; done.
Running hooks in /etc/ca-certificates/update.d...
done.
Processing triggers for ca-certificates-java (20240118) ...
done.
root@26ba92ad893d:/# wget https://downloads.openwrt.org/snapshots/targets/ramips/mt76x8/openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64.tar.zst
--2025-01-26 01:10:17--  https://downloads.openwrt.org/snapshots/targets/ramips/mt76x8/openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64.tar.zst
Resolving downloads.openwrt.org (downloads.openwrt.org)... 151.101.66.132, 151.101.2.132, 151.101.194.132, ...
Connecting to downloads.openwrt.org (downloads.openwrt.org)|151.101.66.132|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 30907448 (29M) [application/octet-stream]
Saving to: 'openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64.tar.zst'

openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64.tar.zs 100%[============================================================================================================================>]  29.48M  12.8MB/s    in 2.3s

2025-01-26 01:10:20 (12.8 MB/s) - 'openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64.tar.zst' saved [30907448/30907448]

root@26ba92ad893d:/# tar --zstd -xvf openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64.tar.zst
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/.targetinfo
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/package-pack.mk
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/cmake.mk
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/kernel.mk
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/site/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/site/armeb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/site/loongarch64
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/site/arm
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/site/i486
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/site/arc
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/site/m68k
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/site/i686
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/site/linux
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/site/x86_64
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/site/sparc
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/site/mipsel
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/site/mips64el
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/site/darwin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/site/powerpc64
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/site/powerpc
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/site/aarch64_be
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/site/riscv64
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/site/i386
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/site/mips
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/site/mips64
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/site/aarch64
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/version.mk
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/meson.mk
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/trusted-firmware-a.mk
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/kernel-6.6
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/debug.mk
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/feeds.mk
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/logo.svg
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/toolchain-build.mk
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/package-seccomp.mk
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/logo.png
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/host-build.mk
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/default-packages.mk
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/scan.mk
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/scan.awk
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/image.mk
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/kernel-build.mk
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/hardening.mk
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/depends.mk
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/device_table.txt
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/image-commands.mk
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/subdir.mk
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/verbose.mk
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/openssl-module.mk
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/package-defaults.mk
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/toplevel.mk
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/package-dumpinfo.mk
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/rootfs.mk
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/uclibc++.mk
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/u-boot.mk
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/nls.mk
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/kernel-version.mk
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/download.mk
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/kernel-defaults.mk
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/prereq-build.mk
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/target.mk
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/shell.sh
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/bpf.mk
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/hardened-ld-pie.specs
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/unpack.mk
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/prereq.mk
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/netfilter.mk
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/optee-os.mk
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/package.mk
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/autotools.mk
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/quilt.mk
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/include/package-bin.mk
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.dlink-sge-image.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/ldconfig
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/cpio
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.mkhash.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/mkmerakifw
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.mkzyxelzldfw.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/mkedimaximg
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/quilt
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.bcm4908asus.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/unlz4
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/mktplinkfw2
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/resize2fs
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.uimage_sgehdr.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.motorola-bin.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/bzip2recover
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/fix-u-media-header
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/mkcasfw
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/xiaomifw
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/fsck.fat
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.patch-dtb.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/locate
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/nosimg-enc
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/nand_ecc
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/seama
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/e2fsck
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/ccache
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/egrep
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/ubinize
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/patch
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.nec-enc.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.unstrip.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.mkenvimage.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/bzgrep
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/iptime-crc32
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.xorimage.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/texi2pdf
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.srcfiles.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.resize2fs.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/realpath
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/m4
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/xzdiff
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.e2fsck.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.fsck.ext2.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/srec2bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/lzcmp
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/mktplinkfw
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.buffalo-enc.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/findtextrel
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/mkmylofw
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/ccmake
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/mktitanimg
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.mkbrnimg.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/mkfs.fat
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/lzgrep
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.dgfirmware.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/autoconf
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.flock.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/mksenaofw
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.asustrx.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.bc.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.mksenaofw.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/patch-cmdline
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/fsck.ext3
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.oseama.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/mkdniimg
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/patchelf
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.e2mmpstatus.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.padjffs2.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/mkbuffaloimg
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/trx2usr
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.fsck.fat.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.ptgen.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.mkfs.ext4.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.elfcmp.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/lxlfw
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/patch-dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/file
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/mk_cmds
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.mkfs.ubifs.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.chattr.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/aclocal-1.12
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/apk
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/nec-enc
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.bcmclm.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/flex
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/ninja
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/lzma2eva
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.mkplanexfw.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.ubinize.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/add_header
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/bison
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/bzegrep
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/bzless
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/filefrag
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/aclocal-1.13
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/aclocal-1.14
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/autoscan
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/unsquashfs4
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/aclocal-1.11
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/updatedb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/mkdapimg
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.ninja.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/mkfs.ubifs
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/spw303v
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/mkplanexfw
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.e2freefrag.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.mktitanimg.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/grep
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.m4.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.mkedimaximg.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/srcfiles
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/libtoolize
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.buffalo-tftp.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/zstdgrep
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/elfclassify
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.mkzcfw.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/mkfs.ext2
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/mksercommfw
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.mkwrgimg.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.gengetopt.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/e4crypt
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/gzip
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/guards
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/trx2edips
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.nec-usbatermfw.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.ocspcheck.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/bcmclm
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/lzmainfo
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.mkdapimg2.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/edimax_fw_header
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/xzegrep
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.blkid.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/mkporayfw
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.imagetag.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.patchelf.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/mkhash
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/iptime-naspkg
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/openssl
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/aclocal
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/libdeflate-gzip
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/xzfgrep
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/lzfgrep
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/logsave
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.buffalo-tag.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.mklibs-readelf.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.dumpe2fs.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.otrx.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.sstrip.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/mkfs.ext3
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/bcm4908kernel
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.e4crypt.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.e2undo.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/sign_dlink_ru
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/zyimage
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.cpack.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/make_ext4fs
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.stack.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.mkchkimg.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.trx.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.mkheader_gemtek.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.mkzynfw.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/install
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/mkheader_gemtek
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/elfcmp
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/faked
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/which
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/xorimage
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/hexdump
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/mkhilinkfw
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.sign_dlink_ru.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/e2image
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.badblocks.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.mkwrggimg.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/chattr
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/mkdapimg2
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/stack
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/mke2fs
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/lzdiff
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/flock
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/mklibs
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.pc1crypt.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.mkdapimg.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/mklibs-readelf
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/xzgrep
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/debugfs
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.ucert.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.bzip2.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/mkzynfw
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/bash
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/xmlwf
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.xz.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.mke2fs.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/texi2dvi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/mkwrggimg
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/autoupdate
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.zyimage.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/pdftexi2dvi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.ctest.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.mkdlinkfw.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/asusuimage
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.mkfs.fat.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.mkdhpimg.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.dgn3500sum.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.add_header.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/e2label
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.mkmerakifw-old.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/mkdhpimg
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.wrt400n.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/mkfwimage
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.mkmylofw.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/mkbrncmdline
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.iptime-naspkg.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/lex
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.mkrtn56uimg.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/tar
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/cros-vbutil
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/e2scrub_all
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/lzma
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/avm-wasp-checksum
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/yacc
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/make-debug-archive
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/awk
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/seq
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/mkrasimage
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/mkcameofw
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/unstrip
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.mkcsysimg.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.zip.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.filefrag.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/dumpe2fs
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/oseama
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.findfs.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/autoheader
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/stat
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.nosimg-enc.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.seama.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/getopt
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/addpattern
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/unxz
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.lzop.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.mkbrncmdline.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/mkmerakifw-old
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/tune2fs
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/zstdless
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/usign
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/mmd
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/xzcmp
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/lzmadec
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.mcopy.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/aclocal-1.15
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/mkzyxelzldfw
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/libtool
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.bzip2recover.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/mkdlinkfw
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.fsck.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.mmd.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/rsync
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/sstrip
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.pkg-config.real.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.fsck.ext4.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/mkchkimg
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.apk.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/mkh3cimg
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.elfclassify.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/python3
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/diff
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.fwtool.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/dlink-sge-image
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/gcc
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/bzdiff
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/mkwrgimg
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/autom4te
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.mksercommfw.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.nand_ecc.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/lzmore
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/mkimage
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/mcopy
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/lz4
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/gengetopt
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/ptgen
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/bc
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/pkg-config.real
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.bcmblob.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/mkfwimage2
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/e2mmpstatus
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/dns313-header
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/mklibs-copy
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.mkmerakifw.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/help2man
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/xzmore
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.mkfs.ext3.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/makeinfo
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.fsck.ext3.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/elfcompress
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/uimage_padhdr
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.mkh3cimg.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/mksquashfs4
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/zstdcat
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.openssl.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.locate.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/zstd
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/bzfgrep
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/ifnames
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.lsattr.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/libdeflate-gunzip
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.zytrx.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.e2image.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.encode_crc.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/bcmblob
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/encode_crc
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/cpack
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/pc1crypt
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/lzegrep
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/motorola-bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.unsquashfs4.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/texi2any
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/bzcmp
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/mkbrnimg
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.cros-vbutil.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/wget
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/dgn3500sum
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.patch-cmdline.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/fatlabel
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/b43-asm
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/ucert
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.mklost+found.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/zyxbcm
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/bcm4908asus
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/automake
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.mkcasfw.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.findtextrel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/padjffs2
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/xzless
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/mklost+found
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.trx2edips.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.bcm4908kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.lzma2eva.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/fakeroot
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/jcgimage
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.uimage_padhdr.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/imagetag
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/zytrx
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.cpio.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.ccache.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.mkhilinkfw.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.mkfs.jffs2.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.mkfwimage2.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.hexdump.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/bzcat
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.edimax_fw_header.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.cmake.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/mkzcfw
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/lsattr
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/g++
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/mkenvimage
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.tune2fs.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.dns313-header.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.mkfwimage.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/findfs
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.srec2bin.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/uimage_sgehdr
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/automake-1.16
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.flex.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.mkfs.ext2.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.faked.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.lz4.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/perl
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.xiaomifw.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/e2scrub
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/nec-usbatermfw
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.logsave.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.asusuimage.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.avm-wasp-checksum.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.lxlfw.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.zstd.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/dc
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/hcsmakeimage
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/osbridge-crc
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.fix-u-media-header.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/trx
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.lzma.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.usign.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/xargs
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.hcsmakeimage.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/e4defrag
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/python
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.debugfs.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.fatlabel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/mkh3cvfs
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/tplink-safeloader
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/autoreconf
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.mkcameofw.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.mktplinkfw2.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/git
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/lz4c
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.libdeflate-gzip.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/find
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/mkfs.ext4
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/xxd
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/lzless
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/bzip2
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.mkimage.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/zstdmt
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/cp
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.b43-fwcutter.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/aclocal.real
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.lzmainfo.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.trx2usr.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/meson.py
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/badblocks
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.b43-asm.bin.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.addpattern.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/jshn
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/wrt400n
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.mksquashfs4.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/unzstd
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/xz
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/bzmore
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/fwtool
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/fsck.ext4
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.xzdec.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/otrx
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.jshn.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/elflint
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.e4defrag.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/mkrtn56uimg
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.spw303v.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/ocspcheck
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/buffalo-enc
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.tar.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/fsck.ext2
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/blkid
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/e2undo
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/asustrx
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.zyxbcm.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/texi2html
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/bunzip
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.make_ext4fs.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.bison.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.zycast.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/lz4cat
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/cmake
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/gnulib-tool
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/lzop
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.tplink-safeloader.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/mkcsysimg
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.jcgimage.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.sed.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.makeamitbin.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.dc.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/fsck
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.mkh3cvfs.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.xmlwf.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.iptime-crc32.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/b43-asm.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.elfcompress.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/e2freefrag
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/makeamitbin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.osbridge-crc.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/unzip
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.find.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.patch.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/buffalo-tftp
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.mkrasimage.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/zycast
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/b43-fwsquash.py
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/compile_et
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.linksys-addfwhdr.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.mktplinkfw.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/sed
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/mkfs.jffs2
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/xzdec
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/aclocal-1.16
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.elflint.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.mkporayfw.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/b43-fwcutter
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/pkg-config
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/linksys-addfwhdr
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.ccmake.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.lzmadec.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/buffalo-tag
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.xargs.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.mkdniimg.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.e2label.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/dgfirmware
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/ctest
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/.mkbuffaloimg.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/flex++
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/zip
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/bin/xzcat
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/lib/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/lib/libfakeroot-0.so
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/lib/librt.so.1
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/lib/libm.so.6
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/lib/libblobmsg_json.so
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/lib/libc.so.6
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/lib/libtinfo.so.6
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/lib/libubox.so
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/lib/libstdc++.so.6
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/lib/libncurses.so.6
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/lib/runas.so
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/lib/libfakeroot.so
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/lib/libdl.so.2
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/lib/libfakeroot.a
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/lib/libgcc_s.so.1
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/lib/ld-linux-x86-64.so.2
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/lib/libfakeroot.la
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/host/lib/libpthread.so.0
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/target-mipsel_24kc_musl/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/target-mipsel_24kc_musl/image/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/target-mipsel_24kc_musl/image/mt7628_rfb-u-boot-with-spl.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/target-mipsel_24kc_musl/image/mt7981_eeprom_mt7976_dbdc.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/staging_dir/target-mipsel_24kc_musl/image/mt7628_ravpower_rp-wd009-u-boot.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/.packageinfo
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/packages/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/packages/kernel-6.6.73~7303fbb67480e28a41c6ca21c6a94ce8-r1.apk
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/packages/README.md
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/packages/libc-1.2.5-r4.apk
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/packages/base-files-1651~6a7fa68569.apk
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/.config
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/keys/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/keys/openwrt-snapshots.pem
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/oraybox_x1-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_tplink_re365-v1.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/asus_rt-n11p-b1-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/asus_rt-n10p-v3-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_tplink_archer-c50-v6.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_wavlink_wl-wn575a3.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_xiaomi_mi-router-4a-100m-intl-v2.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/wavlink_wl-wn570ha1-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/comfast_cf-wr758ac-v1-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/totolink_a3-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_onion_omega2p.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_vocore_vocore2.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_jotale_js76x8-32m.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/tplink_tl-mr3420-v5-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/hak5_wifi-pineapple-mk7-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_tplink_tl-mr6400-v5.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_d-team_pbr-d1.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/tplink_tl-wr840n-v4-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_tplink_tl-mr3420-v5.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_wavlink_wl-wn577a2.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/alfa-network_awusfree1-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_zyxel_keenetic-extra-ii.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_yuncore_cpe200.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/mercury_mac1200r-v2-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/xiaomi_mi-router-4c-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_tplink_tl-wr841n-v14.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/rakwireless_rak633-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_glinet_microuter-n300.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_tplink_tl-wa801nd-v5.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_7links_wlr-1240.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_tplink_archer-mr200-v6.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_tplink_tl-wr902ac-v3.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_motorola_mwr03.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_xiaomi_mi-router-4a-100m.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/jotale_js76x8-16m-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/xiaomi_mi-router-4a-100m-intl-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/xiaomi_mi-router-4a-100m-intl-v2-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_wiznet_wizfi630s.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_totolink_lr1200.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_tplink_re200-v3.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/tplink_archer-c50-v3-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/wrtnode_wrtnode2p-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/unielec_u7628-01-16m-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/glinet_gl-mt300n-v2-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_keenetic_kn-3211.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_glinet_vixmini.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/zbtlink_zbt-we2426-b-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/wiznet_wizfi630s-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_tplink_tl-wr842n-v5.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_keenetic_kn-1613.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_keenetic_kn-1711.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_oraybox_x1.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_asus_rt-ac1200.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_kroks_kndrt31r19.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/tplink_tl-wa801nd-v5-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/zyxel_keenetic-extra-ii-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/hiwifi_hc5761a-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_wrtnode_wrtnode2r.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/netgear_r6080-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/iptime_a3-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_xiaomi_mi-router-4c.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/vmlinux.elf
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_tplink_re305-v1.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/duzun_dm06-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_yuncore_m300.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_tplink_re305-v3.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/yuncore_cpe200-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_hak5_wifi-pineapple-mk7.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_hiwifi_hc5661a.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/elecom_wrc-1167fs-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_tplink_tl-wr802n-v4.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_rakwireless_rak633.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_netgear_r6020.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_tplink_tl-mr3020-v3.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_glinet_gl-mt300n-v2.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/tplink_tl-wr841n-v13-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_wavlink_wl-wn570ha1.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/tplink_tl-wr840n-v5-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_asus_rt-n12-vp-b1.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/d-team_pbr-d1-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_widora_neo-16m.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_buffalo_wcr-1166ds.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/cudy_tr1200-v1-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/zbtlink_zbt-we1226-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/onion_omega2p-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_keenetic_kn-1713.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/minew_g1-c-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_tplink_tl-wr841n-v13.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/tplink_tl-mr6400-v5-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_zbtlink_zbt-we2426-b.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_xiaomi_mi-ra75.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_hilink_hlk-7688a.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_tplink_re205-v3.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/wavlink_wl-wn575a3-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_asus_rt-n11p-b1.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/asus_rt-ac1200-v2-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_netgear_r6120.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/xiaomi_miwifi-nano-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_tplink_tl-wr840n-v4.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/ravpower_rp-wd009-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_hiwifi_hc5611.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/ralink/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/ralink/mt7621.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/ralink/rt2880.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/ralink/vocore2.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/ralink/rt3052_eval.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/ralink/mt7620a_eval.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/ralink/mt7628a.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/ralink/mt7621-gnubee-gb-pc2.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/ralink/omega2p.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/ralink/gardena_smart_gateway_mt7688.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/ralink/rt3883_eval.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/ralink/rt3883.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/ralink/rt3050.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/ralink/mt7620a.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/ralink/mt7621-tplink-hc220-g5-v1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/ralink/mt7621-gnubee-gb-pc1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/ralink/rt2880_eval.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/ralink/Makefile
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/built-in.a
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/qca/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/qca/ar9331_tl_mr3020.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/qca/ar9132.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/qca/ar9331_omega.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/qca/ar9331_dragino_ms14.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/qca/ar9331_dpt_module.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/qca/ar9331_openembed_som9331_board.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/qca/ar9331.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/qca/Makefile
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/qca/ar9132_tl_wr1043nd_v1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/img/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/img/pistachio.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/img/boston.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/img/pistachio_marduk.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/img/Makefile
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/mti/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/mti/sead3.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/mti/malta.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/mti/Makefile
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/brcm/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/brcm/bcm63268.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/brcm/bcm97420c.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/brcm/bcm7360.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/brcm/bcm97346dbsmb.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/brcm/bcm3368.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/brcm/bcm6358-neufbox4-sercomm.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/brcm/bcm6362.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/brcm/bcm97358svmb.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/brcm/bcm7420.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/brcm/bcm6358.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/brcm/bcm7362.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/brcm/bcm7435.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/brcm/bcm7346.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/brcm/bcm97360svmb.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/brcm/bcm7125.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/brcm/bcm63268-comtrend-vr-3032u.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/brcm/bcm6362-neufbox6-sercomm.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/brcm/bcm3384_zephyr.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/brcm/bcm96368mvwg.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/brcm/bcm97362svmb.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/brcm/bcm97435svmb.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/brcm/bcm3384_viper.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/brcm/bcm93384wvg_viper.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/brcm/bcm3368-netgear-cvg834g.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/brcm/bcm9ejtagprb.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/brcm/bcm7425.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/brcm/bcm97125cbmb.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/brcm/bcm6328.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/brcm/bcm97425svmb.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/brcm/bcm97xxx-nand-cs1-bch4.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/brcm/bcm97xxx-nand-cs1-bch24.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/brcm/bcm93384wvg.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/brcm/bcm6368.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/brcm/Makefile
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/brcm/bcm7358.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/cavium-octeon/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/cavium-octeon/dlink_dsr-500n-1000n.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/cavium-octeon/dlink_dsr-1000n.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/cavium-octeon/octeon_3xxx.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/cavium-octeon/dlink_dsr-500n.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/cavium-octeon/octeon_68xx.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/cavium-octeon/octeon_3xxx.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/cavium-octeon/ubnt_e100.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/cavium-octeon/Makefile
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/ni/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/ni/169445.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/ni/Makefile
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/mscc/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/mscc/jaguar2_pcb111.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/mscc/serval.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/mscc/serval_pcb105.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/mscc/serval_common.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/mscc/ocelot_pcb120.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/mscc/jaguar2_common.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/mscc/jaguar2_pcb118.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/mscc/ocelot_pcb123.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/mscc/luton.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/mscc/ocelot.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/mscc/serval_pcb106.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/mscc/luton_pcb091.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/mscc/jaguar2_pcb110.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/mscc/jaguar2.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/mscc/Makefile
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/xilfpga/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/xilfpga/microAptiv.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/xilfpga/nexys4ddr.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/xilfpga/Makefile
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/ingenic/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/ingenic/qi_lb60.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/ingenic/x1000.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/ingenic/cu1000-neo.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/ingenic/jz4740.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/ingenic/jz4780.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/ingenic/gcw0_proto.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/ingenic/jz4770.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/ingenic/cu1830-neo.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/ingenic/jz4725b.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/ingenic/ci20.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/ingenic/gcw0.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/ingenic/x1830.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/ingenic/rs90.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/ingenic/Makefile
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/modules.order
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/realtek/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/realtek/rtl838x.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/realtek/cisco_sg220-26.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/realtek/rtl83xx.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/realtek/Makefile
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/lantiq/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/lantiq/danube.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/lantiq/danube_easy50712.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/lantiq/Makefile
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/pic32/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/pic32/pic32mzda_sk.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/pic32/pic32mzda.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/pic32/Makefile
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/loongson/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/loongson/loongson64-2k1000.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/loongson/loongson64g-package.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/loongson/rs780e-pch.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/loongson/loongson64c-package.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/loongson/loongson64g_4core_ls7a.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/loongson/loongson64c_4core_rs780e.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/loongson/loongson64c_4core_ls7a.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/loongson/loongson64v_4core_virtio.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/loongson/loongson64c_8core_rs780e.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/loongson/ls7a-pch.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/loongson/loongson64_2core_2k1000.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/loongson/Makefile
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/arch/mips/boot/dts/Makefile
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/.config
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/scripts/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/scripts/dtc/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/scripts/dtc/.dtc.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linux-6.6.73/scripts/dtc/dtc
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_comfast_cf-wr758ac-v1.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_asus_rt-n10p-v3.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/hiwifi_hc5861b-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/dlink_dap-1325-a1-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/netgear_r6120-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/linksys_e5400-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/tplink_re305-v3-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/hiwifi_hc5611-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_tplink_tl-mr6400-v4.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/tplink_re365-v1-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_comfast_cf-wr617ac.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_cudy_tr1200-v1.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/tplink_archer-c50-v6-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/vmlinux
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_wrtnode_wrtnode2p.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_tplink_archer-c50-v4.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_wavlink_wl-wn531a3.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/asus_rt-n12-vp-b1-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/wavlink_wl-wn578a2-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_xiaomi_miwifi-3c.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_minew_g1-c.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/tplink_tl-wr842n-v5-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/glinet_vixmini-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_hiwifi_hc5861b.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/iptime_a604m-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/hilink_hlk-7628n-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/glinet_microuter-n300-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/comfast_cf-wr758ac-v2-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/comfast_cf-wr617ac-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/keenetic_kn-1711-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/jotale_js76x8-8m-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/vocore_vocore2-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_tplink_re200-v4.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_wavlink_wl-wn578a2.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/7links_wlr-1230-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_tplink_tl-wr902ac-v4.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_netgear_r6080.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_tplink_re220-v2.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/xiaomi_mi-router-4a-100m-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/netgear_r6020-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_asus_rt-ac1200-v2.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/tplink_archer-c20-v5-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/jotale_js76x8-32m-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/xiaomi_miwifi-3c-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_comfast_cf-wr758ac-v2.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_jotale_js76x8-16m.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_widora_neo-32m.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/yuncore_m300-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_duzun_dm06.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/xiaomi_mi-ra75-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_xiaomi_mi-router-4a-100m-intl.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/tplink_archer-c20-v4-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/widora_neo-32m-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_tplink_archer-c20-v4.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/tplink_tl-mr3020-v3-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/cudy_wr1000-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/keenetic_kn-3211-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_ravpower_rp-wd009.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/buffalo_wcr-1166ds-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/tplink_re205-v3-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_alfa-network_awusfree1.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/widora_neo-16m-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_iptime_a3.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/vocore_vocore2-lite-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_tplink_archer-mr200-v5.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_skylab_skw92a.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_tplink_tl-wr840n-v5.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/hilink_hlk-7688a-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/tplink_re200-v2-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/tplink_archer-mr200-v6-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/tplink_re220-v2-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/tplink_tl-wr841n-v14-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/motorola_mwr03-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_mediatek_mt7628an-eval-board.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_kroks_kndrt31r16.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/tplink_tl-wr850n-v2-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_zbtlink_zbt-we1226.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/keenetic_kn-1613-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/tplink_re305-v1-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/skylab_skw92a-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/mediatek_mt7628an-eval-board-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/7links_wlr-1240-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/tplink_tl-wr802n-v4-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/wrtnode_wrtnode2r-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_vocore_vocore2-lite.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/wavlink_wl-wn531a3-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_xiaomi_miwifi-nano.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/tplink_archer-c50-v4-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_elecom_wrc-1167fs.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/hiwifi_hc5661a-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_totolink_a3.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_iptime_a604m.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/kroks_kndrt31r19-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/wavlink_wl-wn577a2-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_linksys_e5400.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_tplink_tl-wr850n-v2.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/onion_omega2-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_tplink_archer-c20-v5.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_tama_w06.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/tplink_tl-mr6400-v4-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_mediatek_linkit-smart-7688.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/mediatek_linkit-smart-7688-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_wavlink_wl-wn576a2.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_7links_wlr-1230.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_hiwifi_hc5761a.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/tplink_re200-v4-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_cudy_wr1000.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/tama_w06-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_hilink_hlk-7628n.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_mercury_mac1200r-v2.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/tplink_tl-wr902ac-v4-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_tplink_archer-c50-v3.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/tplink_re200-v3-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_dlink_dap-1325-a1.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/keenetic_kn-1713-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/kroks_kndrt31r16-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/totolink_lr1200-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_jotale_js76x8-8m.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/wavlink_wl-wn576a2-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_tplink_re200-v2.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_onion_omega2.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/tplink_tl-wr902ac-v3-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/asus_rt-ac1200-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/image-mt7628an_unielec_u7628-01-16m.dtb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/tplink_archer-mr200-v5-kernel.bin
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/base-files/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/base-files/etc/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/base-files/etc/inittab
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/base-files/etc/hotplug.d/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/base-files/etc/hotplug.d/usb/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/base-files/etc/hotplug.d/usb/10-motion
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/base-files/etc/uci-defaults/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/base-files/etc/uci-defaults/09_fix-checksum
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/base-files/etc/uci-defaults/04_led_migration
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/modules.mk
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/rt3883/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/rt3883/base-files/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/rt3883/base-files/lib/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/rt3883/base-files/lib/upgrade/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/rt3883/base-files/lib/upgrade/platform.sh
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/rt3883/base-files/lib/preinit/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/rt3883/base-files/lib/preinit/04_handle_checksumming
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/rt3883/base-files/etc/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/rt3883/base-files/etc/board.d/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/rt3883/base-files/etc/board.d/01_leds
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/rt3883/base-files/etc/board.d/02_network
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/rt3883/config-6.6
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/rt3883/target.mk
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/rt305x/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/rt305x/base-files/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/rt305x/base-files/lib/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/rt305x/base-files/lib/upgrade/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/rt305x/base-files/lib/upgrade/platform.sh
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/rt305x/base-files/lib/preinit/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/rt305x/base-files/lib/preinit/04_handle_checksumming
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/rt305x/base-files/etc/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/rt305x/base-files/etc/board.d/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/rt305x/base-files/etc/board.d/01_leds
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/rt305x/base-files/etc/board.d/02_network
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/rt305x/config-6.6
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/rt305x/target.mk
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_motorola_mwr03.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_tplink_eap235-wall-v1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_asus_rt-n10p-v3.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_youku_yk-l1c.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_netgear_ex6150.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt5350_belkin_f7c027.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt5350_olimex_rt5350f-olinuxino.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_elecom_wrc-1900gst.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_bolt_arion.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_buffalo_wsr-2533dhpl.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt5350_nexx_wt1520-8m.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_buffalo_wcr-1166ds.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_humax_e2.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3352_dlink_dir-620-d1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_gnubee_gb-pc1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_tplink_archer-x6-v3.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_d-team_pbr-d1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_trendnet_tew-810dr.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_alfa-network_awusfree1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_wavlink_wl-wn531a6.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_lenovo_newifi-y1s.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_gehua_ghl-r-001.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_tplink_tl-wpa8631p-v3.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_oraybox_x1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620n_kimax_u35wf.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3662_asus_rt-n56u.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_mqmaker_witi.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_mikrotik_routerboard-760igs.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_buffalo_wsr-2533dhpls.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_asus_rp-n53.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_iodata_wn-deax1800gr.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3662_samsung_cy-swr1100.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_xiaomi_mi-router-3g.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3052_hauppauge_broadway.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt5350_dlink_dwr-512-b.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_xiaomi_miwifi-mini.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt5350_tenda_3g150b.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_tplink_re650-v1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620n_dlink_dwr-116-a1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_youhua_wr1200js.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_edimax_re23s.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_tplink_tl-wr802n-v4.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_wavlink_wl-wn531g3-a2.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_phicomm_psg1218b.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_dlink_dir-882-r1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_dlink_dir-1960-a1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_tplink_re200-v2.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_glinet_vixmini.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_planex_mzk-ex300np.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt5350_hame_mpr-a2.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_snr_snr-cpe-me2-lite.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_netgear_r6900-v2.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_lava_lr-25g001.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_netgear_wndr3700-v5.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_xiaomi_miwifi-3c.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_cudy_x6.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_iptime_a3.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_zbtlink_zbt-wg3526-16m.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt2880_buffalo_wzr-agl300nh.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_linksys_re7000.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3662_omnima_hpm.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_iodata_wn-ac1167gr.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_linksys_ea7500-v2.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_iptime.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3052_tenda_w306r-v2.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_phicomm_k2-v22.4.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_mtc_wr1201.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_tplink_archer-c50-v1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_tplink_re200-v4.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_iptime_a1004ns.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt5350_wansview_ncs601w.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_engenius_esr600.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_dovado_tiny-ac.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3050_dlink_dir-600-b1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_elecom_wrc-1750gsv.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_tplink_mr600-v2-eu.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_zbtlink_zbt-we1026.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_bdcom_wap2100-sk.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_zyxel_keenetic-viva.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_confiabits_mt7621-v1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_zbtlink_zbt-we826-32m.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_asus_rt-ac1200-v2.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_dlink_dap-1325-a1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_tplink_tl-wa801nd-v5.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_yuncore_g720.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3050_arcwireless_freestation5.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_phicomm_k2x.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_planex_db-wrt01.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_dlink_dir-2640-a1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_zbtlink_zbt-wg1602-v04-16m.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_h3c_tx180x.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_sim_simax1800t.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_iodata_wn-dx2033gr.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_netis_n6.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_wavlink_wl-wn535k1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_jotale_js76x8.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_raisecom_msg1500-x-00.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_xiaomi_mi-router-4.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3050_alphanetworks_asl26555.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_ubnt_usw-flex.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3052_upvel_ur-326n4g.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3352_zyxel_nbg-419n-v2.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_winstars_ws-wn536p3.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt5350_zyxel_keenetic-start.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3052_planex_mzk-wdpr.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_widora_neo-16m.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_huasifei_ws1208v2.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_zbtlink_zbt-wg3526.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_jotale_js76x8-32m.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_cudy_m1800.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_edimax_ew-7476rpc.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_phicomm_k2g.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_zbtlink_zbt-we826-16m.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_glinet_microuter-n300.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_hanyang_hyc-g920.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt5350_nexx_wt1520.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_cudy_wr1300-v2v3.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_tplink_tl-mr6400-v4.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620n_sunvalley_filehub.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_buffalo_wsr-1166dhp.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_linksys_e7350.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3052_aximcom_mr-102n.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_microduino_microwrt.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_zbtlink_zbt-wg1608-16m.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_netgear_r6700-v2.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_yuncore_cpe200.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_openfi_5pro.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_linksys_ea8100-v2.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_zbtlink_zbt-we1326.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3883_belkin_f9k1109v1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_netgear_wac124.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_cudy_x6-v2.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3052_mofinetwork_mofi3500-3gn.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3052_omnima_miniembwifi.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_hiwifi_hc5962.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_tplink_tl-mr3420-v5.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_wifire_s1500-nbn.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_iodata_wn-xx-xr.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_wavlink_wl-wn570ha1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_xiaomi_mi-router-cr6609.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_tplink_archer-c20-v4.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_wodesys_wd-r1802u.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3052_nexaira_bc2.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_ubnt_unifi-6-lite.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_arcadyan_wg4xx223.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_keenetic_kn-3510.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_phicomm_k2-v22.5.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_hiwifi_hc5611.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_xiaomi_mi-router-cr6608.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_snr_snr-cpe-me2-sfp.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_asiarf_ap7621.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_asus_rt-n56u-b1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_cudy_x6-v1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_zbtlink_zbt-wg1608-32m.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_asus_rt-n11p-b1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_elecom_wmc-m1267gst2.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_xiaomi_mi-router-4a-100m-intl.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_linksys_ea7300-v1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_wavlink_wl-wn577a2.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_zyxel_lte5398-m904.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620n_snr_cpe-w4n-mt.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_dlink_dir-860l-b1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_dlink_dra-1360-a1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_dlink_dir-853-r1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_planex_cs-qr10.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_zbtlink_zbt-wg1602-v04.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_wevo_air-duo.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt5350_planex_mzk-dp150n.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_linksys_ea7300-v2.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_netgear_wn3000rp-v3.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_samknows_whitebox-v8.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_jcg_q20.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_glinet_gl-mt750.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3052_zyxel_keenetic.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3052_upvel_ur-336un.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_wavlink_wl-wn573hx1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_xiaomi_mi-router-4.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt5350_poray_x5.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_iodata_wn-dx1200gr.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt2880.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_netgear_r6800.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_comfast_cf-e390ax.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_zbtlink_zbt-we1026-h-32m.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3052_sitecom_wl-351.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3052_argus_atp-52b.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_firefly_firewrt.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620n_ravpower_rp-wd03.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_netgear_ex3x00_ex61xx.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_domywifi_dm202.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3662_edimax_br-6475nd.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_tplink_re220-v2.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620n_dlink_dwr-921-c1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3052_engenius_esr-9753.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_ampedwireless_ally-r1900k.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_alfa-network_ac1200rm.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_cudy_tr1200-v1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620n_kingston_mlwg2.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_sanlinking_d240.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt5350_omnima_miniembplug.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_netgear_ex6130.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_tplink_tl-wr841n-v14.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3052_unbranded_xdx-rn502j.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_widora_neo.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_rakwireless_rak633.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_sercomm_dxx_nand_256m.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3050_dlink_dir-620-a1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_mikrotik_routerboard-m11g.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_dlink_dir-8xx.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_dlink_dir-3060-a1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_elecom_wrc-2533gst.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_zbtlink_zbt-we826.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620n_netgear_pr2000.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt5350_easyacc_wizard-8800.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_asiarf_ap7621-001.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_afoundry_ew1200.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_netgear_r6220.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_zbtlink_zbt-we2426-b.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_minew_g1-c.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_zyxel_wap6805.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3052_buffalo_whr-g300n.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3052_jcg_jhr-n926r.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3352.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620n_zyxel_keenetic-omni-ii.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_domywifi.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_xiaomi_mi-router-4a-3g-v2.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3050_sparklan_wcr-150gn.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_totolink_a3.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620n_buffalo_wmr-300.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_sercomm_na502s.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_tplink_eap615-wall-v1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_tplink_archer-c5-v4.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_humax_e10.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt2880_dlink_dap-1522-a1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_hiwifi_hc5761a.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_kroks_kndrt31r19.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_d-team_newifi-d2.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_tplink_archer-c6-v3.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_asus_rt-ac65p.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt5350_trendnet_tew-714tru.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt2880_buffalo_wli-tx4-ag300n.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_zyxel_nwa50ax.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3052_accton_wr6202.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_kroks.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3050_allnet_all0256n-4m.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3052_tenda_3g300m.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt2880_airlink101_ar670w.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_comfast_cf-ew72-v2.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_dlink_dir-810l.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_iptime_t5004.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_xiaomi_mi-router-4c.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_etisalat_s3.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_engenius_epg600.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt5350_nixcore_x1.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_unielec_u7628-01-16m.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_jcg_y2.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_xiaomi_miwifi-nano.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_tplink_tl-wr842n-v5.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3050_dlink_dcs-930.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_totolink_x5000r.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_unielec_u7621-06.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_renkforce_ws-wn530hp3-a.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3352_allnet_all5002.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_tplink_archer-c50-v3.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_tplink_8m.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_tplink_archer-c2-v1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_ubnt_edgerouter-x.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt5350_hame_mpr-a1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_ohyeah_oy-0001.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt5350_zyxel_keenetic-4g-b.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_elecom_wrc-1167fs.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_unielec_u7621-06-64m.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_tenbay_t-mb5eu-v01.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_tplink_re2x0-v1.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_planex_mzk-750dhp.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_dlink_dir-806a-b1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_rostelecom_s1010.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt2880_ralink_v11st-fe.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt5350_nexx_wt1520-4m.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620n_zbtlink_zbt-wa05.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_snr_snr-cpe-me1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_dlink_dir-2660-a1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_rostelecom_rt-fe-1a.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_vocore_vocore2.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_lenovo_newifi-y1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_cudy_wr1300-v3.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt5350_7links_px-4885.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_ampedwireless_ally-00x19k.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt5350_vocore_vocore.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_tplink_re210-v1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt5350_asiarf_awm002-evb-8m.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3052_prolink_pwh2004.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_zbtlink_zbt-wexx26.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_linksys_e5600.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_tplink_archer-c50-v4.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt5350_nixcore_x1-16m.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_oraybox_x3a.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_rostelecom_rt-fl-1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_netgear_wn3100rp-v2.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3050_huawei_d105.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_sercomm_s1500.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_asus_rt-ac1200.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_netgear_r7450.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3050_teltonika_rut5xx.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_wevo_11acnas.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_dna_valokuitu-plus-ex400.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_sercomm_na502.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3050_allnet_all0256n.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_wevo_w2914ns-v2.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_h3c_tx1806.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt5350_wiznet_wizfi630a.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt5350_7links_px-4885-8m.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_meig_slt866.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_dlink_dwr-96x.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_xiaomi_mi-router-4a-gigabit.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3050_8devices_carambola.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_netgear_ex2700.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3052_unbranded_wr512-3gn-4m.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3052_skyline_sl-r7205.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_buffalo_wsr-600dhp.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_wavlink_wl-wn576a2.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_dlink_dir-1935-a1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_dlink_dir-878-a1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_zyxel_nwa-ax.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3662_engenius_esr600h.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_zbtlink_zbt-we1026-h.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_ruijie_rg-ew1200g-pro-v1.1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_wevo_w2914ns-v2.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_tozed_zlt-s12-pro.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt5350_vocore_vocore-8m.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3352_dlink_dir-615-h1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt5350_dlink_dir-320-b1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3052_aztech_hw550-3g.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_elecom_wrc-gs-2pci.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt5350.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_dlink_dap-1620-b1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_tplink_re500-v1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_edimax_rx21s.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt5350_dlink_dir-610-a1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_asus_rp-ac87.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_tplink_archer-c20i.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_storylink_sap-g3200u3.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_youku_yk-l2.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_asus_rt-n1x.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt2880_asus_rt-n15.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_wavlink_wl-wn530hg4.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_linksys_ea7xxx.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_zio_freezio.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_iptime_a3004t.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_bolt_bl100.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt2880_belkin_f5d8235-v1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620n_nexx_wt3020-8m.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_hiwifi_hc5861b.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3050_trendnet_tew-638apb-v2.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_glinet_gl-mt300n.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_wavlink_wl-wn53xax.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_iodata_wn-dx1167r.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_netgear_wax202.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_zbtlink_zbt-ape522ii.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620n_zbtlink_zbt-wr8305rt.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_netis_wf2770.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_duzun_dm06.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3052_dlink_dap-1350.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_hak5_wifi-pineapple-mk7.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt5350_poray_m3.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_tplink_archer-c50-v6.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt5350_dlink_dcs-930l-b1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_comfast_cf-wr758ac.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_zbtlink_zbt-we1026-5g-16m.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3883_trendnet_tew-691gr.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_beeline_smartbox-giga.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_wavlink_ws-wn572hp3-4g.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3050_alphanetworks_asl26555-16m.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_ampedwireless_ally.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_gemtek_wvrtm-127acn.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_xiaomi_mi-router-4a-100m-intl-v2.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3052_planex_mzk-w300nh2.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3052_alfa-network_w502u.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_netcore_nw5212.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_asiarf_ap7621-nv1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_sitecom_wlr-4100-v1-002.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_wavlink_wl-wn575a3.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620n_netgear_jwnr2010-v5.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_jcg_jhr-ac876m.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620n_netgear_n300.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_keenetic_kn-1711.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_dlink_dch-m225.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_netgear_r6080.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_buffalo_wsr-2533dhpl2.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_fon_fon2601.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_domywifi_dm203.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_ubnt_unifi-flexhd.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_d-team_pbr-m1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_rostelecom_rt-sf-1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_tplink_ec330-g5u-v1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_beeline_smartbox-turbo.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_alfa-network_r36m-e4g.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_dlink_dir-2055-a1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_edimax_ra21s.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_onion_omega2.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_dlink_dir-3040-a1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_yuncore_ax820.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3052_asus_rt-n13u.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_tplink_archer-a6-v3.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_telco-electronics_x1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3052_unbranded_wr512-3gn-8m.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_buffalo_whr-1166d.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_glinet_gl-mt300n-v2.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_netgear_sercomm_ayx.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3050_netcore_nw718.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_jotale_js76x8-16m.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_xiaomi_nand_128m.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3050_asus_wl-330n.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_hnet_c108.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_wavlink_wl-wn531g3.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_head-weblink_hdrm200.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_comfast_cf-wr617ac.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_ampedwireless_b1200ex.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_asus_rt-ac1200.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_cameo_810.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_hiwifi_hc5x61a.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt5350_nixcore_x1-8m.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt5350_asiarf_awm002-evb.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_linksys_ea8100-v1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_hiwifi_hc5661.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_elecom_wmc-x1800gst.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620n_comfast_cf-wr800n.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_elecom_wrc-gs.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_netgear_r6xxx.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_tplink_archer-ax23-v1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt5350_7links_px-4885-4m.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_wrtnode_wrtnode2p.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_xiaoyu_xy-c5.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_glinet_vixmini_microuter.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_wavlink_wl-wn531a3.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_tplink_tl-wr840n-v5.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_asus_rt-n12-vp-b1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_zyxel_nr7101.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620n_zbtlink_zbt-we2026.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_zbtlink_zbt-we3526.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_aigale_ai-br100.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_tplink_re200-v3.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_h3c_tx1800-plus.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_netis_wf2881.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_buffalo_whr-300hp2.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_asus_rt-ac5x.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_cudy_wr2100.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_tplink_re205-v3.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt5350_poray_x8.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt5350_dlink_dir-300-b7.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_dlink_flash-16m-a1.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620n_zbtlink_zbt-cpe102.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_ubnt_edgerouter-x.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_zbtlink_zbt-we1226.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_mikrotik_routerboard-7xx.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_unielec_u7621-06-32m.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_dlink_covr-x1860-a1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620n_dlink_dwr-922-e2.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_elecom_wrc-1167gst2.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3883_sitecom_wlr-6000.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_tplink_ex220-v1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_iptime_a104ns.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_gnubee_gb-pc2.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_unielec_u7628-01.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_tplink_rexx0-v1.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_tplink_re350-v1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_netgear_eax12.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_xiaomi_mi-router-3-pro.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_iptime_a3004ns-dual.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_linksys_re6500.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3352_zte_mf283plus.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_zyxel_wsm20.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_keenetic_kn-1910.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_haier-sim_wr1800k.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_edimax_rg21s.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_asus_rt-ac54u.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_elecom_wrc-1167ghbk2-s.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3662_loewe_wmdr-143n.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_tplink_8m-split-uboot.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_ralink_mt7620a-mt7610e-evb.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3050_dlink_dir-615-d.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_tplink_tl-wr840n-v4.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt5350_allnet_all5003.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_dlink_dir-2150-r1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_iptime_a3002mesh.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_cudy_wr1300-v1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_wrtnode_wrtnode2.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_ubnt_edgerouter-x-sfp.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_mikrotik.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3052_ralink_v22rw-2x2.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_ubnt_unifi-nanohd.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_xzwifi_creativebox-v1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt5350_poray_m4-8m.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_ralink_mt7620a-v22sg-evb.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_phicomm_psg1208.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_xiaomi_mi-router-3g-v2.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3883.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_iodata_wn-ax1167gr.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_keenetic_kn-3211.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_belkin_rt1800.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_netgear_wax214v2.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_mikrotik_ltap-2hnd.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_ralink_mt7620a-mt7530-evb.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_netgear_ex6120.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_ralink_mt7620a-evb.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_tplink_re200-v1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3052_fon_fonera-20n.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_dlink_dir-882-a1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_lenovo_newifi-y1.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_zyxel_lte3301-plus.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_unielec_u7621-01-16m.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_phicomm_k2p.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_keenetic_kn-3010.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620n_vonets_var11n-300.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_asus_rt-ax54.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_iodata_wnpr2600g.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_linksys_e5400.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_jotale_js76x8-8m.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_onion_omega2p.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3052_petatel_psr-680w.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_iptime_ax2004m.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt5350_hootoo_ht-tm02.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_gemtek_wvrtm-1xxacn.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_yukai_bocco.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_zbtlink_zbt-wg2626.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_netgear_r6260.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620n_asus_rt-n12p.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3883_trendnet_tew-692gr.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_asus_rt-ax53u.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_asus_rp-ac56.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_kimax_u25awf-h1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620n_zyxel_keenetic-omni.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_tplink_tl-mr6400-v5.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_xiaomi_redmi-router-ac2100.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_unielec_u7621-01.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3050.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_alfa-network_tube-e4g.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_netgear_r7200.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_tplink_archer-c6u-v1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_tplink_archer-mr200.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_asus_rt-acx5p.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_elecom_wrc-2533ghbk2-t.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620n_nexx_wt3020-4m.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_hilink_hlk-7621a-evb.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_keenetic_kn-1613.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3050_asus_wl-330n3g.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_mikrotik_routerboard-m33g.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_dlink_dir-867-a1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt5350_unbranded_a5-v11.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_buffalo_wsr-2533dhplx.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_arcadyan_we420223-99.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_beeline_smartbox-pro.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_linksys_ea6350-v4.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_elecom_wrc-1750gs.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_mikrotik_routerboard-750gr3.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_wavlink_wl-wn579x3.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_youku_yk-l1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_mercury_mac1200r-v2.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_elecom_wrc-2533ghbk-i.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_tplink_er605-v2.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_xiaomi_mi-router-cr660x.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620n_zyxel_keenetic-lite-iii-a.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_asus_rt-ac57u-v1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_skylab_skw92a.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_elecom_wmc-s1267gs2.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_tplink_tl-wr902ac-v4.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_youku_yk-l1.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_beeline_smartbox-flash.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_netgear_r6850.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_edimax_ew-747x.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_elecom_wrc-gs-1pci.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620n_elecom_wrh-300cr.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_edimax_br-6208ac-v2.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620n_hootoo_ht-tm05.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_edimax_br-6478ac-v2.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_tplink_re305-v3.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_totolink_a7000r.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_elecom_wxc-x1800gsx.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_asus_4g-ax56.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_hiwifi_hc5x61.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_winstars_ws-wn583a6.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_xiaomi_router-ac2100.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_iodata_wn-ac733gr3.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_tama_w06.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_7links_wlr-12xx.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_wavlink_wl-wn578a2.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt2880_airlink101_ar725w.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3050_tenda_w150m.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_hiwifi_hc5861.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_iodata_wn-gx300gr.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_kroks_kndrt31r16.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_iodata_wn-ax2033gr.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_vocore_vocore2.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_lb-link_bl-w1200.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_dlink_dap-x1860-a1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_iptime_a6004ns-m.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_zbtlink_zbt-we1026-5g.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_thunder_timecloud.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_tplink_archer-mr200-v5.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3050_asus_rt-n10-plus.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_planex_mzk-ex750np.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3050_allnet_all0256n-8m.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_zbtlink_zbt-wg1608.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt5350_poray_m4-4m.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt5350_asiarf_awm002-evb-4m.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3050_edimax_3g-6200nl.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_tplink_deco-m4r-v4.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_netgear_sercomm_bzv.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620n_kingston_mlw221.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_cudy_wr1000.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_ubnt_unifi.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_zbtlink_zbt-wg1602.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_comfast_cf-wr758ac-v2.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_elecom_wsc-x1800gs.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_netgear_sercomm_chj.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_mts_wg430223.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_netgear_r6020.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_elecom_wrc-1167gs2-b.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_trendnet_tha103ac.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_xiaomi_mi-router-ac2100.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_dlink_dir_nand_128m.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_dlink_dir-853-a1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_xiaomi_mi-router-4a-common.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_alfa-network_quad-e4g.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_z-router_zr-2660.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3052_zyxel_nbg-419n.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_iptime_a604m.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_mediatek_mt7621-eval-board.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_netgear_r6120.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_dlink_flash-16m-r1.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_tplink_tl-wr850n-v2.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_tplink_eap613-v1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_tplink_re650-v2.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt5350_hilink_hlk-rm04.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_tplink_re305-v1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_dlink_dir-878-r1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3052_asiarf_awapn2403.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_iptime_a6004ns-m.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_tplink_re200.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_tplink_tl-mr3020-v3.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_hiwifi_hc5661a.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_wavlink_wl-wn533a8.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_elecom_wrc-2533gs2.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_wiznet_wizfi630s.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_tplink_tl-wr841n-v13.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_iptime_a8004t.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_keenetic_kn-1713.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_dlink_dwr-118-a1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt5350_intenso_memory2move.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_beeline_smartbox-turbo-plus.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_ravpower_rp-wd009.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3052_jcg_jhr-n825r.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_edimax_ew-7478apc.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt5350_zyxel_keenetic-lite-b.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_zbtlink_zbt-we826-e.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3052_poray_ip2202.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_wrtnode_wrtnode2r.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3052_netgear_wnce2001.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_zbtlink_zbt-wg1602-v04-32m.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_tplink_8m.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3050_edimax_3g-6200n.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_glinet_gl-mt300a.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_dlink_dxx-1xx0-x1.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_mediatek_linkit-smart-7688.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_mercusys_mr70x-v1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620n_asus_rt-n14u.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_xiaomi_mi-router-4a-gigabit-v2.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3050_asus_rt-g32-b1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_dlink_dir-853-a3.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_mediatek_mt7628an-eval-board.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3050_jcg_jhr-n805r.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt5350_vocore_vocore-16m.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_dlink_dwr-960.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt5350_airlive_air3gii.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_hilink_hlk-7628n.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_jdcloud_re-cp-02.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_gemtek_wvrtm-130acn.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_7links_wlr-1230.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_mediatek_ap-mt7621a-v60.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_elecom_wrc-1750gst2.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_zyxel_keenetic-extra-ii.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_sercomm_na930.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_xiaomi_mi-router-cr6606.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt5350_olimex_rt5350f-olinuxino-evb.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_yuncore_fap640.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_asus_rt-ac85p.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_onion_omega2.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_tplink_tl-wr902ac-v3.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3662_dlink_dir-645.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_adslr_g7.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_yuncore_fap690.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_tplink_re305.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_hilink_hlk-7688a.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_zbtlink_zbt-wg3526-32m.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_asus_rt-ac51u.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_iptime.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt5350_poray_m4.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_widora_neo-32m.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3050_dlink_dir-300-b1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_tplink_ec220-g5-v2.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_lenovo_newifi-d1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_xiaomi_mi-router-4a-100m.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_zyxel_nwa55axe.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_iodata_wn-ax1167gr2.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_planex_vr500.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_xiaomi_mi-ra75.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_h3c_tx1801-plus.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_comfast_cf-wr758ac-v1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_glinet_gl-mt1300.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_haier_har-20s2u1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_buffalo_whr-600d.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_iptime_a6ns-m.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_domywifi_dw22d.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_dlink_dir-510l.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620n_nexx_wt3020.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3052_huawei_hg255d.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_youku_x2.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3050_alphanetworks_asl26555-8m.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_netgear_r6350.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt5350_olimex_rt5350f-olinuxino.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_yuncore_m300.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620n_wrtnode_wrtnode.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_alfa-network_ax1800rm.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_netgear_ex3700.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_zbtlink_zbt-wg1602-16m.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt5350_zorlik_zl5900v2.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3052_unbranded_wr512-3gn.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_netgear_wac104.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_tplink_archer-c20-v5.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_edimax_ew-7478ac.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_tplink_archer-c20-v1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_dual-q_h721.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_elecom_wrc-x1800gs.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/rt3052_belkin_f5d8235-v2.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_dlink_dwr-961-a1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_tplink_re365-v1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_linksys_e1700.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_dlink_dwr-118-a2.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_elecom_wrc-2533ghbk.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_sercomm_cpj.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_totolink_lr1200.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_elecom_wrc-2533gst2.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_zte_q7.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_vocore_vocore2-lite.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_7links_wlr-1240.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7628an_tplink_archer-mr200-v6.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_unielec_u7621-06-16m.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620n.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_cudy_wr1300-v2.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_hiwifi_hc5761.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7620a_netgear_wn3x00rp.dtsi
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/dts/mt7621_dlink_dir-2150-a1.dts
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/rt288x/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/rt288x/base-files/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/rt288x/base-files/lib/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/rt288x/base-files/lib/upgrade/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/rt288x/base-files/lib/upgrade/platform.sh
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/rt288x/base-files/etc/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/rt288x/base-files/etc/board.d/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/rt288x/base-files/etc/board.d/01_leds
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/rt288x/base-files/etc/board.d/02_network
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/rt288x/config-6.6
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/rt288x/target.mk
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt7620/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt7620/base-files/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt7620/base-files/lib/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt7620/base-files/lib/upgrade/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt7620/base-files/lib/upgrade/platform.sh
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt7620/base-files/etc/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt7620/base-files/etc/hotplug.d/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt7620/base-files/etc/hotplug.d/ieee80211/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt7620/base-files/etc/hotplug.d/ieee80211/10_fix_wifi_mac
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt7620/base-files/etc/hotplug.d/firmware/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt7620/base-files/etc/hotplug.d/firmware/10-rt2x00-eeprom
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt7620/base-files/etc/uci-defaults/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt7620/base-files/etc/uci-defaults/05_fix-compat-version
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt7620/base-files/etc/init.d/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt7620/base-files/etc/init.d/bootcount
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt7620/base-files/etc/board.d/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt7620/base-files/etc/board.d/01_leds
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt7620/base-files/etc/board.d/03_gpio_switches
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt7620/base-files/etc/board.d/02_network
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt7620/config-6.6
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt7620/target.mk
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/image/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/image/mt7621.mk
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/image/rt288x.mk
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/image/mt76x8.mk
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/image/common-tp-link.mk
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/image/rt3883.mk
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/image/rt305x.mk
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/image/common-sercomm.mk
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/image/lzma-loader/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/image/lzma-loader/src/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/image/lzma-loader/src/head.S
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/image/lzma-loader/src/loader.c
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/image/lzma-loader/src/loader.lds
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/image/lzma-loader/src/board.c
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/image/lzma-loader/src/printf.c
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/image/lzma-loader/src/cache.c
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/image/lzma-loader/src/cacheops.h
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/image/lzma-loader/src/loader2.lds
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/image/lzma-loader/src/LzmaDecode.h
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/image/lzma-loader/src/LzmaTypes.h
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/image/lzma-loader/src/LzmaDecode.c
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/image/lzma-loader/src/printf.h
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/image/lzma-loader/src/lzma-data.lds
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/image/lzma-loader/src/config.h
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/image/lzma-loader/src/cp0regdef.h
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/image/lzma-loader/src/cache.h
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/image/lzma-loader/src/Makefile
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/image/lzma-loader/Makefile
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/image/mt7620.mk
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/image/Makefile
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt76x8/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt76x8/base-files/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt76x8/base-files/lib/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt76x8/base-files/lib/upgrade/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt76x8/base-files/lib/upgrade/platform.sh
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt76x8/base-files/etc/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt76x8/base-files/etc/hotplug.d/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt76x8/base-files/etc/hotplug.d/ieee80211/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt76x8/base-files/etc/hotplug.d/ieee80211/10_fix_wifi_mac
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt76x8/base-files/etc/init.d/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt76x8/base-files/etc/init.d/bootcount
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt76x8/base-files/etc/board.d/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt76x8/base-files/etc/board.d/01_leds
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt76x8/base-files/etc/board.d/02_network
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt76x8/base-files/etc/board.d/04_led_migration
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt76x8/config-6.6
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt76x8/target.mk
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt7621/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt7621/base-files/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt7621/base-files/sbin/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt7621/base-files/sbin/fixup-mac-address
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt7621/base-files/lib/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt7621/base-files/lib/upgrade/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt7621/base-files/lib/upgrade/buffalo.sh
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt7621/base-files/lib/upgrade/platform.sh
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt7621/base-files/lib/upgrade/ubnt.sh
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt7621/base-files/lib/upgrade/iodata.sh
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt7621/base-files/lib/upgrade/dna.sh
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt7621/base-files/lib/upgrade/zyxel.sh
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt7621/base-files/lib/preinit/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt7621/base-files/lib/preinit/04_set_netdev_label
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt7621/base-files/etc/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt7621/base-files/etc/hotplug.d/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt7621/base-files/etc/hotplug.d/ieee80211/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt7621/base-files/etc/hotplug.d/ieee80211/10_fix_wifi_mac
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt7621/base-files/etc/hotplug.d/ieee80211/05-wifi-migrate
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt7621/base-files/etc/hotplug.d/firmware/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt7621/base-files/etc/hotplug.d/firmware/10-ath9k-eeprom
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt7621/base-files/etc/hotplug.d/firmware/11-mt76-caldata
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt7621/base-files/etc/uci-defaults/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt7621/base-files/etc/uci-defaults/09_fix_crc
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt7621/base-files/etc/uci-defaults/01_enable_packet_steering
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt7621/base-files/etc/uci-defaults/04_led_migration
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt7621/base-files/etc/init.d/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt7621/base-files/etc/init.d/bootcount
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt7621/base-files/etc/board.d/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt7621/base-files/etc/board.d/01_leds
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt7621/base-files/etc/board.d/03_gpio_switches
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt7621/base-files/etc/board.d/02_network
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt7621/base-files/etc/board.d/05_compat-version
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt7621/base-files/etc/rc.button/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt7621/base-files/etc/rc.button/lights_toggle
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt7621/config-6.6
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/mt7621/target.mk
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/ramips/Makefile
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/generic/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/generic/config-6.6
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/generic/config-filter
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/generic/PATCHES
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/generic/other-files/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/generic/other-files/init
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/generic/image/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/generic/image/initramfs-base-files.txt
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/generic/image/relocate/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/generic/image/relocate/head.S
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/generic/image/relocate/loader.lds
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/generic/image/relocate/cacheops.h
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/generic/image/relocate/cp0regdef.h
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/generic/image/relocate/Makefile
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/generic/image/lzma-loader/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/generic/image/lzma-loader/src/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/generic/image/lzma-loader/src/start.S
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/generic/image/lzma-loader/src/print.h
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/generic/image/lzma-loader/src/uart16550.c
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/generic/image/lzma-loader/src/printf.c
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/generic/image/lzma-loader/src/lzma-copy.lds.in
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/generic/image/lzma-loader/src/lzma.lds.in
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/generic/image/lzma-loader/src/decompress.c
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/generic/image/lzma-loader/src/uart16550.h
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/generic/image/lzma-loader/src/LzmaDecode.h
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/generic/image/lzma-loader/src/LzmaDecode.c
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/generic/image/lzma-loader/src/print.c
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/generic/image/lzma-loader/src/printf.h
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/generic/image/lzma-loader/src/Makefile
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/generic/image/lzma-loader/Makefile
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/generic/image/Makefile
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/target/linux/Makefile
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/repositories
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/rules.mk
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/metadata.pm
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/bundle-libraries.sh
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/clean-package.sh
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/kernel_bump.sh
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/cfe-wfi-tag.py
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/target-metadata.pl
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/spelling.txt
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/ext-tools.sh
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/json_add_image_info.py
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/kconfig.pl
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/symlink-tree.sh
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/om-fwupgradecfg-gen.sh
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/dl_github_archive.py
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/ipkg-make-index.sh
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/dl_cleanup.py
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/noop.sh
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/strip-kmod.sh
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/linksys-image.sh
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/sercomm-crypto.py
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/ipkg-build
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/patch-kernel.sh
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/mkits-qsdk-ipq-image.sh
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/deptest.sh
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/feeds
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/portable_date.sh
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/slugimage.pl
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/config.guess
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/md5sum
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/srecimage.pl
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/sign_images.sh
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/relink-lib.sh
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/belkin-header.py
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/brcmImage.pl
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/gen-dependencies.sh
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/remote-gdb
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/sercomm-kernel-header.py
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/get_source_date_epoch.sh
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/gen_image_generic.sh
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/pad_image
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/sercomm-payload.py
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/const_structs.checkpatch
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/config/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/config/images.c
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/config/qconf-cfg.sh
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/config/menu.o
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/config/nconf-cfg.sh
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/config/expr.h
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/config/parser.y
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/config/util.o
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/config/conf.o
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/config/confdata.c
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/config/qconf.h
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/config/lexer.lex.c
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/config/nconf.h
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/config/menu.c
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/config/preprocess.c
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/config/expr.o
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/config/preprocess.o
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/config/expr.c
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/config/images.h
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/config/symbol.o
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/config/confdata.o
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/config/conf
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/config/conf.c
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/config/README
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/config/nconf.gui.c
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/config/parser.tab.o
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/config/lkc.h
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/config/lxdialog/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/config/lxdialog/textbox.c
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/config/lxdialog/dialog.h
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/config/lxdialog/checklist.c
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/config/lxdialog/yesno.c
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/config/lxdialog/menubox.c
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/config/lxdialog/inputbox.c
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/config/lxdialog/util.c
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/config/parser.tab.c
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/config/lexer.lex.o
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/config/nconf.c
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/config/.gitignore
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/config/mconf-cfg.sh
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/config/symbol.c
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/config/lkc_proto.h
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/config/list.h
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/config/Kbuild.include
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/config/mconf.c
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/config/parser.tab.h
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/config/util.c
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/config/lexer.l
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/config/internal.h
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/config/qconf.cc
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/config/Makefile
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/cameo-tag.py
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/check-toolchain-clean.sh
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/kconfig-reorder.sh
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/cfe-partition-tag.py
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/ipkg-remove
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/redboot-script.pl
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/config.rpath
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/checkpatch.pl
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/fixup-makefile.pl
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/patch-specs.sh
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/timestamp.pl
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/getver.sh
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/combined-ext-image.sh
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/package-metadata.pl
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/sercomm-pid.py
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/qemustart
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/flashing/
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/flashing/flash.sh
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/flashing/adam2flash-fritzbox.pl
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/flashing/adam2flash-502T.pl
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/flashing/eva_ramboot.py
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/flashing/adsl2mue_flash.pl
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/flashing/adam2flash.pl
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/flashing/jungo-image.py
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/cleanfile
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/mkhash.c
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/sercomm-partition-tag.py
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/rstrip.sh
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/sysupgrade-tar.sh
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/projectsmirrors.json
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/dump-target-info.pl
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/download.pl
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/moxa-encode-fw.py
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/cameo-imghdr.py
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/command_all.sh
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/mkits-zyxel-fit-filogic.sh
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/download-check-artifact.sh
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/tplink-mkimage-2022.py
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/ext-toolchain.sh
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/mkits.sh
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/xxdi.pl
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/combined-image.sh
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/cfe-bin-header.py
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/diffconfig.sh
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/cleanpatch
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/netgear-encrypted-factory.py
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/time.pl
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/mkits-zyxel-fit.sh
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/config.sub
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/env
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/size_compare.sh
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/make-ipkg-dir.sh
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/functions.sh
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/json_overview_image_info.py
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/scripts/ubinize-image.sh
openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/Makefile
root@26ba92ad893d:/# cd openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64
root@26ba92ad893d:/openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64# make image PROFILE="tplink_tl-mr6400-v4" PACKAGES="modemmanager"
Generate local signing keys...
WARNING: can't open config file: /builder/shared-workdir/build/staging_dir/host/etc/ssl/openssl.cnf
WARNING: can't open config file: /builder/shared-workdir/build/staging_dir/host/etc/ssl/openssl.cnf
read EC key
writing EC key
Checking 'true'... ok.
Checking 'false'... ok.
Checking 'working-make'... ok.
Checking 'case-sensitive-fs'... ok.
Checking 'proper-umask'... ok.
Checking 'perl-data-dumper'... ok.
Checking 'perl-findbin'... ok.
Checking 'perl-file-copy'... ok.
Checking 'perl-file-compare'... ok.
Checking 'perl-thread-queue'... ok.
Checking 'perl-ipc-cmd'... ok.
Checking 'tar'... ok.
Checking 'find'... ok.
Checking 'bash'... updated.
Checking 'xargs'... ok.
Checking 'patch'... ok.
Checking 'diff'... ok.
Checking 'cp'... updated.
Checking 'seq'... ok.
Checking 'awk'... ok.
Checking 'grep'... updated.
Checking 'egrep'... updated.
Checking 'getopt'... ok.
Checking 'realpath'... ok.
Checking 'stat'... ok.
Checking 'gzip'... updated.
Checking 'unzip'... ok.
Checking 'bzip2'... ok.
Checking 'wget'... ok.
Checking 'install'... ok.
Checking 'perl'... ok.
Checking 'python'... updated.
Checking 'python3'... updated.
Checking 'python3-distutils'... ok.
Checking 'python3-stdlib'... ok.
Checking 'file'... ok.
Checking 'which'... ok.
Checking 'ldconfig-stub'... ok.
Building images for ramips - TP-Link TL-MR6400 v4
Packages: modemmanager apk-mbedtls base-files ca-bundle dnsmasq dropbear firewall4 fstools kernel kmod-gpio-button-hotplug kmod-leds-gpio kmod-mt7603 kmod-nft-offload kmod-usb-ledtrig-usbport kmod-usb-net-qmi-wwan kmod-usb-ohci kmod-usb-serial-option kmod-usb2 libc libgcc libustream-mbedtls logd mtd netifd nftables odhcp6c odhcpd-ipv6only ppp ppp-mod-pppoe swconfig uci uclient-fetch uqmi urandom-seed urngd wpad-basic-mbedtls

fetch https://downloads.openwrt.org/snapshots/targets/ramips/mt76x8/packages/packages.adb
fetch https://downloads.openwrt.org/snapshots/packages/mipsel_24kc/base/packages.adb
fetch https://downloads.openwrt.org/snapshots/targets/ramips/mt76x8/kmods/6.6.73-1-7303fbb67480e28a41c6ca21c6a94ce8/packages.adb
fetch https://downloads.openwrt.org/snapshots/packages/mipsel_24kc/luci/packages.adb
fetch https://downloads.openwrt.org/snapshots/packages/mipsel_24kc/packages/packages.adb
fetch https://downloads.openwrt.org/snapshots/packages/mipsel_24kc/routing/packages.adb
fetch https://downloads.openwrt.org/snapshots/packages/mipsel_24kc/telephony/packages.adb
fetch https://downloads.openwrt.org/snapshots/packages/mipsel_24kc/video/packages.adb
WARNING: opening packages/packages.adb: No such file or directory
OK: 0 MiB in 0 packages
Package list missing or not up-to-date, generating it.

Building package index...

Installing packages...
(  1/136) Installing libgcc1 (13.3.0-r4)
(  2/136) Installing libc (1.2.5-r4)
(  3/136) Installing libmbedtls21 (3.6.2-r1)
(  4/136) Installing libubox20240329 (2024.03.29~eb9bcb64-r1)
(  5/136) Installing libuclient20201210 (2024.10.22~88ae8f20-r1)
(  6/136) Installing uclient-fetch (2024.10.22~88ae8f20-r1)
(  7/136) Installing zlib (1.3.1-r1)
(  8/136) Installing apk-mbedtls (3.0.0_pre20241130-r2)
(  9/136) Installing busybox (1.37.0-r4)
( 10/136) Installing libubus20250102 (2025.01.02~afa57cce-r1)
( 11/136) Installing libuci20250120 (2025.01.20~16ff0bad-r1)
( 12/136) Installing libjson-c5 (0.18-r1)
( 13/136) Installing libblobmsg-json20240329 (2024.03.29~eb9bcb64-r1)
( 14/136) Installing ubusd (2025.01.02~afa57cce-r1)
( 15/136) Installing ubus (2025.01.02~afa57cce-r1)
( 16/136) Installing ubox (2024.04.26~85f10530-r1)
( 17/136) Installing fstools (2024.12.02~49d36ba2-r1)
( 18/136) Installing fwtool (2019.11.12~8f7fe925-r1)
( 19/136) Installing jsonfilter (2024.01.23~594cfa86-r1)
( 20/136) Installing jshn (2024.03.29~eb9bcb64-r1)
( 21/136) Installing libnl-tiny1 (2023.12.05~965c4bf4-r1)
( 22/136) Installing libudebug (2023.12.06~6d3f51f9)
( 23/136) Installing libucode20230711 (2024.12.06~209f041f-r1)
( 24/136) Installing ucode (2024.12.06~209f041f-r1)
( 25/136) Installing ucode-mod-fs (2024.12.06~209f041f-r1)
( 26/136) Installing netifd (2024.12.17~ea01ed41-r1)
( 27/136) Installing openwrt-keyring (2024.11.01~fbae29d7-r1)
( 28/136) Installing libjson-script20240329 (2024.03.29~eb9bcb64-r1)
( 29/136) Installing procd (2024.12.22~42d39376-r1)
( 30/136) Installing procd-seccomp (2024.12.22~42d39376-r1)
( 31/136) Installing usign (2020.05.23~f1f65026-r1)
( 32/136) Installing base-files (1651~6a7fa68569)
( 33/136) Installing ca-bundle (20240203-r1)
( 34/136) Installing dnsmasq (2.90-r3)
( 35/136) Installing dropbear (2024.86-r1)
( 36/136) Installing kernel (6.6.73~7303fbb67480e28a41c6ca21c6a94ce8-r1)
( 37/136) Installing kmod-crypto-hash (6.6.73-r1)
( 38/136) Installing kmod-crypto-crc32c (6.6.73-r1)
( 39/136) Installing kmod-lib-crc32c (6.6.73-r1)
( 40/136) Installing kmod-nf-conntrack (6.6.73-r1)
( 41/136) Installing kmod-nf-conntrack6 (6.6.73-r1)
( 42/136) Installing kmod-nf-log (6.6.73-r1)
( 43/136) Installing kmod-nf-log6 (6.6.73-r1)
( 44/136) Installing kmod-nf-nat (6.6.73-r1)
( 45/136) Installing kmod-nf-reject (6.6.73-r1)
( 46/136) Installing kmod-nf-reject6 (6.6.73-r1)
( 47/136) Installing kmod-nfnetlink (6.6.73-r1)
( 48/136) Installing kmod-nft-core (6.6.73-r1)
( 49/136) Installing kmod-nft-fib (6.6.73-r1)
( 50/136) Installing kmod-nft-nat (6.6.73-r1)
( 51/136) Installing kmod-nf-flow (6.6.73-r1)
( 52/136) Installing kmod-nft-offload (6.6.73-r1)
( 53/136) Installing jansson4 (2.14-r3)
( 54/136) Installing libmnl0 (1.0.5-r1)
( 55/136) Installing libnftnl11 (1.2.8-r1)
( 56/136) Installing nftables-json (1.1.1-r1)
( 57/136) Installing ucode-mod-ubus (2024.12.06~209f041f-r1)
( 58/136) Installing ucode-mod-uci (2024.12.06~209f041f-r1)
( 59/136) Installing firewall4 (2024.12.18~18fc0ead-r1)
( 60/136) Installing kmod-gpio-button-hotplug (6.6.73-r5)
( 61/136) Installing kmod-leds-gpio (6.6.73-r1)
( 62/136) Installing hostapd-common (2024.09.15~5ace39b0-r3)
( 63/136) Installing iw (6.9-r1)
( 64/136) Installing libiwinfo-data (2024.10.20~b94f066e-r1)
( 65/136) Installing libiwinfo20230701 (2024.10.20~b94f066e-r1)
( 66/136) Installing iwinfo (2024.10.20~b94f066e-r1)
( 67/136) Installing ucode-mod-digest (2024.12.06~209f041f-r1)
( 68/136) Installing ucode-mod-nl80211 (2024.12.06~209f041f-r1)
( 69/136) Installing ucode-mod-rtnl (2024.12.06~209f041f-r1)
( 70/136) Installing wifi-scripts (1.0-r1)
( 71/136) Installing wireless-regdb (2024.10.07-r1)
( 72/136) Installing kmod-cfg80211 (6.6.73.6.12.6-r1)
( 73/136) Installing kmod-crypto-null (6.6.73-r1)
( 74/136) Installing kmod-crypto-aead (6.6.73-r1)
( 75/136) Installing kmod-crypto-manager (6.6.73-r1)
( 76/136) Installing kmod-crypto-hmac (6.6.73-r1)
( 77/136) Installing kmod-crypto-sha3 (6.6.73-r1)
( 78/136) Installing kmod-crypto-sha512 (6.6.73-r1)
( 79/136) Installing kmod-crypto-rng (6.6.73-r1)
( 80/136) Installing kmod-crypto-geniv (6.6.73-r1)
( 81/136) Installing kmod-crypto-seqiv (6.6.73-r1)
( 82/136) Installing kmod-crypto-ctr (6.6.73-r1)
( 83/136) Installing kmod-crypto-ccm (6.6.73-r1)
( 84/136) Installing kmod-crypto-cmac (6.6.73-r1)
( 85/136) Installing kmod-crypto-gf128 (6.6.73-r1)
( 86/136) Installing kmod-crypto-ghash (6.6.73-r1)
( 87/136) Installing kmod-crypto-gcm (6.6.73-r1)
( 88/136) Installing kmod-mac80211 (6.6.73.6.12.6-r1)
( 89/136) Installing kmod-mt76-core (6.6.73.2025.01.22~a22d59e4-r1)
( 90/136) Installing kmod-mt7603 (6.6.73.2025.01.22~a22d59e4-r1)
( 91/136) Installing kmod-nls-base (6.6.73-r1)
( 92/136) Installing kmod-usb-core (6.6.73-r1)
( 93/136) Installing kmod-usb-ledtrig-usbport (6.6.73-r1)
( 94/136) Installing kmod-mii (6.6.73-r1)
( 95/136) Installing kmod-usb-net (6.6.73-r1)
( 96/136) Installing kmod-usb-wdm (6.6.73-r1)
( 97/136) Installing kmod-usb-net-qmi-wwan (6.6.73-r1)
( 98/136) Installing kmod-usb-ohci (6.6.73-r1)
( 99/136) Installing kmod-usb-serial (6.6.73-r1)
(100/136) Installing kmod-usb-serial-wwan (6.6.73-r1)
(101/136) Installing kmod-usb-serial-option (6.6.73-r1)
(102/136) Installing kmod-usb-ehci (6.6.73-r1)
(103/136) Installing kmod-usb2 (6.6.73-r1)
(104/136) Installing libustream-mbedtls20201210 (2024.07.28~99bd3d2b-r1)
(105/136) Installing logd (2024.04.26~85f10530-r1)
(106/136) Installing libpthread (1.2.5-r4)
(107/136) Installing libdbus (1.14.10-r1)
(108/136) Installing libexpat (2.6.3-r1)
(109/136) Installing dbus (1.14.10-r1)
(110/136) Installing libattr (2.5.2-r3)
(111/136) Installing libffi (3.4.6-r1)
(112/136) Installing libpcre2 (10.42-r1)
(113/136) Installing glib2 (2.82.0-r1)
(114/136) Installing libmbim (1.30.0-r2)
(115/136) Installing libqrtr-glib (1.2.2-r3)
(116/136) Installing libqmi (1.34.0-r2)
(117/136) Installing kmod-lib-crc-ccitt (6.6.73-r1)
(118/136) Installing kmod-slhc (6.6.73-r1)
(119/136) Installing kmod-ppp (6.6.73-r1)
(120/136) Installing ppp (2.5.2-r1)
(121/136) Installing modemmanager (1.22.0-r20)
(122/136) Installing mtd (26)
(123/136) Installing odhcp6c (2024.09.25~b6ae9ffa-r1)
(124/136) Installing odhcpd-ipv6only (2024.05.08~a2988231-r1)
(125/136) Installing kmod-pppox (6.6.73-r1)
(126/136) Installing kmod-pppoe (6.6.73-r1)
(127/136) Installing ppp-mod-pppoe (2.5.2-r1)
(128/136) Installing swconfig (12)
(129/136) Installing uci (2025.01.20~16ff0bad-r1)
(130/136) Installing wwan (2019.04.29-r6)
(131/136) Installing uqmi (2024.08.25~28b48a10-r2)
(132/136) Installing getrandom (2024.04.26~85f10530-r1)
(133/136) Installing urandom-seed (3)
(134/136) Installing urngd (2023.11.01~44365eb1-r1)
(135/136) Installing ucode-mod-uloop (2024.12.06~209f041f-r1)
(136/136) Installing wpad-basic-mbedtls (2024.09.15~5ace39b0-r3)
OK: 26 MiB in 136 packages

Finalizing root filesystem...
add alternative: /usr/bin/ssh-keygen -> /usr/sbin/dropbear
add alternative: /usr/bin/scp -> /usr/sbin/dropbear
add alternative: /usr/bin/ssh -> /usr/sbin/dropbear
add alternative: /sbin/logread -> /usr/libexec/logread-ubox
add alternative: /sbin/rmmod -> /sbin/kmodloader
add alternative: /sbin/insmod -> /sbin/kmodloader
add alternative: /sbin/lsmod -> /sbin/kmodloader
add alternative: /sbin/modinfo -> /sbin/kmodloader
add alternative: /sbin/modprobe -> /sbin/kmodloader
add alternative: /usr/bin/wget -> /bin/uclient-fetch
Enabling boot
Enabling bootcount
Enabling cron
Enabling dbus
Enabling dnsmasq
Enabling done
Enabling dropbear
Enabling firewall
Enabling gpio_switch
Enabling led
Enabling log
Enabling modemmanager
Enabling network
Enabling odhcpd
Enabling packet_steering
Enabling sysctl
Enabling sysfixtime
Enabling sysntpd
Enabling system
Enabling umount
Enabling urandom_seed
Enabling urngd
Enabling wpad

Building images...
Parallel mksquashfs: Using 10 processors
Creating 4.0 filesystem on /openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/root.squashfs, block size 1048576.
Pseudo file "dev" exists in source filesystem "/openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/root-ramips/dev".
Ignoring, exclude it (-e/-ef) to override.
[=========================================================================================================================================================================================================|] 1015/1015 100%

Exportable Squashfs 4.0 filesystem, xz compressed, data block size 1048576
	compressed data, compressed metadata, compressed fragments,
	no xattrs, compressed ids
	duplicates are removed
Filesystem size 5675.42 Kbytes (5.54 Mbytes)
	23.66% of uncompressed filesystem size (23984.46 Kbytes)
Inode table size 8592 bytes (8.39 Kbytes)
	19.24% of uncompressed inode table size (44655 bytes)
Directory table size 12096 bytes (11.81 Kbytes)
	45.42% of uncompressed directory table size (26633 bytes)
Number of duplicate files found 75
Number of inodes 1345
Number of files 1012
Number of fragments 19
Number of symbolic links 229
Number of device nodes 1
Number of fifo nodes 0
Number of socket nodes 0
Number of directories 103
Number of hard-links 0
Number of ids (unique uids + gids) 1
Number of uids 1
	unknown (0)
Number of gids 1
	unknown (0)
[mktplinkfw2] rootfs offset aligned to 0x2246284
[mktplinkfw2] *** error: images are too big by 62518 bytes
stat: cannot statx '/openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/tmp/openwrt-ramips-mt76x8-tplink_tl-mr6400-v4-squashfs-sysupgrade.bin': No such file or directory
bash: line 1: [: 7995392: unary operator expected
    WARNING: Image file /openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/tmp/openwrt-ramips-mt76x8-tplink_tl-mr6400-v4-squashfs-sysupgrade.bin is too big:  > 7995392
Failed to open firmware file
sha256sum: /openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/tmp/openwrt-ramips-mt76x8-tplink_tl-mr6400-v4-squashfs-sysupgrade.bin: No such file or directory
cp: cannot stat '/openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/tmp/openwrt-ramips-mt76x8-tplink_tl-mr6400-v4-squashfs-sysupgrade.bin': No such file or directory
Skip JSON creation for non existing file /openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/tmp/openwrt-ramips-mt76x8-tplink_tl-mr6400-v4-squashfs-sysupgrade.bin
1+0 records in
1+0 records out
131072 bytes (131 kB, 128 KiB) copied, 0.00128733 s, 102 MB/s
[mktplinkfw2] rootfs offset aligned to 0x2246284
[mktplinkfw2] *** error: images are too big by 62518 bytes
cp: cannot stat '/openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/tmp/openwrt-ramips-mt76x8-tplink_tl-mr6400-v4-squashfs-tftp-recovery.bin': No such file or directory
Skip JSON creation for non existing file /openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64/build_dir/target-mipsel_24kc_musl/linux-ramips_mt76x8/tmp/openwrt-ramips-mt76x8-tplink_tl-mr6400-v4-squashfs-tftp-recovery.bin
JSON info file script could not find any JSON files for target

Calculating checksums...
root@26ba92ad893d:/openwrt-imagebuilder-ramips-mt76x8.Linux-x86_64#
```


