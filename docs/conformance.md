# QBO CNCF Conformance

This portion provides guidance on conducting CNCF [conformance](https://www.cncf.io/certification/software-conformance/) tests for the qbo application. The testing process utilizes a diagnostic tool named [sonobuoy](https://github.com/vmware-tanzu/sonobuoy).


|   Dependency	        |      Validated or Included Version(s)     | Notes 
|-|-|-|
|qbo|ce|
|kubernetes|v1.30.0-alpha.0.102_41890534532931 v1.29.0 v1.28.0 v1.27.3 v1.27.2 v1.27.1 v1.27.0 v1.26.6 v1.26.4 v1.26.3 v1.26.2 v1.26.0 v1.25.9 v1.25.8 v1.25.3 v1.25.11 v1.24.15 v1.24.13 v1.24.12 v1.23.17 v1.23.13 v1.22.17 v1.21.14 v1.20.15 v1.19.16|

### 1. Get Console Access
> To access the qbo web console, follow these steps:

Execute the following commands in your terminal
```bash
echo http://localhost:9601
```

Open your web browser and navigate to the address displayed in the output of the commands.

### 2. Run Conformance Tests
---
> The execution of conformance tests involves running the `conformance` script, assisted by a typing bot named `qbot` that automates command input. Alternatively, you have the option to manually enter the commands in the shell.

The `conformance` script is designed to execute the following actions:

* Creation of a `qbo` cluster
* Configuration of `kubectl`
* Retrieval and setup of `sonobuoy`
* Execution of conformance tests on the specified version.

Usage:

```bash
./conformance help
>>> ./conformance help                 -- Show usage
>>> ./conformance list                 -- List available Kubernetes image tags
>>> ./conformance run {tag}            -- Run CNCF conformance results for qbo
```

List available Kubernetes tags:

```bash
./conformance list
```
Select version and run conformance test. Example: 
```bash
./conformance run v1.28.0
```
### 3. Get Conformance Results
---
```bash
cat $HOME/sonobuoy/v1.28.0/qbo/e2e.log | grep Pass
```

## References

> [CERTIFIED KUBERNETES SOFTWARE CONFORMANCE](https://www.cncf.io/certification/software-conformance/)
