FROM debian:jessie

RUN yum install -y initscripts curl tar gcc libc6-dev git

#RUN	sudo apt-get update 
WORKDIR	~


# install go
ENV     GOPATH /gopath
RUN	curl -O https://storage.googleapis.com/golang/go1.7.linux-amd64.tar.gz && \
	tar -xvf go1.7.linux-amd64.tar.gz -C /usr/local && \
	mkdir -p $GOPATH
ENV	PATH /usr/local/go/bin:$GOPATH/bin:$PATH
ENV	GOARM 6

# install godep
RUN	go get github.com/tools/godep


# install phantomjs 1.9.8
#RUN     cd /usr/local/bin \
#        && wget https://github.com/piksel/phantomjs-raspberrypi/raw/master/bin/phantomjs \
#        && chmod a+x phantomjs

# install phantomjs 2.1.1
#RUN    wget https://github.com/timstanley1985/phantomjs-linux-armv6l/raw/master/phantomjs-2.1.1-linux-armv6l.tar.gz && \
#       tar -xf phantomjs-2.1.1-linux-armv6l.tar.gz && \
 #      cp phantomjs-2.1.1-linux-armv6l/bin/phantomjs /usr/local/bin/phantomjs && \
  #     chmod a+x /usr/local/bin/phantomjs && \
   #    rm phantomjs-2.1.1-linux-armv6l.tar.gz && \
    #   rm -r phantomjs-2.1.1-linux-armv6l/

#RUN curl -O http://archive.ubuntu.com/ubuntu/pool/main/a/apt/apt_0.8.16~exp5ubuntu13_amd64.deb && \
 #   tar -xvf apt_0.8.16~exp5ubuntu13_amd64.deb -C /usr/local
    #dpkg -i apt_*.deb

# install node 0.12.6
#RUN     curl -sLS https://apt.adafruit.com/add |bash \
#        && apt-get install -qy node

ENV    NODE_VER 4.4.1
#RUN   apt install npm
#RUN    wget https://nodejs.org/dist/v$NODE_VER/node-v$NODE_VER-linux-armv7l.tar.gz && \
 #      tar -xf node-v$NODE_VER-linux-armv7l.tar.gz && \
  #     cd node-v$NODE_VER-linux-armv7l && \
   #    cp -R * /usr/local/ && \
    #   cd .. && \
     #  rm -r node-v$NODE_VER-linux-armv7l && \
      # rm  node-v$NODE_VER-linux-armv7l.tar.gz



# install fpm
#RUN     gem install fpm



ARG     GRAFANA_VERSION
# invalidate docker cache
RUN	echo building $GRAFANA_VERSION

# FIXME: exit 0 hack for error "no buildable Go source files in /gopath1.5/src/github.com/grafana/grafana"
WORKDIR $GOPATH
RUN	go get -t -d github.com/yuvaraj951/grafana; exit 0

RUN cd $GOPATH/src/github.com && \
    mv yuvaraj951 grafana
# set working dir
WORKDIR $GOPATH/src/github.com/grafana/grafana

# if GRAFANA_VERSION not master, get specific version
#RUN	if [[ $GRAFANA_VERSION != master* ]]; then git checkout tags/v$GRAFANA_VERSION; fi
#RUN	git checkout tags/v$GRAFANA_VERSION

# go setup
RUN     go run build.go setup 
        

RUN apt install npm

RUN	npm install

RUN	npm install -g grunt-cli

RUN	go run build.go build

RUN     go run build.go package


VOLUME	/dist

CMD	if [ -d /dist ]; then cp $GOPATH/src/github.com/grafana/grafana/dist/* /dist; fi


