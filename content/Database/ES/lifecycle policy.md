# lifecycle policy

## lifecycle phases

An index has five lifecycle phases: hot, warm, cold, frozen, delete.

- Hot: full operational mode, availabe for both read and write operations
- Warm: read-only, no indexing is allowed, allows frequent querying
- Cold: read-only, querying is expected to be infrequent and slow
- Frozen: read-only, querying is expected to be rare or very infrequent and sluggish
- Delete: final stage, the index is deleted premanently

## define lifecycle policy

```json
PUT _ilm/policy/hot_delete_policy
{
    "policy": {
        "phases": {
            "hot": {
                "min_age": "1d",
                "actions": {
                    "set_priority": {
                        "priority": 250
                    }
                }
            },
            "delete": {
                "actions": {
                    "delete": {  }
                }
            }
        }
    }
}
```

## policy scan interval

By default, policies are scanned every 10 minutes. To alter this scan period, we need to update the cluster settings using the `_cluster` endpoint.

We can reset the scan period by invoking the `_cluster/settings` endpoint with the appropriate period. For example, the following snippet resets the poll interval to 10 milliseconds:

```json
PUT _cluster/settings
{
    "persistent": {
        "indices.lifecycle.poll_interval": "10ms"
    }
}
```