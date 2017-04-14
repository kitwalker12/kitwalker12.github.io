---
layout: post
title: Pub/Sub with Go-Micro
date: 2017-04-014 00:00:00 -07:00
comments: true
---

Here's a small tutorial on how to do pub/sub with RabbitMQ in golang using [go-micro](https://github.com/micro/go-micro) which is a pluggable RPC framework for microservices.

We'll be creating a service which creates two subroutines. One will be listening on a routing key on a rabbitmq exchange, while another writes to the exchange every second.

Here's your `main.go`

{% highlight go %}
package main

import (
  "fmt"
  "log"
  "time"

  "github.com/micro/go-micro/broker"
  "github.com/micro/go-micro/cmd"
  _ "github.com/micro/go-plugins/broker/rabbitmq"
)

func pub() {
  cmd.Init()

  if err := broker.Init(); err != nil {
    log.Fatalf("Broker Init error: %v", err)
  }
  if err := broker.Connect(); err != nil {
    log.Fatalf("Broker Connect error: %v", err)
  }

  tick := time.NewTicker(time.Second)
  i := 0
  for _ = range tick.C {
    msg := &broker.Message{
      Header: map[string]string{
        "id": fmt.Sprintf("%d", i),
      },
      Body: []byte(fmt.Sprintf("%d: %s", i, time.Now().String())),
    }
    if err := broker.Publish("service.topic", msg); err != nil {
      log.Printf("[pub] failed: %v", err)
    } else {
      fmt.Println("[pub] pubbed message:", string(msg.Body))
    }
    i++
  }
}

func sub() {
  cmd.Init()

  if err := broker.Init(); err != nil {
    log.Fatalf("Broker Init error: %v", err)
  }
  if err := broker.Connect(); err != nil {
    log.Fatalf("Broker Connect error: %v", err)
  }

  _, err := broker.Subscribe("service.topic", func(p broker.Publication) error {
    fmt.Println("[sub] received message:", string(p.Message().Body), "header", p.Message().Header)
    return nil
  })
  if err != nil {
    fmt.Println(err)
  }
}

func main() {
  forever := make(chan struct{})

  go pub()
  go sub()

  <-forever
}
{% endhighlight %}

Let's use docker to create the service and dependent servers like rabbitmq and consul which is need by go-micro.

Here's a simple `Dockerfile`

{% highlight bash %}
FROM golang

COPY . /go/src/github.com/my-repo/pubsub
WORKDIR /go/src/github.com/my-repo/pubsub

RUN go get ./...
RUN go build
CMD pubsub
{% endhighlight %}

Here's the `docker-compose.yml` file I used where we can use environment variables to set the rabbitmq connection details.

{% highlight yaml %}
version: "3"
services:
  services:
    build: .
    ports:
      - "8080:8080"
    environment:
      - MICRO_REGISTRY_ADDRESS=consul:8500
      - MICRO_BROKER_ADDRESS=amqp://admin:password@rabbit:5672/
      - MICRO_BROKER=rabbitmq
    depends_on:
      - consul
      - rabbit

  consul:
    image: progrium/consul:latest
    command: -server -bootstrap -rejoin
    ports:
      - "8500"

  rabbit:
    image: rabbitmq:3-management
    ports:
      - "5672"
    environment:
      RABBITMQ_DEFAULT_USER: admin
      RABBITMQ_DEFAULT_PASS: password
{% endhighlight %}

If you prefer to not use environment variables for setting the rabbitmq address or exchange value, you can do it on the code by instantiating a new rabbitmq broker like this

```
r := rabbitmq.NewBroker(micro.Addrs("amqp://admin:password@rabbit:5672/"))
//For setting exhange do
r := rabbitmq.NewBroker(
    rabbitmq.Exchange("foo"),
)
//Or
rabbitmq.DefaultExchange = "foo"
```
