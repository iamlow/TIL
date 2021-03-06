## Installation on ubuntu 16.04

**preinstallation:**

* https://docs.fluentd.org/v1.0/articles/before-install

### installation package

```sh
curl -L https://toolbelt.treasuredata.com/sh/install-ubuntu-xenial-td-agent3.sh | sh
```

**start:**

```sh
sudo systemctl start td-agent.service
```

## Check

**show log messages:**

```sh
tail -f /var/log/td-agent/td-agent.log
```

**send log messages:**

```sh
#http
curl -X POST -d 'json={"json":"message"}' http://localhost:8888/debug.test
```

## Test

Setting fluentd configuration

```sh
sudo vi /etc/td-agent/td-agent.conf
```

Fluentd daemon must be launched with a tcp source configuration

```xml
<source>
  type forward
  port 24224
</source>
```

To quickly test your setup, add a matcher that logs to the stdout

```xml
<match app.**>
  type stdout
</match>
```

### how to use python fluentd logger 

**installation python fluentd logger and send log messages:**

```
pipenv --three
pipenv install fluent-logger
pipenv shell
python
>>> from fluent import sender
>>> logger = sender.FluentSender('app')
>>> logger.emit('follow', {'from': 'userA', 'to': 'userB'})
>>> logger.close()
```

## Docker

### installation docker image

```sh
docker run -d -p 24224:24224 -p 24224:24224/udp -v /data:/fluentd/log --name fluentd fluent/fluentd
```

### show log message

```sh
tail -f /data/data.b58650af3857144aa19c0330883935da0.log
```

**References:**

* https://docs.fluentd.org/v1.0/articles/quickstart
* https://docs.fluentd.org/v1.0/articles/python
* https://github.com/fluent/fluent-bit/
* https://github.com/fluent/fluent-logger-python
