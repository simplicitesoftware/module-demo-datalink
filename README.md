<!--
 ___ _            _ _    _ _    __
/ __(_)_ __  _ __| (_)__(_) |_ /_/
\__ \ | '  \| '_ \ | / _| |  _/ -_)
|___/_|_|_|_| .__/_|_\__|_|\__\___|
            |_| 
-->
![](https://docs.simplicite.io//logos/logo250.png)
* * *

`DemoDataLink` module definition
================================

[![Quality Gate Status](https://sonarcloud.io/api/project_badges/measure?project=simplicite-modules-DemoProject&metric=alert_status)](https://sonarcloud.io/dashboard?id=simplicite-modules-DemoProject)

### Introduction

This is an addon to demo **order management** application to explain the **DataLink** behavior:
- Between 2 hosts = 2 instances of the platform
- The instances must have the Demo installed, or at least the 2 object definitions: `DemoSupplier` and `DemoProduct`
- The DetaLink synchronises those objects between a master and a slave host
	- Slave is notified on the fly when the master updates those objetcs thru synchronous API calls
	- A cron job will make a resync from the last scan/timestamp if the slave is not available

### Import

To import this DataLink demo:

- First install the module `Demo` in 2 isolated instances (V5.3+)
- Create for each a module named `DemoDataLink`
- Set the settings as:

```json
{
	"type": "git",
	"origin": {
		"uri": "https://github.com/simplicitesoftware/module-demo-datalink.git"
	}
}
```

- Click on the _Import module_ button

### Configure

Open the DataLink from the menu (operation or settings) to change the Hosts URL/credentials

- Host name:
	- same as the `SERVER_URL` or `DIRECT_URL` to be accessed from a public URL by API
	- or a logical name forced on startup with `setApplicationName("myAppName")` in the PlatformHooks (needs to restart the server)

```java
public class PlatformHooks extends com.simplicite.util.engine.PlatformHooksInterface {
	public String postPlatformInit(Grant sys) {
		Globals.setApplicationName("SLAVE_NAME" /* or "MASTER_NAME" */);
		return null;
	}
}
```

- URL : the public URL to application (ex `https://myapp.domain.com`)
- User/Password: 
	- API credentials to get a token from slave instance
	- The user must have full access to `DemoSupplier` and `DemoProduct` (designer is granted)

Make sure the Crontab contains the `DataLink` job and activate it to desired times.

Make sure `USE_API` services is available.

Clear the platforms caches.

### Test

- Create, update or delete Suppliers/Products on the master with admin profile
- Check on the salve host that updates are applied automatically to local data
- See server logs on both sides to audit issues

### Quality

This module can be analysed by the **SonarQube** analysis tool
using this command:

```bash
mvn verify sonar:sonar
```

