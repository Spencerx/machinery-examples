FROM opensuse
MAINTAINER Cornelius Schumacher <cschum@suse.de>

RUN zypper --non-interactive install ruby

COPY create_files /
RUN /create_files
