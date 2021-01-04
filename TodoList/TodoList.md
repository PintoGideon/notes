1. Memoize the getCommand() and calculateRuntime()
functions in NodeDetails Component

2. 

```javascript
return base64.b64encode(
            zlib.compress(
                json.dumps(json_data).encode('utf-8')
            )
        ).decode('ascii')
```

```
This is @rpienaar’s code but easy to see that the dictionary is converted into a a json string then byte-encoded, then compressed with zlib and then  base 64 encoded
```