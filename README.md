![Node.js Package](https://github.com/daniloliebel/reportportal-kmm/workflows/Node.js%20Package/badge.svg)

## Install

The installation should be global if newman is installed globally, otherwise - local (replace -g from the command below with -S for a local installation).
Install NEWMAN and REPORTER global.
```bash
$ npm install -g newman newman-reporter-reportportalkmm
```

## Usage

There are two ways to enable this reporter - with command line or programmatically.

#### With CLI

To enable this reporter you have to specify `reportportal` in Newman's `-r` or `--reporters` option.

```bash
$ newman run https://postman-echo.com/status/200 \
    -r reportportalkmm \
    --reporter-reportportalkmm-debug=true \
    --reporter-reportportalkmm-endpoint=http://your-instance.com:8080/api/v1 \
    --reporter-reportportalkmm-token=00000000-0000-0000-0000-000000000000 \
    --reporter-reportportalkmm-launch=LAUNCH_NAME \
    --reporter-reportportalkmm-project=PROJECT_NAME \
    -x
```

Pay attention that you **must** add **-x** or **--suppress-exit-code** parameter while running newman using CLI.

#### Programmatically

```javascript
const newman = require("newman");

newman.run(
    {
        collection: "./collections/newman-test_collection.json",
        reporters: "reportportalkmm",
        reporter: {
            "reportportalkmm": {
                debug: true,
                endpoint: "http://your-instance.com:8080/api/v1",
                token: "00000000-0000-0000-0000-000000000000",
                launch: "LAUNCH_NAME",
                project: "PROJECT_NAME"
            }
        }
    },
    function(err) {
        if (err) {
            throw err;
        }
        console.log("collection run complete!");
    }
);
```

#### Options

Both CLI and programmatic runs support following options:

| Parameter | Description                                                                                                       |
| --------- | ----------------------------------------------------------------------------------------------------------------- |
| token     | User's Report Portal toke from which you want to send requests. It can be found on the profile page of this user. |
| endpoint  | URL of your server. For example 'https://server:8080/api/v1'.                                                     |
| launch    | Name of launch at creation.                                                                                       |
| project   | The name of the project in which the launches will be created.                                                    |
| debug     | Determines whether newman's run should be logged in details.                                                      |

## Docker

This reporter can also be used inside of a docker container.

### Using existing image

The docker image for this reporter is available for download from our docker hub. So, first of all you have to ensure that you have docker installed and running in your system. Otherwise, see <a href="https://docs.docker.com/installation/" target="_blank">installation guideline for
you operating systems</a>.

#### Step 1

Pull the <a href="https://hub.docker.com/r/reportportal/newman" target="_blank">newman docker image with installed reporter</a> from docker hub:

```console
$ docker pull reportportal/newman
```

#### Step 2

Run newman commands on the image:

```bash
$ docker run -t reportportal/newman run https://www.getpostman.com/collections/8a0c9bc08f062d12dcda \
    -r reportportalkmm \
    --reporter-reportportalkmm-debug=true \
    --reporter-reportportalkmm-endpoint=http://your-instance.com:8080/api/v1 \
    --reporter-reportportalkmm-token=00000000-0000-0000-0000-000000000000 \
    --reporter-reportportalkmm-launch=LAUNCH_NAME \
    --reporter-reportportalkmm-project=PROJECT_NAME \
    -x
```

If you want to use this reporter for a specific collection JSON file, you have to mount a directory with this file:

```bash
$ docker run -v ~/collections:/etc/newman -t reportportal/newman run "example_postman-collection.json" \
    -r reportportalkmm \
    --reporter-reportportalkmm-debug=true \
    --reporter-reportportalkmm-endpoint=http://your-instance.com:8080/api/v1 \
    --reporter-reportportalkmm-token=00000000-0000-0000-0000-000000000000 \
    --reporter-reportportalkmm-launch=LAUNCH_NAME \
    --reporter-reportportalkmm-project=PROJECT_NAME \
    -x
```

### Build the docker image from this repository

**Step 1:**

Clone this repository:

```bash
$ git clone https://github.com/daniloliebel/reportportal-kmm.git
```

**Step 2:**

Build the image:

```bash
$ docker build -t reportportal/newman --build-arg VERSION="full semver version".
```

**Step 3:**

Run a collection using the newman image:

```bash
$ docker run -t reportportal/newman run https://www.getpostman.com/collections/8a0c9bc08f062d12dcda \
    -r reportportalkmm \
    --reporter-reportportalkmm-debug=true \
    --reporter-reportportalkmm-endpoint=http://your-instance.com:8080/api/v1 \
    --reporter-reportportalkmm-token=00000000-0000-0000-0000-000000000000 \
    --reporter-reportportalkmm-launch=LAUNCH_NAME \
    --reporter-reportportalkmm-project=PROJECT_NAME \
    -x
```

# Copyright Notice

Licensed under the [Apache License v2.0](LICENSE)

# Contribution and Support

**by Danilo Liebel**
