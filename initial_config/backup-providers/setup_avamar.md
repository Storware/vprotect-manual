# Dell EMC Avamar setup

To enable Avamar support please do the following on the vProtect Node:

1. Install **avtar** - [https://www.dellemc.com/en-us/collaterals/unauth/technical-guides-support-information/products/data-protection/docu91839.pdf](https://www.dellemc.com/en-us/collaterals/unauth/technical-guides-support-information/products/data-protection/docu91839.pdf) - page 16
2. Use `avregister` command to register backup client
3. Install **mccli** - [https://www.dellemc.com/en-us/collaterals/unauth/quick-reference-guides/products/data-protection/docu91838.pdf](https://www.dellemc.com/en-us/collaterals/unauth/quick-reference-guides/products/data-protection/docu91838.pdf) - page 18
4. Use `avsetup_mccli` to setup client
   * **When choosing JRE - do NOT use** `/usr/java/latest` but `/usr/java/jre1.8...`
5. Now create backup destination using Web UI and provide parameters described [here](../../admin_webui_overview/admin_webui_bd.md#dell-emc-avamar).

