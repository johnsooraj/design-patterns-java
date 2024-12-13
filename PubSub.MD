# Publisher Subscriber Design Pattern

Here I show the code sample in 3 java classes and 1 interface 

## 1. Interface that needs to be into every Subscriber
This basic example demonstrates how to write a simple "Hello, World!" program in Java.

```java
package org.example;

public interface Subscriber {
    void update(String event, Object data);
}
```
## 2. A Subscriber class implements above interface
So who-ever implement this interface will be able to get an event

```java
package org.example;

import java.util.Map;

public class UserAddedSubscriber implements Subscriber {

    @Override
    public void update(String event, Object data) {
        System.out.println("User Added: " + ((Map<?, ?>) data).get("name"));
    }
}
```
## 3. PubSub Class is responsible for the events
This class implements the logic of looping the subscribers and decide which subscriber needs to triggered

```java
package org.example;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class PubSub {

    private final Map<String, List<Subscriber>> subscribers = new HashMap<>();

    public void subscribe(String event, Subscriber subscriber) {
        subscribers.computeIfAbsent(event, k -> new ArrayList<>()).add(subscriber);
    }

    public void unsubscribe(String event, Subscriber subscriber) {
        List<Subscriber> subs = subscribers.get(event);
        if (subs != null) {
            subs.remove(subscriber);
        }
    }

    public void publish(String event, Object data) {
        List<Subscriber> subs = subscribers.get(event);
        if (subs != null) {
            for (Subscriber subscriber : subs) {
                subscriber.update(event, data);
            }
        }
    }
}

```
## 4. Finally the runner class
It will be the starting point for above logic and create each Subscribers

```java
package org.example;

import java.util.Map;

public class Main {
    public static void main(String[] args) {
        PubSub pubSub = new PubSub();
        Subscriber userAddedSub = new UserAddedSubscriber();

        pubSub.subscribe("userAdded", userAddedSub);
        pubSub.publish("userAdded", Map.of("name", "John"));

    }
}
```
---

This example is applicable for most of the languages like, Typescript