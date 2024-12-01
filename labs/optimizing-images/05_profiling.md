
## Profiling Containerized Applications

Application Profiling allows to characterize application behavior to enable code performance optimizations

Docker prevents the container from running profiling, by default.

We can modify this, by use of the ```--security-opt``` option, either as:
- ```--security-opt seccomp=unconfined```
- or ```-security-opt seccomp=profile.json``` with an example json config as below

#### Creating a seccom profile.json file

We first download the default seccomp settings used by the Moby project (the upstream Docker software)

```
wget https://raw.githubusercontent.com/moby/moby/master/profiles/seccomp/default.json
```

Towards the end of that file, you should see the following lines

```json
    {
      "names": [
        "perf_event_open"
      ],
      "action": "SCMP_ACT_ALLOW",
      "includes": {
        "caps": [
          "CAP_PERFMON"
        ]
      }
    }
```

This has the effect of restricting CAP_PERFMON use, we wish to disable that, so change those lines to

```json
    {
      "names": [
        "perf_event_open"
      ],
      "action": "SCMP_ACT_ALLOW"
      }
    }
```

Save this file as *profile.json*.

Then when running an application under Docker we would use the command

```docker run mypod --security-opt seccomp=profile.json ```

This would allow us to run profiling tools against this container.


