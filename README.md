# segment-android

Simple and *unofficial* Android wrapper for the [Segment HTTP API](https://segment.com/docs/sources/server/http/).

If you're a novice user, you should use the [official `analytics-android` library](https://segment.com/docs/sources/mobile/android/), which is better maintained, has more features and much better support in case you run into issues.

## Motivations

`segment-android` is a playground for pre-release features planned in `analytics-android`. You'll get access to fewer and less stable, but more powerful features compared to the official library.

Compare to `analytics-android`, `segment-android` library only handles queueing, batching and uploading messages for you. `segment-android` does not support bundled integrations out of the box (so you can't use tools like Flurry with it without extra work). `segment-android` does not [automatically instrument events](https://segment.com/docs/spec/mobile/), nor does it automatically collect [context data](https://segment.com/docs/spec/common/#context-fields-automatically-collected).

`analytics-android` has 0 dependencies. This makes some of the core in `analytics-android` larger, since it has to write more complex HTTP logic, JSON parsing and persistence code. `segment-android` simply delegates this work to [OkHttp](https://github.com/square/okhttp) + [Retrofit](https://github.com/square/retrofit), [Moshi](https://github.com/square/moshi) and [Tape](https://github.com/square/tape). This makes the `segment-android` core much lighter, especially if you are using these libraries already. If you aren't, `segment-android` will be much heavier for your app since it will add these dependencies into your app.

This also makes the core in `segment-android` simpler, since the heavy lifting is done by other libraries. Since `segment-android` does not support bundled integrations out of the box, its core is further simplified.

## Features

Like [`analytics-java`](https://github.com/segmentio/analytics-java), `segment-android` supports interceptors. Interceptors are a powerful mechanism to transform messages before they're processed by the library. You can use interceptors to rewrite or even skip messages. For example, if your applications supports a way for users to opt out of data collection, you can write an interceptor that returns `null` if the user has opted out.

`segment-android` also gives you way to listen on the status of published actions. For example, if you want to verify that a `flush` is completed for some critical events before proceeding, you can use the following snippet:

```java
try {
  segment.flush().get();
} catch (InterruptedException e) {
  // The thread was interrupted before the task was completed.
} catch (ExecutionException e) {
  // Something went wrong during the flush. Inspect the exception supplied to figure out why.
}
```

## Usage

Initialize a client:

```java
Segment segment = new Segment.Builder().writeKey(writeKey).context(context).build();
```

Create user actions:

```java
TrackMessage message = segment.newTrack("Event A").properties(properties).build();
```

Enqueue actions:

```java
segment.enqueue(message);
```

Upload actions:

```java
segment.flush();
```

## Download

Download [the latest JAR](https://search.maven.org/remote_content?g=com.f2prateek.segment&a=segment&v=LATEST) or grab via Maven:
```xml
<dependency>
  <groupId>com.f2prateek.segment</groupId>
  <artifactId>segment</artifactId>
  <version>LATEST</version>
</dependency>
```

or Gradle:
```groovy
compile 'com.f2prateek.segment:segment:+'
```

Snapshots of the development version are available in [Sonatype's `snapshots` repository](https://oss.sonatype.org/content/repositories/snapshots/).
# stripe-android
