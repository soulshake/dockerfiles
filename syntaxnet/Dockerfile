FROM java:8
MAINTAINER AJ Bowen <aj@soulshake.net>

ENV SYNTAXNETDIR=/opt/tensorflow PATH=$PATH:/root/bin

RUN mkdir -p $SYNTAXNETDIR \
    && cd $SYNTAXNETDIR \
    && apt-get update \
    && apt-get install git zlib1g-dev file swig python2.7 python-dev python-pip -y \
    && pip install --upgrade pip \
    && pip install -U protobuf==3.0.0b2 \
    && pip install asciitree \
    && pip install numpy \
    && wget https://github.com/bazelbuild/bazel/releases/download/0.2.2b/bazel-0.2.2b-installer-linux-x86_64.sh \
    && chmod +x bazel-0.2.2b-installer-linux-x86_64.sh \
    && ./bazel-0.2.2b-installer-linux-x86_64.sh --user \
    && git clone --recursive https://github.com/tensorflow/models.git \
    && cd $SYNTAXNETDIR/models/syntaxnet/tensorflow \
    && echo "\n\n\n" | ./configure

RUN cd $SYNTAXNETDIR/models/syntaxnet \
    && bazel test --genrule_strategy=standalone syntaxnet/... util/utf8/... \
    && apt-get autoremove -y \
    && apt-get clean

WORKDIR $SYNTAXNETDIR/models/syntaxnet

COPY ./entrypoint.sh /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]

# Run command
# ===============================
# docker run -ti soulshake/syntaxnet "i went to the store"

