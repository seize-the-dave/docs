# Send events to external systems using Output Bindings

Using bindings, its possible to invoke external resources without tying in to special SDK or libraries.
For a compelete sample showing output bindings, visit this [link](<PLACEHOLDER>).

## 1. Create a binding

An output binding represents a resource that Dapr will use invoke and send messages to.

For the purpose of this guide, we'll use a Kafka binding. You can find a list of the different binding specs [here](../../concepts/bindings/Readme.md).

Create the following YAML file, named binding.yaml, and save this to the /components sub-folder in your application directory.

*Note: When running in Kubernetes, apply this file to your cluster using `kubectl apply -f binding.yaml`*

```
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: myEvent
spec:
  type: bindings.kafka
  metadata:
  - name: brokers
    value: localhost:9092
  - name: publishTopic
    value: topic1
```

Here, we create a new binding component with the name of `myEvent`.<br>
Inside the `metadata` section, we configure Kafka related properties such as the topic to publish the message to and the broker.

## 2. Send an event

All that's left now is to invoke the bindings endpoint on a running Dapr instance.

We can do so using HTTP:

```
curl -X POST -H  http://localhost:3500/v1.0/bindings/myEvent -d '{ data: { "message": "Hi!" } }'
```

As seen above, we invoked the `/binding` endpoint with the name of the binding to invoke, in our case its `myEvent`.<br>
The payload goes inside the `data` field.
