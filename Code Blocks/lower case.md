# Lower Case

##### Takes input data in from one specified column and outputs just that column but all in lowercase

This example is designed around the SourceName field

```
def transform(inward_array):
    outward_array = []  

    for log in inward_array:
        for k, v in log.items():
            if k == 'SourceName':
                log[k] = v.lower()
                outward_array.append({'SourceName': v.lower()})

    return outward_array
```
