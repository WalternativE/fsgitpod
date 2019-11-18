FROM gitpod/workspace-full:latest

USER root

RUN wget -q https://packages.microsoft.com/config/ubuntu/18.04/packages-microsoft-prod.deb && \
    dpkg -i packages-microsoft-prod.deb && rm -rf packages-microsoft-prod.deb && \
    add-apt-repository universe && \
    apt-get update && apt-get -y -o APT::Install-Suggests="true" install dotnet-sdk-3.0 && \
    apt -y clean;

USER gitpod

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