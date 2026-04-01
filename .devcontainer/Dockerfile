FROM ubuntu:22.04

ENV DEBIAN_FRONTEND=noninteractive

# Install dependencies
RUN apt-get update && apt-get install -y \
    curl git unzip xz-utils zip libglu1-mesa openjdk-17-jdk \
    adb wget clang cmake ninja-build pkg-config libgtk-3-dev \
    && rm -rf /var/lib/apt/lists/*

# Install Flutter
WORKDIR /opt
RUN git clone https://github.com/flutter/flutter.git -b stable

ENV PATH="/opt/flutter/bin:/opt/flutter/bin/cache/dart-sdk/bin:${PATH}"

# Precache Flutter
RUN flutter doctor

# Install Android command-line tools
RUN mkdir -p /opt/android-sdk/cmdline-tools
WORKDIR /opt/android-sdk/cmdline-tools

RUN wget https://dl.google.com/android/repository/commandlinetools-linux-9477386_latest.zip \
    && unzip commandlinetools-linux-*.zip \
    && mv cmdline-tools latest

ENV ANDROID_HOME=/opt/android-sdk
ENV PATH=$ANDROID_HOME/cmdline-tools/latest/bin:$ANDROID_HOME/platform-tools:$PATH

# Accept licenses + install basic SDK tools
RUN yes | sdkmanager --licenses
RUN sdkmanager "platform-tools" "platforms;android-33" "build-tools;33.0.2"

WORKDIR /workspace
