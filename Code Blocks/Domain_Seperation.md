# Delimate and Seperate

#### This script will take in a column and output everything after the ``` @ ``` where 'user' = an email

Ex. foo@bar.com > bar.com

## With Delimination and only outputing the new column
```
def transform(inward_array):
    outward_array = []  # New list to store extracted domain values

    for log in inward_array:
        for k, v in log.items():
            if k == 'user':
                # Extract the domain (everything after the @ symbol)
                domain = v.split('@')[-1]
                
                # Update the email in the original log to the extracted domain
                log['user'] = domain

                # Append the extracted domain to the outward_array
                outward_array.append({'Domain': domain})
```

test
## With regex and outputing the original result and the domain seperatly

```
def transform(inward_array):
   for log in inward_array:
       for k,v in list(log.items()):
           log["domain"] =  regexp_extract("\S+\@(\S+)", log["user"],1)
   return inward_array
```
