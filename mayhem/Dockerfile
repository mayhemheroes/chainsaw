FROM rustlang/rust:nightly as builder

RUN apt-get update && apt-get install -y wget unzip

RUN wget https://github.com/omerbenamram/evtx/archive/refs/heads/master.zip && \
    unzip master.zip && \
    mkdir /testsuite && \
    (cp -r evtx-master/samples/. /testsuite/ 2>/dev/null || echo "No samples to copy")

ADD . /src
WORKDIR /src

RUN env RUSTFLAGS="-g -Cdebug-assertions=on" cargo +nightly build --release && \
    mv target/release/chainsaw /chainsaw

FROM rustlang/rust:nightly

COPY --from=builder /chainsaw /
RUN mkdir /testsuite /rules
COPY --from=builder /testsuite/ /testsuite/
COPY --from=builder /src/rules/ /rules/
