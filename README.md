# PHONE HACKING SEASON 2 - Episode 1 THE NFC DATA WIPE
## Authors: Jonathan Scott @jonathandata1  & Darcom
### CURRENT VERSION 2.1


![Android Data Wipe](https://i.postimg.cc/HkKkwhpm/Untitled-design-Max-Quality-2022-01-08-T191450-161.jpg)

## About Jonathan Scott

> My name is Jonathan Scott, and I'm an American Security Researcher. I am currently a computer science PhD student at North Central University. My research focus is mobile spyware. 

## Background: 

> ### I made this app with a friend and former co-worker "Darcom."  It is based off of the Google Android's Device Admin Sample
> ### Source: https://android.googlesource.com/platform/development/+/master/samples/ApiDemos/src/com/example/android/apis/app/DeviceAdminSample.java

### The device admin sample is a demo that allows you to do the following. 

	1. Allow the installed sample to be a device administrator
    2. Authorize the data wipe
    3. Double authorize the data wipe

![Android Data Wipe](https://i.postimg.cc/8cFt2xg4/Screen-Shot-2022-01-09-at-5-03-32-AM.png)

![Android Data Wipe](https://i.postimg.cc/0jLcPZ8s/Screen-Shot-2022-01-09-at-5-15-47-AM.png)

![Android Data Wipe](https://i.postimg.cc/1RTWGRDj/Screen-Shot-2022-01-09-at-5-16-00-AM.png)


### The reason that there is a 3 step process is because data wiping is a really serious thing. If someone were to data wipe your phone, tablet, car, smart watch, drone, or other android based devices without authorization, the impact of that could immeasurable. 

### We were tasked to create software that would data wipe all android devices, and minimize the need for human interaction. 

### As Darcom and I planned this out we thought, if we can install an app elevate the privileges, and on launch wipe out the device we should be able to accomplish this. 

### My tasks were as follows:
1.  Elevate privileges for the data wipe app programmatically - Bypass Step 1 mentioned Above
2.  Launch the app programmatically - Bypass Step 2 Mentioned Above 

### Darcom's tasks were as follows:
1. Strip the sample down to bare bones, and hiding all the button functionality
2. Compile the app so that only Step 3 Mentioned Above is needed. 

## The Expected Result 

> Once the app would be launched programmatically all of the steps referenced above would be bypassed, and this should work on every android mobile device in the world.

## Acceptable Failure
 
> If for any reason the app failed to launch when attached to a host via USB with ADB enabled, tapping on the app alone will data wipe the device without any other need for authorization.

## Taking The Data Wiping Further

>Years later after we had created this app, I was no longer working and hacking with Darcom, but I realized I could take this further, so I created a PoC for wiping out androids that had NFC enabled, and it was rather simple. Details to perform this down below. 

## Known Vulnerabilities That Derived From This Application

### Launching this app on a device below the minimum power needed
#### We noticed that on some devices even though we were able to launch the app and trigger a factory reset, if the device did not have sufficient power to actually perform the reset. We were working for a company that was involved in reverse logistics, and this was a huge issue. The software we created was functional, but when a supply chain worker saw the device "reset" they would unplug the device and send the device down the production line. The device looked as though it was powered off which was part of the process we had programmatically implemented as well, but when you charged the device and booted the device backup, the data wiping app was still visible in the applications section, and if you were to tap on it, it would still wipe out the device. 

#### This means there was a high potential for customers who received refurbished devices to have this app installed on it, and you can imagine what could happen next. 

# Installing

1. Enable ADB on your android
2. Install the apk in this repo 
`adb install jv_darcom.apk`
3. Enable device admin
`adb shell dpm set-device-owner com.Darcom.device.admin/.DeviceAdminDemo`
4. Launch
`adb shell am start -a android.intent.action.MAIN -n com.Darcom.device.admin/com.Darcom.device.admin.MainActivity`

## Using NFC. 
1. Do steps 1-3 Above
2. Write to an NFC tag with the following 
`com.Darcom.device.admin`
3. Tap the tag to the device
4. Done

# Considerations

### 1. If the device has a user account for androids 8.0 and higher, the user account should be removed first or enabling device admin programmatically may not work. Launching the app manually will prompt for device admin authorization, and once device admin has been granted, data wipe will start, there is no warning, it just begins. 

### 2. This is for sure more of a supply chain attack, but this can also be used by apps that have device admin privileges. 

### 3. Data wiping should never be instant even if the device admin has set permissions, there needs to be more checks and balances. 

## Public Recommendations. 

### Check the permissions of all of your apps. Uninstall any app that allows for data wiping and device admin unless you really trust it. 