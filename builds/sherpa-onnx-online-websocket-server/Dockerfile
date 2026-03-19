#########################################################################################
## docker will invoke this file from ../.. dir in order to access code
#########################################################################################
FROM debian:bookworm-slim AS base
RUN apt-get update && apt-get install -y \
    ca-certificates
#########################################################################################
FROM base AS builder

RUN apt-get install -y \
    cmake ninja-build build-essential \
    pkg-config

WORKDIR /src

COPY . /src

RUN mkdir build && cd build && \
    cmake .. -DBUILD_SHARED_LIBS=ON -DCMAKE_BUILD_TYPE=Release \
    -DSHERPA_ONNX_ENABLE_PORTAUDIO=OFF \
    -DSHERPA_ONNX_ENABLE_TTS=OFF \
    -DSHERPA_ONNX_ENABLE_SPEAKER_DIARIZATION=OFF \
    -DSHERPA_ONNX_BUILD_C_API_EXAMPLES=OFF \
    -DSHERPA_ONNX_USE_PRE_INSTALLED_ONNXRUNTIME_IF_AVAILABLE=OFF \
    -DSHERPA_ONNX_ENABLE_WEBSOCKET=ON && \
    cmake --build . --target sherpa-onnx-online-websocket-server -j
#####################################################################################
FROM base AS runner

RUN rm -rf /var/lib/apt/lists/*

WORKDIR /app

COPY --from=builder /src/build/_deps/onnxruntime-src/lib/libonnxruntime.so /app/
COPY --from=builder /src/build/bin/sherpa-onnx-online-websocket-server /app/

EXPOSE 6006

ENTRYPOINT ["./sherpa-onnx-online-websocket-server"]

