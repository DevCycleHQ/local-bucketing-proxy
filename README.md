# DevCycle Local Bucketing Proxy

This is an implementation that uses our Go Server SDK to initialize and start multiple servers that emulate the response
format of the
bucketing-api server. This allows SDK's where implementing the WebAssembly bucketing library as a core isn't possible to
benefit from the Local Bucketing benefits of the DevCycle platform.

## Usage

The application is delivered in multiple formats - a Docker image, a deb, and RPM package, and in a raw application
format for local building and implementation.

The proxy handles two modes of operation - you can expose the HTTP server over a TCP port, or over Unix domain sockets.
The latter is recommended for servers that will deploy this in a fashion where the proxy is running on the same machine
as the SDK,
preventing the need for network calls.

The HTTP server mode is a 1:1 replacement for the bucketing-api - and can be used in place where there is no SDK in use
as well.

### Docker

Please note that the docker container image in the repo is not the official one, and is only used for testing purposes.

## Options

Either a path to a config file which allows specifying multiple instances of a proxy, or environment variables can be
used to configure the proxy.

### Command Line Arguments

| ARGUMENT | TYPE   | DEFAULT | REQUIRED | DESCRIPTION                                |
|----------|--------|---------|----------|--------------------------------------------|
| -h       |        |         |          | Prints help information, and version info. |
| -c       | String |         |          | The path to the config file.               |

### Environment Variables

| KEY                                                    | TYPE          | DEFAULT | REQUIRED | DESCRIPTION                                                                     |
|--------------------------------------------------------|---------------|---------|----------|---------------------------------------------------------------------------------|
| DVC_LB_PROXY_UNIX_SOCKET_PATH                          | String        |         |          | The path to the Unix socket.                                                    |
| DVC_LB_PROXY_HTTP_PORT                                 | Integer       | 8080    |          | The port to listen on for HTTP requests. Defaults to 8080.                      |
| DVC_LB_PROXY_UNIX_SOCKET_ENABLED                       | True or False | false   |          | Whether to enable the Unix socket. Defaults to false.                           |
| DVC_LB_PROXY_HTTP_ENABLED                              | True or False | true    |          | Whether to enable the HTTP server. Defaults to true.                            |
| DVC_LB_PROXY_SDK_KEY                                   | String        |         | true     | The Server SDK key to use for this instance.                                    |
| DVC_LB_PROXY_PLATFORMDATA_SDKTYPE                      | String        |         |          |                                                                                 |
| DVC_LB_PROXY_PLATFORMDATA_SDKVERSION                   | String        |         |          |                                                                                 |
| DVC_LB_PROXY_PLATFORMDATA_PLATFORMVERSION              | String        |         |          |                                                                                 |
| DVC_LB_PROXY_PLATFORMDATA_DEVICEMODEL                  | String        |         |          |                                                                                 |
| DVC_LB_PROXY_PLATFORMDATA_PLATFORM                     | String        |         |          |                                                                                 |
| DVC_LB_PROXY_PLATFORMDATA_HOSTNAME                     | String        |         |          |                                                                                 |
| DVC_LB_PROXY_SDKCONFIG_EVENT_FLUSH_INTERVAL_MS         | Duration      |         |          | The interval at which events are flushed to the events api in milliseconds.     |
| DVC_LB_PROXY_SDKCONFIG_CONFIG_POLLING_INTERVAL_MS      | Duration      |         |          | The interval at which the SDK polls the config CDN for updates in milliseconds. |
| DVC_LB_PROXY_SDKCONFIG_REQUEST_TIMEOUT                 | Duration      |         |          | The timeout for requests to the config CDN and events API in milliseconds.      |
| DVC_LB_PROXY_SDKCONFIG_DISABLE_AUTOMATIC_EVENT_LOGGING | True or False | false   |          | Whether to disable automatic event logging. Defaults to false.                  |
| DVC_LB_PROXY_SDKCONFIG_DISABLE_CUSTOM_EVENT_LOGGING    | True or False | false   |          | Whether to disable custom event logging. Defaults to false.                     |
| DVC_LB_PROXY_SDKCONFIG_MAX_EVENT_QUEUE_SIZE            | Integer       |         |          | The maximum number of events to be in the queue before dropping events.         |
| DVC_LB_PROXY_SDKCONFIG_FLUSH_EVENT_QUEUE_SIZE          | Integer       |         |          | The minimum number of events to be in the queue before flushing events.         |
| DVC_LB_PROXY_SDKCONFIG_CONFIG_CDN_URI                  | String        |         |          | The URI of the Config CDN - leave unspecified if not needing an outbound proxy. |
| DVC_LB_PROXY_SDKCONFIG_EVENTSAPIURI                    | String        |         |          | The URI of the Events API - leave unspecified if not needing an outbound proxy. |