# WebdriverIO Selenoid Standalone Service

Buddy service for [wdio-selenoid-standalone-service](https://github.com/JustSittinHere/wdio-selenoid-standalone-service) that starts the [Selenoid UI](https://aerokube.com/selenoid-ui/latest/) container to allow you to view all tests running inside Selenoid.

__Note:__ This service requires [wdio-selenoid-standalone-service](https://github.com/JustSittinHere/wdio-selenoid-standalone-service) to be used, it is not designed to connect to existing Selenoid grids

## Benefits of Selenoid-Ui

* View the actual desktop and browser for all running tests
* Unlock the session and interact with the browser being controlled as if it was local
* View all session logs for increased debugging

## Installation

Before starting make sure you have [Docker](https://www.docker.com/) installed and [wdio-selenoid-standalone-service](https://github.com/JustSittinHere/wdio-selenoid-standalone-service) added to your wdio.conf

The easiest way is to keep `wdio-selenoid-standalone-ui-service` as a devDependency in your `package.json`.

```json
{
    "devDependencies": {
        "wdio-selenoid-standalone-ui-service": "^1.0.0"
    }
}
```

You can simple do it by:

```bash
npm install wdio-selenoid-standalone-ui-service --save-dev
```

or

```bash
yarn add wdio-selenoid-standalone-ui-service --dev
```

Instructions on how to install `WebdriverIO` can be found [here.](https://webdriver.io/docs/gettingstarted)

## Configuration

### Service configuration

In order to use the service you need to add `selenoid-standalone-ui` to your service array and change the default wdio path prop to match selenoid

```js
export.config = {
    // ...
    services: [
        ['selenoid-standalone-ui']
    ],
    // ...
};
```

The above assumes you are using the default settings from [wdio-selenoid-standalone-service](https://github.com/JustSittinHere/wdio-selenoid-standalone-service), if not, you can change them with the options below.

### Capabilities configuration

After adding the service, you will need to add the `selenoid:options` prop to your browser capabilities.  Open `wdio.conf` and adjust the capabilities array to include the following:

```js
export.config = {
    // ...
    capabilities: [
        {
            // ...
            browserName: 'chrome',
            'selenoid:options': {
                enableVnc: true,
            },
            // ...
        },
    ],
    // ...
};
```

More information about Selenoid-Ui can be found [here](https://aerokube.com/selenoid-ui/latest/)

## Options

The following options can be added to the service options.  They are all optional

### skipAutoPullImage

On startup, skip pulling the version of selenoid-ui required if it's not already available locally

Type: `Boolean`

Default: `false`

### selenoidContainerName

The name of the selenoid service container name to connect to

Type: `String`

Default: `wdio_selenoid`

### selenoidUiContainerName

The name this service should give to the created Selenoid-UI container

Type: `String`

Default: `wdio_selenoidui`

### selenoidUiVersion

If you want to always use a specific version of Selenoid-UI, you can fix the version here.  If unset, this service will use the image tagged with latest-release

Type: `String`

Default: `latest-release`

### port

Port which the Selenoid-Ui container should use to accept incoming connections

Type: `Number`

Default: `8080`

### selenoidPort

The port of the running Selenoid instance for Selenoid-UI to connect to

Type: `Number`

Default: `4444`

## Warning

The following options are experimental and can cause unexpected problems.  Use with caution
### dockerArgs

Any additional arguments you want to pass to docker run when starting the container.  [docker run options](https://docs.docker.com/engine/reference/commandline/run/#options)

Type: `String[]`

Default: `null`

### selenoidUiArgs

Any additional arguments you want to pass to selenoid-ui when starting the container.  [docs](https://aerokube.com/selenoid-ui/latest)

Type: `String[]`

Default: `null`

## Example Options

```js
export.config = {
    // ...
    path: 'wd/hub'
    services: [
        [
            'selenoid-standalone-ui', { 
                skipAutoPullImage: 'false,
                selenoidContainerName: 'wdio_selenoid',
                selenoidUiContainerName: 'wdio_selenoidui',
                selenoidUiVersion: 'latest-release',
                port: 8080,
                selenoidPort: 4444
            },
        ],
    ],
    // ...
};
```

## Troubleshooting

### Service cannot connect to Selenoid

Ensure you have added [wdio-selenoid-standalone-service](https://github.com/JustSittinHere/wdio-selenoid-standalone-service) to wdio.conf

### I cannot access the VNC session from the UI

Ensure you have added the required `selenoid:options` to your browser capabilities to `enableVNC`