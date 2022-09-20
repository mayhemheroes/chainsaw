# Build Stage
FROM ghcr.io/evanrichter/cargo-fuzz:latest as builder

## evtx samples
RUN wget https://github.com/omerbenamram/evtx/archive/refs/heads/master.zip && \
    unzip master.zip && \
    mkdir /testsuite && \
    cp evtx-master/samples/* /testsuite/

## Add source code to the build stage.
ADD . /src
WORKDIR /src

RUN env RUSTFLAGS="-g -Cdebug-assertions=on" cargo +nightly -Z sparse-registry build --release && \
    mv target/release/chainsaw /chainsaw

# Package Stage
FROM rustlang/rust:nightly

COPY --from=builder /chainsaw /
RUN mkdir /testsuite /rules
COPY --from=builder /testsuite/ /testsuite/
COPY --from=builder /src/rules/ /rules/
