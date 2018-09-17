#Depending on the operating system of the host machines(s) that will build or run the containers, the image specified in the FROM statement may need to be changed.
#For more information, please see https://aka.ms/containercompat

FROM microsoft/dotnet:2.1-aspnetcore-runtime-nanoserver-sac2016 AS base
WORKDIR /app
EXPOSE 3909
EXPOSE 44338

FROM microsoft/dotnet:2.1-sdk-nanoserver-sac2016 AS build
WORKDIR /src
COPY ["DockerTestApp/DockerTestApp.csproj", "DockerTestApp/"]
RUN dotnet restore "DockerTestApp/DockerTestApp.csproj"
COPY . .
WORKDIR "/src/DockerTestApp"
RUN dotnet build "DockerTestApp.csproj" -c Release -o /app

FROM build AS publish
RUN dotnet publish "DockerTestApp.csproj" -c Release -o /app

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "DockerTestApp.dll"]