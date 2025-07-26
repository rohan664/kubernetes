# requests	limits
Purpose	Minimum guaranteed resources	Maximum allowed resources
Effect	Scheduler uses this for placement	Kills container if exceeded
Oversubscription	Sum of requests ≤ node capacity	Sum of limits can > node capacity

## Units Explained

### CPU:

    1 CPU core = 1000m (millicores)

    Examples:

    500m = 0.5 CPU core

    1.5 = 1.5 CPU cores (equals 1500m)

### Memory:

Base units: bytes (accepts binary prefixes)

Common units:

    - 1 KB (Kilobyte) = 1000 bytes
    - 1 KiB (Kibibyte) = 1024 bytes
    - 1 MB = 1000² bytes
    - 1 MiB = 1024² bytes
    - 1 GB = 1000³ bytes
    - 1 GiB = 1024³ bytes

Example Configuration
yaml
```
resources:
  requests:
    cpu: "250m"    # 0.25 CPU guaranteed
    memory: "512Mi" # 512 Mebibytes guaranteed
  limits:
    cpu: "1"       # Max 1 full CPU
    memory: "2Gi"   # Max 2 Gibibytes
```

Key Rules

- CPU:
    Can be overcommitted (container throttled if limit reached).Measured in absolute amounts (not percentages)

- Memory:Hard limit - container gets OOM-killed if exceeded.Always specify memory in Mi/Gi (not MB/GB) to avoid confusion

## Best Practices:

Always set requests ≤ limits

Start with:

    CPU: 100m-500m requests, 1-2 limits

    Memory: 128Mi-1Gi requests, 1-4Gi limits