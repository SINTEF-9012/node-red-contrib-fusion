# node-red-contrib-fusion
A Node-RED node to fusion incoming messages by category.


This node will save all incoming messages in memory and publish dictionaries of messages whenever a message is updated.

Dictionaries must be configured by sending a message with the topic `fusion-configuration`. Its payload is a configuration object. The key `output-topic`  configures the topic of the published dictionary *(`fusion` by default)*. The key `input-topics` is a list of topics to fusion *(empty list by default)*. When `allow-undefined` is true, dictionaries containing undefined values will be published as well *(false by default)*.

Dictionaries may be deleted by sending a message with the topic `fusion-configuration` and an empty payload.

All other messages with topics different to `fusion-configuration` will simply be saved for usage in dictionaries. In the current implementation, the messages are saved forever, or until the next NodeRed deployment.

### Example

##### Configuration

```json
{
  "topic": "fusion-configuration",
  "payload": {
    "output-topic": "a-and-b-merged",
    "input-topics": [
      "topicA",
      "topicB"
    ],
    "allow-undefined": false
  }
}
```

##### Input messages

```json
{
  "topic": "topicA",
  "payload": 123
}
```


```json
{
  "topic": "topicB",
  "payload": 456,
  "otherfield": "abc"
}
```

##### Output message
```json
{
  "topic": "a-and-b-merged",
  "payload": {
    "topicA": {
      "topic": "topicA",
      "payload": 123
    },
    "topicB": {
      "topic": "topicB",
      "payload": 456,
      "otherfield": "abc"
    }
  }
}
```