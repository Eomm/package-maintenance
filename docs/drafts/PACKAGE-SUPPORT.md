# Baseline practice - Document support levels

Package maintainers provide different level of support in the large npm ecosystem.
A mismatch between the support a maintainer is planning to
provide and that expected by a consumer of a module will cause
friction and stress on both the package maintainer and
the consumer. This is further complicated because the level of support
provided for dependencies of a package may be different than that
targeted by the package a consumer is directly consuming.

This baseline practice suggests a standardized set of information to
quantify the level of support that a package maintainer wishes/is able to
provide. The goal is to close the gap between the expectations from the
consumer and those of the maintainer and reduce the friction and stress
that can arise due to the gap.

There are 4 main dimensions that we standardize in this practice which are:  

* `target`: the level of support that the package maintainer aims to provide
* `response`: how quickly the maintainer chooses to, or is able to, respond to issues
* `response-paid`: how quickly the maintainer will respond to issues if paid
* `backing`: how the project can be supported

In addition a `url` field can optionally be provided with a link to more detailed
support information.

For each of these dimensions the potential options are not an ordered list.
Instead like licenses each ID will identify a unique option. If you wish to
identify your package with an option which is not yet listed, please PR your
option into the lists in the sections which follow. 
The values can be any string, those which are not documented in the lists within
this document considered "custom" and may ignored or flagged by any tooling that
consumes these elements.

## Support information

This field documents the maintainers’ expectations, adding more useful metadata.
The support information is stored in the `package.json` file in the root directory of the project.

The file expects this example structure:

```json
{
  "name": "my-package",
  "version": "1.0.0",
  "description": "a package.json file",
  "support": {
    "versions": [
      {
        "version": "^1.0.0",
        "target": "abandoned",
        "response": "none",
        "response-paid": "regular-1",
        "backing": [
          "paid-support"
        ],
        "expires": "P1Y",
        "contact": {
          "issues": {
            "url": "http://support.it/issue"
          },
          "security": {
            "file": "./SECURITY.md"
          },
          "paid-channel": {
            "email": "mailto:iwantmoney@nodejs.com"
          }
        }
      },
      {
        "version": ">=2.0.0",
        "target": "lts",
        "response": "regular-7",
        "response-paid": "regular-1",
        "backing": "hobby",
        "expires": "P1M",
        "contact": {
          "issues": {
            "url": "http://support.it/issue"
          },
          "security": {
            "email": "mailto:security@nodejs.com"
          },
          "paid-channel": {
            "email": "mailto:iwantmoney@nodejs.com"
          }
        }
      }
    ]
  }
}
```

The default for packages created by individuals for their own use should most often add this support field:

```json
{
  "support": {
    "versions": [
      {
        "version": "*",
        "target": "lts",
        "response": "best-effort",
        "backing": [ "hobby" ],
        "expires": "P1Y2M10DT2H30M",
        "contact": {
          "issues": {
            "url": "https://github.com/nodejs/package-maintenance/issues"
          },
          "security": {
            "email": "mailto:security@nodejs.com"
          }
        }
      }
    ]
  }
}
```

This reflects that the maintainer in this case may have no interest in ensuring that the package works
outside of their use case.

### Support `versions`

The `versions` key is an array of JSON that contains all the useful information regarding a defined
package version.

### Support `version`

