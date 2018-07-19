# vProtect Virtual Machine deployment on RHV/oVirt

Unpack downloaded tar pack:

```text
   [root@storage temp]# tar -xvf vProtect-RHV.tar.gz
   vProtect-RHV/
   vProtect-RHV/master/
   vProtect-RHV/master/vms/
   vProtect-RHV/master/vms/70dd2959-f827-42f9-a40c-1e384cb37084/
   vProtect-RHV/master/vms/70dd2959-f827-42f9-a40c-1e384cb37084/70dd2959-f827-42f9-a40c-1e384cb37084.ovf
   vProtect-RHV/images/
   vProtect-RHV/images/27afaa2a-079a-4fba-8e88-5f05c752d039/
   vProtect-RHV/images/27afaa2a-079a-4fba-8e88-5f05c752d039/c4d596fb-a4d2-4cb9-bbb5-e8e29c4e9519
   vProtect-RHV/images/27afaa2a-079a-4fba-8e88-5f05c752d039/c4d596fb-a4d2-4cb9-bbb5-e8e29c4e9519.meta
   vProtect-RHV/images/a425fde0-bd64-4ef9-ba13-71372c84e56c/
   vProtect-RHV/images/a425fde0-bd64-4ef9-ba13-71372c84e56c/a8bccf24-dfc6-4b4f-b916-836a7be7a66a
   vProtect-RHV/images/a425fde0-bd64-4ef9-ba13-71372c84e56c/a8bccf24-dfc6-4b4f-b916-836a7be7a66a.meta
```

Optionaly change owner for the downloaded files

```text
   chown user_name:group_name vProtect-RHV/ -R
```

Copy pack to your export domain:

```text
   cp -rf temp/vProtect-RHV/* 6abe3ee7-b174-4cc3-953a-af89c6e8b82c/
```

Log in to RHV/oVirt, go to "Storage", and export domain, tab VM Import. Select vProtect image and import it. ![](../.gitbook/assets/images_rhv_01.png)

Choose Cluster, and deploy it. ![](../.gitbook/assets/images_rhv_02.png)

After import image to enviroinment set IP addresation, run nmtui &gt; "Edit a connection". Select network interface, and edit it network settings.

