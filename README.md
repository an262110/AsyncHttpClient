#AsyncHttpClient by Callum Taylor

This is the new and improved version of AsyncHttpClient taken from X-Library. It was long due a re-write.

In this version it allows a more flexible usage of posting files, http entities and GZIP handling.

Read the full documentation [http://scruffyfox.github.com/AsyncHttpClient](http://scruffyfox.github.com/AsyncHttpClient)

#Downloading large files

In order to download large files, you will need to subclass `AsyncHttpResponseHandler` and override the `onPublishedDownloadProgress()` method to write directly to cache instead of appending to a `ByteArrayOutputStream` which is what the standard `BinaryResponseHandler` does. This is to stop OOM due to a over-sized output stream.

#Usage example

##Example GET

```java
	AsyncHttpClient client = new AsyncHttpClient("http://example.com");
	List<NameValuePair> params = new ArrayList<NameValuePair>();
	params.add(new BasicNameValuePair("key", "value"));
	
	List<Header> headers = new ArrayList<Header>();
	headers.add(new BasicHeader("1", "2"));
	
	client.get("api/v1/", params, headers, new JsonResponseHandler()
	{
		@Override public JsonElement onSuccess(byte[] response)
		{
			JsonElement result = super.onSuccess(response);
			return null;
		}
	});
```

##Example DELETE

```java
	AsyncHttpClient client = new AsyncHttpClient("http://example.com");
	List<NameValuePair> params = new ArrayList<NameValuePair>();
	params.add(new BasicNameValuePair("key", "value"));
	
	List<Header> headers = new ArrayList<Header>();
	headers.add(new BasicHeader("1", "2"));
	
	client.delete("api/v1/", params, headers, new JsonResponseHandler()
	{
		@Override public JsonElement onSuccess(byte[] response)
		{
			JsonElement result = super.onSuccess(response);
			return null;
		}
	});
```

##Example POST - Single Entity

```java
	AsyncHttpClient client = new AsyncHttpClient("http://example.com");
	List<NameValuePair> params = new ArrayList<NameValuePair>();
	params.add(new BasicNameValuePair("key", "value"));
	
	List<Header> headers = new ArrayList<Header>();
	headers.add(new BasicHeader("1", "2"));
	
	JsonEntity data = new JsonEntity("{\"key\":\"value\"}");
	GzippedEntity entity = new GzippedEntity(data);
	
	client.post("api/v1/", params, entity, headers, new JsonResponseHandler()
	{
		@Override public JsonElement onSuccess(byte[] response)
		{
			JsonElement result = super.onSuccess(response);
			return null;
		}
	});
```

##Example POST - Multiple Entity + file

```java
	AsyncHttpClient client = new AsyncHttpClient("http://example.com");
	List<NameValuePair> params = new ArrayList<NameValuePair>();
	params.add(new BasicNameValuePair("key", "value"));
	
	List<Header> headers = new ArrayList<Header>();
	headers.add(new BasicHeader("1", "2"));
	
	MultiPartEntity entity = new MultiPartEntity();
	FileEntity data1 = new FileEntity(new File("/IMG_6614.JPG"), "image/jpeg");
	JsonEntity data2 = new JsonEntity("{\"key\":\"value\"}");
	entity.addFilePart("image1.jpg", data1);
	entity.addPart("content1", data2);
	
	client.post("api/v1/", params, entity, headers, new JsonResponseHandler()
	{
		@Override public JsonElement onSuccess(byte[] response)
		{
			JsonElement result = super.onSuccess(response);
			return null;
		}
	});
```

##Example PUT

```java
	AsyncHttpClient client = new AsyncHttpClient("http://example.com");
	List<NameValuePair> params = new ArrayList<NameValuePair>();
	params.add(new BasicNameValuePair("key", "value"));
	
	List<Header> headers = new ArrayList<Header>();
	headers.add(new BasicHeader("1", "2"));
	
	JsonEntity data = new JsonEntity("{\"key\":\"value\"}");
	GzippedEntity entity = new GzippedEntity(data);
	
	client.post("api/v1/", params, entity, headers, new JsonResponseHandler()
	{
		@Override public JsonElement onSuccess(byte[] response)
		{
			JsonElement result = super.onSuccess(response);
			return null;
		}
	});
```

Because of the nature of REST, GET and DELETE requests behave in the same
way, POST and PUT requests also behave in the same way.