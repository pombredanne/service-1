Buildtime Trend as a Service
============================

Visualise what's trending in your build process

[![Buildtime Trend as a Service](http://img.shields.io/badge/release-v0.2-blue.svg)](https://github.com/buildtimetrend/service/releases/latest)
[![Buildtime Trend as a Service (dev)](http://img.shields.io/badge/dev-v0.3.dev-blue.svg)](https://github.com/buildtimetrend/service/zipball/master)

[![Build Status](https://travis-ci.org/buildtimetrend/service.svg?branch=master)](https://travis-ci.org/buildtimetrend/service)
[![Code Health](https://landscape.io/github/buildtimetrend/service/master/landscape.svg)](https://landscape.io/github/buildtimetrend/service/master)
[![Scrutinizer Code Quality](https://scrutinizer-ci.com/g/buildtimetrend/service/badges/quality-score.png?b=master)](https://scrutinizer-ci.com/g/buildtimetrend/service/?branch=master)
[![Codacy Badge](https://www.codacy.com/project/badge/4ff71ed6b542493ab6f407f4c37aeb01)](https://www.codacy.com/public/ruleant/service)
[![Stack Share](http://img.shields.io/badge/tech-stack-0690fa.svg)](http://stackshare.io/ruleant/buildtime-trend)

[![Buildtime trend](https://buildtimetrend-dev.herokuapp.com/badge/buildtimetrend/service/latest)](https://buildtimetrend-dev.herokuapp.com/dashboard/buildtimetrend/service/index.html)
[![Total builds](https://buildtimetrend-dev.herokuapp.com/badge/buildtimetrend/service/builds/month)](https://buildtimetrend-dev.herokuapp.com/dashboard/buildtimetrend/service/index.html)
[![Percentage passed build jobs](https://buildtimetrend-dev.herokuapp.com/badge/buildtimetrend/service/passed/month)](https://buildtimetrend-dev.herokuapp.com/dashboard/buildtimetrend/service/index.html)

This project contains the files that offer Buildtime Trend as a service. The [Buildtime Trend library](https://github.com/buildtimetrend/python-lib) powers this service.

Usage
-----

The service is available on Heroku :
- production : https://buildtimetrend.herokuapp.com/

    Currently in beta.

    Available for free for Open Source projects (public on GitHub), thanks to [Keen.io](https://keen.io) for hosting the timing data.

    Private projects will be supported soon.

- development : https://buildtimetrend-dev.herokuapp.com/

    Used for development, expect frequent changes, accepts only a limited number of projects.
    [Contact us](#Contact) if you'd like to test the development version. It's recommended to use the production version.

Or you can clone the project and deploy it to Heroku.

Or run it on your own server :

```bash
python service.py
```

This will launch a CherryPy instance hosting the service on port 5000.

## Components

- [Index page](#index-page)
- [Dashboard](#dashboard)
- [Shield badges](#shield-badges)
- [Process Travis CI build log](#process-travis-ci-build-log)

### Index page
- path : `/`

### Dashboard

Display a dashboard with Buildtime Trend charts

- path : `/dashboard`
- usage : `/dashboard/repo_owner/repo_name`
- parameters :
  - `repo_owner` : name of the Github repo owner, fe. `buildtimetrend`
  - `repo_name` : name of the Github repo, fe. `service`

> **Remark :** When visiting `/dashboard` (without parameters), an overview of all hosted projects is displayed.

### Shield badges

Generate a shield badges

- path : `/badge`
- usage : `/badge/repo_owner/repo_name/badge_type/interval`
- parameters :
  - `repo_owner` : name of the Github repo owner, fe. `buildtimetrend`
  - `repo_name` : name of the Github repo, fe. `service`
  - `badge_type` : type of badge, options :
    - `latest` : buildtime of last build job
    - `avg` : average buildtime of buildjobs in period set by `interval` (default)
    - `jobs` : number of build jobs in period set by `interval`
    - `builds` : number of builds in period set by `interval`
    - `passed` : percentage of successful build jobs during `interval`
  - `interval` : time interval, options :
    - `week` (default) : events of last week (last 7 days)
    - `month` : events of last month (last 30 days)
    - `year` : events of last year (last 52 weeks)

#### Badge examples
- ![Latest build duration](https://buildtimetrend-dev.herokuapp.com/badge/buildtimetrend/service/latest) : `/badge/buildtimetrend/service/latest`
- ![Average buildtime (week)](https://buildtimetrend-dev.herokuapp.com/badge/buildtimetrend/service/avg/week) : `/badge/buildtimetrend/service/avg/week`
- ![Average buildtime (month)](https://buildtimetrend-dev.herokuapp.com/badge/buildtimetrend/service/avg/month) : `/badge/buildtimetrend/service/avg/month`
- ![Average buildtime (year)](https://buildtimetrend-dev.herokuapp.com/badge/buildtimetrend/service/avg/year) : `/buildtimetrend/service/avg/year`
- ![Build jobs (week)](https://buildtimetrend-dev.herokuapp.com/badge/buildtimetrend/service/jobs/week) : `/badge/buildtimetrend/service/jobs/week`
- ![Build jobs (month)](https://buildtimetrend-dev.herokuapp.com/badge/buildtimetrend/service/jobs/month) : `/badge/buildtimetrend/service/jobs/month`
- ![Build jobs (year)](https://buildtimetrend-dev.herokuapp.com/badge/buildtimetrend/service/jobs/year) : `/buildtimetrend/service/jobs/year`
- ![Builds (week)](https://buildtimetrend-dev.herokuapp.com/badge/buildtimetrend/service/builds/week) : `/badge/buildtimetrend/service/builds/week`
- ![Builds (month)](https://buildtimetrend-dev.herokuapp.com/badge/buildtimetrend/service/builds/month) : `/badge/buildtimetrend/service/builds/month`
- ![Builds (year)](https://buildtimetrend-dev.herokuapp.com/badge/buildtimetrend/service/builds/year) : `/buildtimetrend/service/builds/year`
- ![% passed build jobs (week)](https://buildtimetrend-dev.herokuapp.com/badge/buildtimetrend/service/passed/week) : `/badge/buildtimetrend/service/passed/week`
- ![% passed build jobs (month)](https://buildtimetrend-dev.herokuapp.com/badge/buildtimetrend/service/passed/month) : `/badge/buildtimetrend/service/passed/month`
- ![% passed build jobs (year)](https://buildtimetrend-dev.herokuapp.com/badge/buildtimetrend/service/passed/year) : `/badge/buildtimetrend/service/passed/year`

### Process Travis CI build log

Loads a Travis CI build log file, processes it and sends the data to Keen.io.

- path : `/travis`
- usage : `/travis/repo_owner/repo_name/build`
- parameters :
  - `repo_owner` : name of the Github repo owner, fe. `buildtimetrend`
  - `repo_name` : name of the Github repo, fe. `service`
  - `build` : Travis CI build ID

OR

  - `payload` : Travis CI notification payload, more info in the [Travis CI documentation](http://docs.travis-ci.com/user/notifications/#Webhook-notification)

To trigger the service at the end of a Travis CI build, add this to your `.travis.yml` file:

```yaml
    notifications:
      webhooks:
        # trigger Buildtime Trend Service to parse Travis CI log and send result to Keen.io
        - https://buildtimetrend.herokuapp.com/travis
```

When Buildtime Trend Service is triggered by a Travis CI notification, it will get the necessary parameters (repo name and build number) from the `payload` that is passed by Travis CI. This will trigger loading and parsing the Travis CI log of the corresponding build, the analysed data is stored in the Keen.io database.


Config file
-----------

Add a configfile named `config_service.yml` based on `config_sample.yml` to configure the way the service behaves.

- `denied_repo` : defines which repos are not allowed by the service. If the `denied_repo` setting is not defined, all repos are allowed (unless `allowed_repo` is defined, see below). If a substring matches the repo name, it is denied, so fe. `my_name` will disallow `my_name/my_first_repo` and `my_name/another_repo`. A complete repo name is valid as well.
> **Remark :** Setting `denied_repo` takes precedence over `allowed_repo`, if a repo name matches a rule in `denied_repo` setting, the repo will be denied, even if it matches a rule in `allowed_repo`.
Multiple entries are allowed, fe. :

```yaml
buildtimetrend:
  denied_repo:
    - "my_name" # denying all repo names that contain my_name
    - "another_name/some_repo" # denies this specific repo
```

- `allowed_repo` : defines which repos are allowed by the service. If the `allowed_repo` setting is not defined, all repos are allowed (unless denied by the `denied_repo` setting). If substring matches the repo name, it is allowed, so fe. `my_name` will allow `my_name/my_first_repo` and `my_name/another_repo`. A complete repo name is allowed as well.
> **Remark :** Setting `denied_repo` takes precedence over `allowed_repo`, if a repo name matches a rule in `denied_repo` setting, the repo will be denied, even if it matches a rule in `allowed_repo`.
Multiple entries are allowed, fe. :

```yaml
buildtimetrend:
  allowed_repo:
    - "my_name" # allowing all repo names that contain my_name
    - "another_name/some_repo" # allows this specific repo
```

- `travis_account_token` : define to enable checking Travis CI notification Authorization header. More info on Travis CI Webhook Authorization and where to find the Account token : http://docs.travis-ci.com/user/notifications/#Authorization-for-Webhooks

> **Remark :** the account token should be the one of the user who created the repo on Travis CI.

It can also be defined with the `TRAVIS_ACCOUNT_TOKEN` environment variable.

- `loglevel` : defines loglevel, possible values : `DEBUG`, `INFO`, `WARNING` (default), `ERROR`
It can also be defined with the `BTT_LOGLEVEL` environment variable.

Dependencies
------------

- `buildtimetrend` : [Buildtime Trend library](https://github.com/buildtimetrend/python-lib)
- `cherrypy` : [CherrPy](http://www.cherrypy.org/) A Minimalist Python Web Framework, making the API available as a web service

Bugs and feature requests
-------------------------

Please report bugs and add feature requests in the Github [issue tracker](https://github.com/buildtimetrend/python-lib/issues).


Credits
-------

For an overview of who contributed to create Buildtime trend, see [Credits](https://github.com/buildtimetrend/python-lib/wiki/Credits).

Contact
-------

Website : https://buildtimetrend.github.io/service

Mailinglist : [Buildtime Trend Community](https://groups.google.com/d/forum/buildtimetrend-dev)

Follow us on [Twitter](https://twitter.com/buildtime_trend), [Github](https://github.com/buildtimetrend) and [OpenHub](https://www.openhub.net/p/buildtime-trend).


License
-------

Copyright (C) 2014-2015 Dieter Adriaenssens <ruleant@users.sourceforge.net>

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU Affero General Public License as published by
the Free Software Foundation, either version 3 of the License, or
any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU Affero General Public License for more details.

You should have received a copy of the GNU Affero General Public License
along with this program.  If not, see <http://www.gnu.org/licenses/>.
