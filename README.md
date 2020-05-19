# Smart Location - Fetch Location Background and Foreground

# Use Case
1. To fetch location with very less code into your fragment/activity
2. To fetch location in background using foreground notification in app

Download
--------

Grab via Maven:
```xml
<dependency>
  <groupId>com.hb.smartlocation</groupId>
  <artifactId>smartlocation</artifactId>
  <version>1.0</version>
  <type>pom</type>
</dependency>
```
or Gradle:
```groovy
implementation 'com.hb.smartlocation:smartlocation:1.0'
```

# How it works?

**Create instance using context**

```java
smartLocation = SmartLocation.getInstance(this) 
```

**Ask location permission**
```java
smartLocation!!.requestLocationPermission(this, object : SmartLocation.OnLocationPermission {
                    override fun onGranted() {
                        // here you've location permission and can request location updates using below method
                    }
                })
```

**override activity/fragment permission result and provide result**
```java
override fun onRequestPermissionsResult(requestCode: Int, permissions: Array<String>, grantResults: IntArray) {
        super.onRequestPermissionsResult(requestCode, permissions, grantResults)
        smartLocation?.onRequestPermissionsResult(requestCode, permissions, grantResults)
    }
```
**(Method 1) Start location updates using (work untill activity opened)**
```java
smartLocation!!.bindToLifeCycle(this@..Activity) // For bind location to activity
smartLocation!!.getLastKnownLocation(onLocationUpdate) // For fetching last known location (optional)
smartLocation!!.setOnLocationRequestSetting(object : SmartLocation.OnLocationSetting { // For location request
            override fun onLocationSetting(locationSettingRequest: LocationSettingsRequest) {
                smartLocation!!.askForLocationSettingDialog(locationSettingRequest, this@...Activity)
            }
        })
        // Start Location update using configuration
smartLocation!!.startLocationUpdate(SmartLocation.Configuration.Builder().apply {
            setFastestIntervalSeconds(3)
            setUpdateIntervalSeconds(10)
        }.build(), onLocationUpdate)
```

```java
private val onLocationUpdate = object : SmartLocation.OnLocationUpdate {
        override fun onLocationUpdate(location: Location) {
            // Here you've latest location
        }
    }
```

**(Method 2) start location update using foreground service (work even after closing app)**

```java
smartLocation!!.setOnLocationRequestSetting(object : SmartLocation.OnLocationSetting {
            override fun onLocationSetting(locationSettingRequest: LocationSettingsRequest) {
                smartLocation!!.askForLocationSettingDialog(locationSettingRequest, this@LocationSampleActivity)
            }
        })

smartLocation!!.requestLocationService(SmartLocation.Configuration.Builder().apply {
            setFastestIntervalSeconds(5)
            setUpdateIntervalSeconds(15)
            setNotificationMessage("{App Name} is Fetching location")
            setNotificationTitle("{App Name}")
            setNotificationIcon(R.drawable.ic_flash)
        }.build())

LocalBroadcastManager.getInstance(this@LocationSampleActivity).registerReceiver(onLocationBroadcast,IntentFilter(SmartLocationService.ACTION_BROADCAST))        
```

 ```java
 private val onLocationBroadcast = object : BroadcastReceiver() {
        override fun onReceive(context: Context?, intent: Intent?) {
            val location = intent!!.getParcelableExtra<Location>(SmartLocationService.EXTRA_LOCATION)
            // Here you've latest location
        }
}
```

<h2 id="social">Social Media :fire:</h2>

If you like this library, please tell others about it :two_hearts: :two_hearts:

(https://github.com/mkrupal09/PinLView/blob/master/twitter_icon.png)
(https://github.com/mkrupal09/PinLView/blob/master/facebook_icon.png)

[![Share on Twitter](https://github.com/mkrupal09/PinLView/blob/master/twitter_icon.png)]
[![Share on Facebook](https://github.com/mkrupal09/PinLView/blob/master/facebook_icon.png)]

# License

```
Copyright 2017 HiddenBrains

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```
    