The support version accepts a [semver range](https://semver.io/) value that define the versions of the
module that applies that configuration.

Note that the `version` ranges could overlap each other: it means that the maintainers provide more
than one support type, and it is up to users choose the support level that best fits their need.

### Support `target`

The support target captures the level of support that the package maintainer
aims to provide.  The standardized options are as follows:

| Value         | Versions   | Description |
|---------------|------------|-------------|
| `abandoned`   |            | Not recommended for use. The package is deprecated or no longer maintained
| `none`        |            | Use at your own risk, no active support. May or may not work for a given Node.js version
| `all`         | 8,10,11,12 | The package is maintained for versions of Node.js including both LTS and non-LTS releases. It may be necessary to accept semver-major level (ie. breaking) changes into that application in order to receive essential fixes. Documentation for the package will include the non-LTS releases for which the package is still maintained. 
| `lts`         | 8,10       | The package is maintained for the Node.js LTS releases (both in Active and Maintenence mode). Anyone creating an application using an LTS version of Node.js and using the latest major version of LTS adopting modules will not have to accept semver-major level (ie. breaking) changes into that application in order to receive essential fixes. Full details are available [here](https://github.com/nodejs/package-maintenance/issues/119)
| `active`      | 10,12      | All releases not in maintenance
| `lts_active`  | 10         | All releases both LTS and active
| `lts_latest`  | 10         | The package is maintained only for the Latest Node.js version. You will be required to update to the latest LTS Node.js version in order to ensure you can use new versions/get security fixes
| `current`     | 12         | The latest release from "all"

Based on the current active Node.js releases which are 8.x (Maintenance LTS), 10.x (Active LTS) and 12.x (Current).

### Support `response` and `response-paid`

Support response quantifies how quickly the maintainer chooses to, or is able to, respond to issues:

| Value | Description |
|-------|-------------|
| `none`         | Don't expect a response, the package is not being actively maintained
| `best-effort`  | The maintainer is interested in fixing/discussing issues, however, there should be no expectation on response times. If and when the maintainer has time they may respond.
| `regular-7`    | There are dedicated resources who regularly maintain the module, expected response time is 7 days or less for a "we read your issue" response. Further work will depend on prioritization of the issue by the maintainer team.
| `regular-1`    | There are dedicated resources who regularly maintain the module, expected response time is 1 days or less for a "we read your issue" response. Further work will depend on prioritization of the issue by the maintainer team.
| `24-7`         | There are dedicated resources who regularly maintain the module and they are available 24/7. You can expect to be able to contact the maintainers and get an initial response with 6 hours.

Support-response indicates the expectation unless you have paid one of the maintainers or a company that provides
3rd party support. If paid support is available then Support-response-paid is an array of one or more options listing the
options that the maintainer knows are available.


### Support `backing`

This section tracks how the project is funding.
It can be a single value or an array of values, for example: [x, y, z]. This supports cases where the
backing comes from more than one source. The documented options include:

| Value | Description |
|-------|-------------|
| `none`         | There is nobody backing this module
| `hobby`        | The single maintainer maintains the package for fun, does not get any support to continue maintenance.
| `sponsored`    | The single maintainer actively maintains the package but depends on sponsorship to be able to continue to maintain the package. Consider supporting this sponsorship through patreon etc.
| `bounty`       | The package is maintained through the use of a bounty service
| `project`      | The package is maintained under the auspices of a larger project (ex Node.js project).
| `foundation`   | The package is maintained and supported under the auspices of a Foundation.
| `company`      | The package is maintained and supported by a corporate entity but may not be related to their product or service offerings.
| `commercial`   | The package is maintained and supported by a corporate entity as part of supporting their products.
| `paid-support` | The package is maintained and supported through paid support contracts.
| `freemium`     | Basic version of the package it provided for free, premium version is available at a cost.
| `donations`    | The project can be funded by any donations.
| `open-collective` | The project can be funded by donations via `open-collective`.


### Support `expires`

The expire field defines the ending of the term of a support entry.
It is a string in the [ISO 8601](https://www.iso.org/iso-8601-date-and-time-format.html) format.

This standard can express:
+ _a date_ (ex: `2007-03-01T13:00:00Z`): the value is the expiring date. This value is strict and not dynamic.
+ _time range_ (ex: `2007-03-01T13:00:00Z/2008-05-11T15:30:00Z`): the value is the range of validity of the supported version. This value is strict and not dynamic.
+ _only duration_ (ex: `P1Y2M10DT2H30M`): the value must be added to the last release date. This value is dynamic.

To understand if the version is expired or not, the user needs to do the operation for _duration_:

`date of last release in the version range` **+** `expires value` **>** `now` => the support is not expired
`date of last release in the version range` **+** `expires value` **<** `now` => the support is expired


### Support `contact`

The contact field is an object that shows to users all possible channels to contact maintainer(s):

- `security`: a contact for security issues
- `paid-channel`: a direct contact for paid support

Each field is an object with more information like an external URL, an email, or a project file.

```json
"contact": {
  "security": {
    "file": "./SECURITY.md"
  },
  "paid-channel": {
    "url": "http://support.it/paid-suport",
    "email": "mailto:paid-channel@node.js"
  }
},
```

## Authoritative

A published module may contain outdates support information, so the valid support information refer
to the published module with the `latest` tag.