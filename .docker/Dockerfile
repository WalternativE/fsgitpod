FROM gitpod/workspace-full:latest

USER root

ENV MONO_THREADS_PER_CPU 50
RUN MONO_VERSION=6.4.0.198 && \
    FSHARP_VERSION=10.2.3 && \
    FSHARP_BASENAME=fsharp-$FSHARP_VERSION && \
    FSHARP_ARCHIVE=$FSHARP_VERSION.tar.gz && \
    FSHARP_ARCHIVE_URL=https://github.com/fsharp/fsharp/archive/$FSHARP_VERSION.tar.gz && \
    export GNUPGHOME="$(mktemp -d)" && \
    apt-get update && apt-get --no-install-recommends install -y gnupg dirmngr ca-certificates apt-transport-https && \
    apt-key adv --batch --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 3FA7E0328081BFF6A14DA29AA6A19B38D3D831EF && \
    echo "deb https://download.mono-project.com/repo/ubuntu stable-bionic/snapshots/$MONO_VERSION main" | tee /etc/apt/sources.list.d/mono-official-stable.list && \
    apt-get update -y && \
    apt-get --no-install-recommends install -y pkg-config make nuget mono-devel msbuild ca-certificates-mono locales && \
    rm -rf /var/lib/apt/lists/* && \
    echo 'en_US.UTF-8 UTF-8' > /etc/locale.gen && /usr/sbin/locale-gen && \
    mkdir -p /tmp/src && \
    cd /tmp/src && \
    printf "namespace a { class b { public static void Main(string[] args) { new System.Net.WebClient().DownloadFile(\"%s\", \"%s\");}}}" $FSHARP_ARCHIVE_URL $FSHARP_ARCHIVE > download-fsharp.cs && \
    mcs download-fsharp.cs && mono download-fsharp.exe && rm download-fsharp.exe download-fsharp.cs && \
    tar xf $FSHARP_ARCHIVE && \
    cd $FSHARP_BASENAME && \
    make && \
    make install && \
    cd ~ && \
    rm -rf /tmp/src /tmp/NuGetScratch ~/.nuget ~/.config ~/.local "$GNUPGHOME" && \
    apt-get purge -y make gnupg dirmngr && \
    apt-get clean

ENV FrameworkPathOverride /usr/lib/mono/4.7.2-api/
ENV NUGET_XMLDOC_MODE skip
RUN apt-get update && \
    apt-get --no-install-recommends install -y \
    curl \
    libunwind8 \
    gettext \
    apt-transport-https \
    libc6 \
    libcurl4 \
    libgcc1 \
    libgssapi-krb5-2 \
    libicu63 \
    liblttng-ust0 \
    libssl1.1 \
    libstdc++6 \
    libunwind8 \
    libuuid1 \
    zlib1g && \
    rm -rf /var/lib/apt/lists/*
RUN DOTNET_SDK_VERSION=3.0.100 && \
    DOTNET_SDK_DOWNLOAD_URL=https://dotnetcli.blob.core.windows.net/dotnet/Sdk/$DOTNET_SDK_VERSION/dotnet-sdk-$DOTNET_SDK_VERSION-linux-x64.tar.gz && \
    DOTNET_SDK_DOWNLOAD_SHA=766da31f9a0bcfbf0f12c91ea68354eb509ac2111879d55b656f19299c6ea1c005d31460dac7c2a4ef82b3edfea30232c82ba301fb52c0ff268d3e3a1b73d8f7 && \
    curl -SL $DOTNET_SDK_DOWNLOAD_URL --output dotnet.tar.gz && \
    echo "$DOTNET_SDK_DOWNLOAD_SHA dotnet.tar.gz" | sha512sum -c - && \
    mkdir -p /usr/share/dotnet && \
    tar -zxf dotnet.tar.gz -C /usr/share/dotnet && \
    rm dotnet.tar.gz && \
    ln -s /usr/share/dotnet/dotnet /usr/bin/dotnet
ENV DOTNET_CLI_TELEMETRY_OPTOUT 1
ENV DOTNET_RUNNING_IN_CONTAINER=true
ENV DOTNET_USE_POLLING_FILE_WATCHER=true
ENV NUGET_XMLDOC_MODE=skip
RUN mkdir warmup && \
    cd warmup && \
    dotnet new && \
    cd - && \
    rm -rf warmup /tmp/NuGetScratch

# set env and warmup for user gitpod
USER gitpod

ENV MONO_THREADS_PER_CPU 50
ENV FrameworkPathOverride /usr/lib/mono/4.7.2-api/
ENV DOTNET_CLI_TELEMETRY_OPTOUT 1
ENV DOTNET_RUNNING_IN_CONTAINER=true
ENV DOTNET_USE_POLLING_FILE_WATCHER=true
ENV NUGET_XMLDOC_MODE=skip

RUN mkdir warmup && \
    cd warmup && \
    dotnet new && \
    cd - && \
    rm -rf warmup /tmp/NuGetScratch

USER root