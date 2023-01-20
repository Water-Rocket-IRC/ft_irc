FROM ubuntu:latest

RUN apt-get -y update \
	&& apt-get -y upgrade \
	&& apt-get install -y inspircd irssi tcpflow

COPY ./setup/irc_setup.sh tmp/

RUN chmod +x /tmp/irc_setup.sh

ENTRYPOINT [ "tmp/irc_setup.sh" ]
