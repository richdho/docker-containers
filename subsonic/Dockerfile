FROM opensuse:tumbleweed
MAINTAINER Carlos Hernandez <carlos@techbyte.ca>
ENV LANG="en_US.UTF-8" s6_overlay_version="1.17.1.2" APP_NAME="subsonic" IMG_NAME="subsonic" TAG_NAME="latest" S6_LOGGING="0"

RUN zypper --non-interactive ar -f -n packman http://ftp.gwdg.de/pub/linux/misc/packman/suse/openSUSE_Tumbleweed/ packman \
 && zypper --gpg-auto-import-keys ref \
 && zypper --non-interactive up \
 && zypper --non-interactive in --no-recommends \
	curl \
	ffmpeg-head \
	glibc-locale \
	java-1_9_0-openjdk-headless \
	tar \
	timezone \
  && curl -skL https://github.com/just-containers/s6-overlay/releases/download/v${s6_overlay_version}/s6-overlay-amd64.tar.gz -o /tmp/s6-overlay-amd64.tar.gz \
  && tar xf /tmp/s6-overlay-amd64.tar.gz -C / \
  && echo $(curl -skL https://sourceforge.net/projects/subsonic/rss |grep link|grep download|grep tar.gz|sed -n s%"\s\+<link>\([^<]*\).*"%"\1"%p|head -1|awk -F/ '{print $8}') > /subsonic.version \
  &&  curl -skL $(curl -skL https://sourceforge.net/projects/subsonic/rss |grep link|grep download|grep tar.gz|sed -n s%"\s\+<link>\([^<]*\).*"%"\1"%p|head -1|sed s%/download$%%) -o /tmp/subsonic.tar.gz \
  && mkdir -p /opt/subsonic \
  && tar xf /tmp/subsonic.tar.gz -C /opt/subsonic \
  && cd /usr/lib/locale \
  && ls | grep -ve "en_CA*\|en_US*" | xargs rm -rf \
  && cd /usr/share/locale \
  &&  find -name '*.mo' | grep -ve "en_US*" | xargs rm \
  && rpm -e --nodeps --allmatches --noscripts \
	tar \
	curl \
	`rpm -qa | grep cpio` \
	`rpm -qa | grep cryptsetup` \
	`rpm -qa | grep dbus-1` \
	`rpm -qa | grep kbd` \
	`rpm -qa | grep kmod` \
	`rpm -qa | grep mapper` \
	`rpm -qa | grep openSUSE-release` \
	`rpm -qa | grep perl` \
	`rpm -qa | grep pigz` \
	`rpm -qa | grep pinentry` \
	`rpm -qa | grep qrencode` \
  && zypper cc --all \
  && rm -rf /usr/lib64/gconv/* \
  && rm -rf /usr/share/{man,doc,info,gnome/help} \
  && rm -rf /var/cache/zypp* \
  && rm -rf /tmp/* \
  && rm -rf /var/log/*

COPY root /
ENTRYPOINT ["/init"]