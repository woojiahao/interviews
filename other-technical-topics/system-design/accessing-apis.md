---
description: >-
  I have noticed an increase in questions where you are required to access an
  API. The following is a quick primer on how API access works in Python
---

# Accessing APIs

{% hint style="info" %}
This section focuses on accessing APIs in Python, if you are not using Python, please refer to the respective documentation for your language to learn how to access APIs
{% endhint %}

To make an API request, use the `requests` library, which you need to import first:

```python
import requests
```

Then, to make a `GET` request, use `requests.get(URL)`. This returns a `Response` object that you can use to extract information from the API:

```python
resp = requests.get('fake URL')
resp.status # status of the request
resp.json() # body of response as JSON, access it as you would an array/dictionary
```

To upload data via a `POST` request, use `requests.post(URL, json=JSON)`

```python
body = {'foo': 'bar'}
requests.post('fake URL', json=body) # this sends the JSON to the URL
```
