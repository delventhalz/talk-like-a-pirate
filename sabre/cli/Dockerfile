# Copyright 2018 Cargill Incorporated
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

FROM rust:1.26

# Need sawtooth-sdk to create keys
RUN echo "deb http://repo.sawtooth.me/ubuntu/1.0/stable xenial universe" >> /etc/apt/sources.list \
 && apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 8AA7AF1F1091A5FD \
 && apt-get update \
 && apt-get install -y -q \
      python3-sawtooth-sdk \
      python3-sawtooth-cli \
      unzip \
 && apt-get clean \
 && rm -rf /var/lib/apt/lists/*

RUN apt-get update && apt-get install -y unzip libzmq3-dev

# For Building Protobufs
RUN curl -OLsS https://github.com/google/protobuf/releases/download/v3.5.1/protoc-3.5.1-linux-x86_64.zip \
 && unzip protoc-3.5.1-linux-x86_64.zip -d protoc3 \
 && rm protoc-3.5.1-linux-x86_64.zip

RUN apt-get update && apt-get install -y protobuf-compiler

WORKDIR /pirate-talk/sabre
RUN USER=root cargo new --bin cli

WORKDIR /pirate-talk/sabre/cli
COPY cli/Cargo.toml cli/Cargo.lock ./
RUN cargo build

WORKDIR /pirate-talk/sabre
COPY . .

WORKDIR /pirate-talk/sabre/cli
RUN cargo build

ENV PATH=$PATH:/pirate-talk/sabre/cli/target/debug/
