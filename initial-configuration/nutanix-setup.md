# Nutanix setup

**Note**: Nutanix environments require vProtect Node to be installed in one of the VMs residing on the Nutanix cluster. vProtect should detect automatically the VM with vProtect during index operation.

When adding Nutanix HV managers \(Prism\) make sure to have URL like the following:

```text
https://PRISM_HOST:9440/api/nutanix/v3
```

Then index your HV manager and verify if hypervisors and VMs are detected.

