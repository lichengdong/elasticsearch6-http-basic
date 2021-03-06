
[![Build Status](https://travis-ci.org/Cleafy/elasticsearch6-http-basic.svg?branch=master)](https://github.com/Cleafy/elasticsearch6-http-basic)


# HTTP Basic auth for ElasticSearch 6.x

This plugin provides an extension of ElasticSearchs HTTP Transport module to enable **HTTP basic authentication** and/or
**Ip based authentication**.

Requesting `/` does not request authentication to simplify health check configuration.

There is no way to configure this on a per index basis.


## Version Mapping

|     Http Basic Plugin       | elasticsearch                |
|-----------------------------|------------------------------|
| v1.0.0                      |                       6.2.4+ |


## Installation

Download the desired version from https://github.com/Cleafy/elasticsearch6-http-basic/releases and copy it to `plugins/http-basic`.

## Configuration

Once the plugin is installed it can be configured in the [elasticsearch modules configuration file](http://www.elasticsearch.org/guide/en/elasticsearch/reference/current/setup-configuration.html#settings). See the [elasticserach directory layout information](http://www.elasticsearch.org/guide/en/elasticsearch/reference/current/setup-dir-layout.html) for more information about the default paths of an ES installation.

|     Setting key                   |  Default value               | Notes                                                                   |
|-----------------------------------|------------------------------|-------------------------------------------------------------------------|
| `http.basic.enabled`              | true                         | **true** disables the default ES HTTP Transport module                  |
| `http.basic.username`             | "admin"                      |                                                                         |
| `http.basic.password`             | "admin_pw"                   |                                   
| `http.basic.log`                  | false                        | enables plugin logging to ES log. Unauthenticated requests are 
| `http.basic.ipwhitelist`          | IP白名单                      | 在此列表中的ip，可以不输入带入用户名密码即可访问es服务，用英文逗号,分割
always logged.                                         |

**Be aware that the password is stored in plain text.**

## Http basic authentication

see [this article](https://en.wikipedia.org/wiki/Basic_access_authentication)

## Configuration example

The following code enables plugin logging, sets user and password.提醒：属性的:后面需要加一个空格！！！

```
http.basic.enable: true
http.basic.log: true
http.basic.username: "some_user"
http.basic.password: "some_password"
http.basic.ipwhitelist: ip1,ip2,ip3
```

## Testing

**note:** localhost is a whitelisted ip as default.
Considering a default configuration with **my_username** and **my_password** configured.

Correct credentials
```
$ curl -v --user my_username:my_password no_local_host:9200/foo # works (returns 200) (if credentials are set in configuration)
```

Wrong credentials
```
$ curl -v --user my_username:wrong_password no_local_host:9200/    # health check, returns 200 with  "{\"OK\":{}}" although Unauthorized
$ curl -v --user my_username:password no_local_host:9200/foo       # returns 401
```


## Issues

Please file your issue here: 
https://github.com/Cleafy/elasticsearch6-http-basic/issues
