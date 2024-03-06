# Most recent reporting-in status

##### This script is to be used for deduping an imput of devices where devices are consistantly reporting in with a status. 
It finds the most recent reported in status per device and disgards older irrelevent data.

Originally this was used for an Intune Compliance check

DQL Query:

``` stream=COMPLIANCE | duration 30d |  groupby DeviceName, @lastSyncDateTime, actiontaken, cnamtime | last 20000 ```

```
def transform(inward_array):
    unique_DeviceName = set()
    DeviceName_last_sync_mapping = {}

    for event in inward_array:
        current_DeviceName = event["DeviceName"]
        current_last_sync = event["lastSyncDateTime"]

        if current_DeviceName not in unique_DeviceName or current_last_sync < DeviceName_last_sync_mapping[current_DeviceName]:
            DeviceName_last_sync_mapping[current_DeviceName] = current_last_sync

        unique_DeviceName.add(current_DeviceName)

    outward_array = []
    for DeviceName in unique_DeviceName:
        latest_event = None
        for event in inward_array:
            if event["DeviceName"] == DeviceName and event["lastSyncDateTime"] == DeviceName_last_sync_mapping[DeviceName]:
                latest_event = event
                break

        temp = {
            "DeviceName": DeviceName,
            "lastSyncDateTime": latest_event["lastSyncDateTime"],
            "actiontaken": latest_event["actiontaken"],
            "cnamtime": latest_event["cnamtime"]
        }
        outward_array.append(temp)

    return outward_array

```
