FROM centos:6

ARG AWS_ACCESS_KEY_ID
ARG AWS_SECRET_ACCESS_KEY
ARG AWS_DEFAULT_REGION=ap-southeast-2

# https://docs.aws.amazon.com/cli/latest/userguide/installing.html
RUN yum -y install epel-release
RUN yum -y install python-pip
RUN pip install awscli --upgrade --user

ENV PATH="$PATH:/root/.local/bin:/root/bin"

RUN yum -y install xz wget perl gcc

# GHC 8.0.1
RUN wget https://downloads.haskell.org/~ghc/8.0.1/ghc-8.0.1-x86_64-centos67-linux.tar.xz
RUN tar xf ghc-8.0.1-x86_64-centos67-linux.tar.xz
RUN rm ghc-8.0.1-x86_64-centos67-linux.tar.xz
RUN cd ghc-8.0.1 && ./configure --prefix=$HOME/haskell/ghc-8.0.1 && make install
ENV PATH="/root/haskell/ghc-8.0.1/bin:$PATH"

# Cabal 1.24
RUN yum -y install gmp-devel zlib-devel
RUN wget "http://hackage.haskell.org/package/cabal-install-1.24.0.0/cabal-install-1.24.0.0.tar.gz"
RUN tar xf cabal-install-1.24.0.0.tar.gz
RUN rm cabal-install-1.24.0.0.tar.gz
RUN mkdir $HOME/bin
RUN cd cabal-install-1.24.0.0 && EXTRA_CONFIGURE_OPTS="" ./bootstrap.sh --sandbox --no-doc && cp .cabal-sandbox/bin/cabal $HOME/bin/cabal

RUN aws s3 cp s3://ambiata-dispensary-v2/dist/master/vee/linux/x86_64/1.0.1-20180130002609-02697b4/vee-1.0.1-20180130002609-02697b4 $HOME/bin/vee
RUN chmod +x $HOME/bin/vee

RUN aws s3 cp $(vee s3 latest) $HOME/bin/s3
RUN aws s3 cp $(vee mafia latest) $HOME/bin/mafia
RUN chmod +x $HOME/bin/*

RUN yum -y install git
