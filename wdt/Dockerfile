FROM ubuntu as builder

RUN apt-get update -y
RUN apt-get install -y libgoogle-glog-dev libboost-system-dev libboost-filesystem-dev libdouble-conversion-dev libjemalloc-dev libssl-dev libfmt-dev
RUN apt-get install -y cmake git make gcc g++ python3

WORKDIR /workspace

RUN git clone https://github.com/facebook/folly.git
RUN git clone https://github.com/facebook/wdt.git --depth=1

RUN cd folly && git checkout v2022.12.05.00
RUN mkdir build && cd build && cmake ../wdt --install-prefix=/workspace/install && make -j install
RUN ldd build/_bin/wdt/wdt | awk 'NF == 4 { system("cp " $3 " /workspace/install/lib") }'


FROM ubuntu

COPY --from=builder /workspace/install/bin /usr/local/bin
COPY --from=builder /workspace/install/lib /usr/lib/x86_64-linux-gnu

