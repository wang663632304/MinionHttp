Minion Http for Android
=======================

An asynchronous HTTP & SPDY callback-based Http client for Android, based predominantly on [Android Async Http][1]. All requests and data parsing are made outside of your app's main UI thread.


Features
--------
* All features of [OkHttp][2]:
> + SPDY support allows all requests to the same host to share a socket.
> + Connection pooling reduces request latency (if SPDY isn’t available).
> + Transparent GZIP shrinks download sizes.
> + Response caching avoids the network completely for repeat requests.

> As OkHttp, the client perseveres when the network is troublesome: it will silently recover from common connection problems. If your service has multiple IP addresses OkHttp will attempt alternate addresses if the first connect fails. This is necessary for IPv4+IPv6 and for services hosted in redundant data centers. OkHttp also recovers from problematic proxy servers and failed SSL handshakes.

> The core module implements the familiar java.net.HttpURLConnection API. And the optional okhttp-apache module implements the Apache HttpClient API.

* Some features of [Android Async Http][1]
> + Make **asynchronous** HTTP requests, handle responses in **anonymous callbacks**
> + HTTP requests happen **outside the UI thread**
> + Requests use a **threadpool** to cap concurrent resource usage
> + GET/POST **params builder** (RequestParams)
> ~~ + **Multipart file uploads** with no additional third party libraries ~~ (under construction)
> + Tiny size overhead to your application
> + Optional built-in response parsing into **JSON** (JsonHttpResponseHandler)
> ~~ + Optional **persistent cookie store**, saves cookies into your app's SharedPreferences ~~ (under construction)


Installation & Basic Usage
--------------------------
* Gradle Support (under construction)
* Maven Support (under construction)
* Jar available (under construction)

For now, you will need to import library project to your application, and if the IDE doesn't detect OkHttp and Gson jar libraries, it will be necessary to add these ones manually.

Create MinionHttpClient instance and make a request:

```java
MinionHttpClient client = new MinionHttpClient();
client.get("http://www.google.com", new MinionStringListener() {
    @Override
    public void onSuccess(String response) {
        System.out.println(response);
    }
});
```


Recommended Usage: Make a Static Http Client
--------------------------------------------
In this example, we’ll make a http client class with static accessors to make it easy to communicate with Twitter’s API.

```java

public class ExampleRestClient {
  private static final String BASE_URL = "http://api.twitter.com/1/";

  private static MinionHttpClient client = new MinionHttpClient();

  public static void get(String url, RequestParams params, DespicableListener responseHandler) {
      client.get(getAbsoluteUrl(url), params, responseHandler);
  }

  public static void post(String url, RequestParams params, DespicableListener responseHandler) {
      client.post(getAbsoluteUrl(url), params, responseHandler);
  }

  private static String getAbsoluteUrl(String relativeUrl) {
      return BASE_URL + relativeUrl;
  }
}
```


Adding GET/POST Parameters with RequestParams
---------------------------------------------
The RequestParams class is used to add optional _GET_ or _POST_ parameters to your requests. _RequestParams_ can be built and constructed in various ways:

Create empty _RequestParams_ and immediately add some parameters:
```java
RequestParams params = new RequestParams();
params.put("key", "value");
params.put("more", "data");
```

Create RequestParams for a single parameter:

```java
RequestParams params = new RequestParams("single", "value");
```
Create RequestParams from an existing Map of key/value strings: (under construction)
```java
~~ HashMap<String, String> paramMap = new HashMap<String, String>(); ~~
~~ paramMap.put("key", "value"); ~~
~~ RequestParams params = new RequestParams(paramMap); ~~
``
See the [RequestParams Javadoc][2] for more information.


Uploading Files with RequestParams (under construction)
-------------------------------------------------------
The _RequestParams_ class additionally supports multipart file uploads as follows:

Add an _InputStream_ to the _RequestParams_ to upload:

```java
InputStream myInputStream = blah;
RequestParams params = new RequestParams();
params.put("secret_passwords", myInputStream, "passwords.txt");
```

add a _File_ object to the _RequestParams_ to upload:
```java
File myFile = new File("/path/to/file.png");
RequestParams params = new RequestParams();
try {
    params.put("profile_picture", myFile);
} catch(FileNotFoundException e) {}
```
Add a byte array to the _RequestParams_ to upload:
```java
byte[] myByteArray = blah;
RequestParams params = new RequestParams();
params.put("soundtrack", new ByteArrayInputStream(myByteArray), "she-wolf.mp3");
```
See the [RequestParams Javadoc][3] for more information.

(...Documentation under construction...)



Developed By
------------

* César Díez Sánchez - <cesaryomismo@gmail.com>

<a href="https://twitter.com/menorking">
  <img alt="Follow me on Twitter" src="http://imageshack.us/a/img812/3923/smallth.png" />
</a>
<a href="https://plus.google.com/115273462230054581675">
  <img alt="Follow me on Google Plus" src="http://imageshack.us/a/img203/4712/smallg.png" />
</a>
<a href="http://www.linkedin.com/in/cesardiezsanchez">
  <img alt="Add me to Linkedin" src="http://imageshack.us/a/img41/7877/smallld.png" />
</a>


License
-------

```
Copyright 2013 m3n0R

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```


[1] http://loopj.com/android-async-http/
[2] http://square.github.io/okhttp/
[3] http://loopj.com/android-async-http/doc/com/loopj/android/http/RequestParams.html