# Waydroid Destiny Rising Setup

Tweaks and configuration to get Destiny Rising running on Linux via Waydroid.

I am currently playing Destiny Rising exclusively through Waydroid on my Ryzen 9 5900X / Radeon RX 7800 XT, and the experience is excellent.

Below are the exact steps I followed to make it work reliably.

---

## 1. Install and Configure Waydroid (Android 13 + GAPPS)

Follow the official Waydroid installation guide here:  
[https://docs.waydro.id/](https://docs.waydro.id/)

When installing, choose the **Android 13 GAPPS** build so that Google Play Services are included.  
After the initial setup, make sure your emulated Android device is properly **signed in with a Google account** so that you can download Destiny Rising from the Play Store.

Ensure that Waydroid boots correctly and has working internet access before proceeding.

---

## 2. Install libndk and widevine

Follow the instructions from the official `waydroid_script` repository to install **libndk** and **widevine**:  
[https://github.com/casualsnek/waydroid_script](https://github.com/casualsnek/waydroid_script)

Select:
- Android 13
- libndk
- widevine

---

## 3. Install the fixed NDK bridge (libndk_fixer.so)

Copy `libndk_fixer.so` from this repository to:

```
/var/lib/waydroid/overlay/system/lib64/
```

This replaces the default translation bridge and gives much better performance on AMD GPUs.

---

## 4. Update your Waydroid configuration

Overwrite your existing config with the provided one from this repo, or merge the relevant lines if you already have custom tweaks.

The config file is located at:

```
/var/lib/waydroid/waydroid.cfg
```

The critical entries are:

```ini
[properties]
ro.hardware.vulkan = amdgpu
ro.dalvik.vm.native.bridge = libndk_fixer.so
```

(My configuration also spoofs a Pixel 7 to improve compatibility; you can keep that or choose a different device fingerprint.)

After editing, apply the changes:

```bash
sudo waydroid upgrade --offline
```

---

## 5. Install and run Destiny Rising

Download and install the game through the Play Store or via APK.

On first launch, it may crash once — just reopen it, and it should run fine afterward.

---

## Notes

- Mouse sensitivity feels low by default; I raised it to around 45 in-game for comfortable aiming.
- Performance is excellent on AMD GPUs when using `libndk_fixer.so`.
- Ensure your Vulkan driver is set to `amdgpu` — `radeon` will cause visual corruption.

---

## Summary

Key steps:

- Follow the official Waydroid installation guide for Android 13 GAPPS and sign in with your Google account.
- Install `libndk` and `widevine` using `waydroid_script`.
- Replace `libndk_translation.so` with `libndk_fixer.so`.
- Set the following in `waydroid.cfg`:

```ini
ro.hardware.vulkan = amdgpu
ro.dalvik.vm.native.bridge = libndk_fixer.so
```

- Run `sudo waydroid upgrade --offline` to bake in the configuration.

Then install Destiny Rising and enjoy native-like performance inside Waydroid on Linux.

