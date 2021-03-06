# Description
A java asynchronous http client based on HttpURLConnection.

# Getting Started

# Installation (Java)
* Click [here](https://bintray.com/eukaprotech/maven/download_file?file_path=com%2Feukaprotech%2Fasyncconnection%2Fasyncconnection%2F1.0.1%2Fasyncconnection-1.0.1.jar "Version 1.0.1 Jar file") to download the jar file. 
* Add the jar file to your project's build path

OR

If your java project is a gradle project:
Add the dependency in build.gradle

```compile 'com.eukaprotech.asyncconnection:asyncconnection:1.0.1'```


# Installation (Android)
It is recommended that you use the android customized library. Click [here](https://github.com/eukaprotech/networking/blob/master/README.md) for the android library review. 

To use this java library for android:

Add the dependency in build.gradle (App module)

```compile 'com.eukaprotech.asyncconnection:asyncconnection:1.0.1'```

Add permission in manifest file

```<uses-permission android:name="android.permission.INTERNET" />```


# Usage

# Request Methods covered:

* GET
* POST
* PUT
* DELETE
* HEAD
* OPTIONS


GET request:
     
``` js
AsyncConnection asyncConnection = new AsyncConnection();
asyncConnection.get("url", new AsyncConnectionHandler() { 
   @Override
   public void onStart() {
      //you can choose to show a progress dialog here
   }

   @Override
   public void onSucceed(int responseCode, HashMap<String, String> headers, byte[] response) {
       //consume the success response here
   }

   @Override
   public void onFail(int responseCode, HashMap<String, String> headers, byte[] response, Exception error) {
       //consume the fail response here
   }

   @Override
   public void onComplete() {
      //you can dismiss the progress dialog here
   }
});
```

Sample GET request:
      
``` js
AsyncConnection asyncConnection = new AsyncConnection();
asyncConnection.get("https://www.google.com", new AsyncConnectionHandler() { 
    @Override
    public void onStart() {
       //you can choose to show a progress dialog here
    }

    @Override
    public void onSucceed(int responseCode, HashMap<String, String> headers, byte[] response) {
        //consume the success response here
    }

    @Override
    public void onFail(int responseCode, HashMap<String, String> headers, byte[] response, Exception error) {
        //consume the fail response here
    }

    @Override
    public void onComplete() {
       //you can dismiss the progress dialog here
    }
});
```
   
# Query Parameters

To attach query parameters to a url:

``` js
Parameters query_parameters = new Parameters();
query_parameters.put("key1", "value1");
query_parameters.put("key2", "value2");
       
String url = URLBuilder.build("initial_url", query_parameters);
//if the initial_url already contains query parameters, the new query parameters are just added at the end of existing ones.
//in case of conflicting keys between the existing query parameters and the new ones; the new ones are given priority.
//you can then use the resulting url in a request
```
       
Sample:
      
``` js
Parameters query_parameters = new Parameters();
query_parameters.put("q", "discover");
String url = URLBuilder.build("https://www.google.com/search", query_parameters);
//will result into https://www.google.com/search?q=discover
//you can then use the resulting url in a request
       
AsyncConnection asyncConnection = new AsyncConnection();
asyncConnection.get(url, new AsyncConnectionHandler() {
    // the implemented listener methods onStart, onSucceed, onFail & onComplete
});
```
        
        
# Body Parameters (Used for POST & PUT)

To attach parameters as the body/content of the request
      
``` js
Parameters parameters = new Parameters();
parameters.put("key1", "value1");
parameters.put("key2", "value2");
        
//For a POST request
AsyncConnection asyncConnection = new AsyncConnection();
asyncConnection.post("url", parameters, new AsyncConnectionHandler() {  
    // the implemented listener methods 
});
        
//For a PUT request
AsyncConnection asyncConnection = new AsyncConnection();
asyncConnection.put("url", parameters, new AsyncConnectionHandler() {  
    // the implemented listener methods
});
```
             
# Request Headers

To attach request headers:
 
``` js
HashMap<String, String> headers = new HashMap<>();
headers.put("header_key1", "header_value1");
headers.put("header_key2", "header_value2"); 
            
//For a POST request 
Parameters parameters = new Parameters();
parameters.put("key1", "value1");
parameters.put("key2", "value2");
AsyncConnection asyncConnection = new AsyncConnection();
asyncConnection.post("url", headers, parameters, new AsyncConnectionHandler() { 
      // the implemented methods 
});
            
//For a GET request 
AsyncConnection asyncConnection = new AsyncConnection();
asyncConnection.get("url", headers, new AsyncConnectionHandler() { 
      // the implemented listener methods 
});
//Other request methods have similar way of attaching request headers
```
        
# Body JSONObject (Used for POST & PUT)

JSONObject can be used in place of Parameters as the body/content of the request:

``` js
JSONObject jsonObject = new JSONObject();
try{
    jsonObject.put("key1", "value1");
    jsonObject.put("key2", "value2");
}catch (Exception ex){}
        
HashMap<String, String> headers = new HashMap<>();
headers.put("Content-Type", "application/json");    //header for JSONObject being the body/content
        
//For a POST request
AsyncConnection asyncConnection = new AsyncConnection();
asyncConnection.post("url", headers, jsonObject.toString(), new AsyncConnectionHandler() {  
    // the implemented listener methods 
});
        
//For a PUT request
AsyncConnection asyncConnection = new AsyncConnection();
asyncConnection.put("url", headers, jsonObject.toString(), new AsyncConnectionHandler() {  
    // the implemented listener methods 
});
```
        
# Basic Authentication

To attach a basic auth in the request headers:
 
 ``` js
HashMap<String, String> headers = new HashMap<>();
String username = "example@gmail.com"; 
String password = "123456789";
String auth_value = username+":"+password;

String basicAuth = "basic "+ Base64.encodeToString(auth_value.getBytes(), Base64.NO_WRAP); //IF ANDROID
OR
String basicAuth = "basic "+ DatatypeConverter.printBase64Binary(auth_value.getBytes());   //IF JAVA

headers.put("Authorization", basicAuth);
```
 
# Uploading Files
 
To upload files, include them in the body parameters
    
``` js
File file = new File("file path");
Parameters parameters = new Parameters();
try {
    parameters.put("key1", file);
} catch (IOException e) {
            
}
parameters.put("key2", "value2");
```
        
# Redirects

By default AsyncConnection does not follow redirects to different protocols such as HTTP to HTTPS or HTTPS to HTTP.
To enable protocol shift redirects:

``` js
AsyncConnection asyncConnection = new AsyncConnection();
asyncConnection.setFollowProtocolShiftRedirects(true);
```
 
# NOTE
 
To consume the byte array response as a String:
     
``` js
String responseBody = new String(response); //OR
String responseBody = new String(response, "UTF-8");
```
 
 
<a href='https://bintray.com/eukaprotech/maven/AsyncConnection?source=watch' alt='Get automatic notifications about new "networking" versions'><img src='https://www.bintray.com/docs/images/bintray_badge_color.png'></a>

[ ![Download](https://api.bintray.com/packages/eukaprotech/maven/AsyncConnection/images/download.svg?version=1.0.1) ](https://bintray.com/eukaprotech/maven/AsyncConnection/1.0.1/link)
