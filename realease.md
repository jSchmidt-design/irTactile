# Release Notes — Version 0.12.0-alpha.1 

Version **0.12.0** is now available as a **pre-release**.

This update introduces several major user-facing improvements along with even more under-the-hood changes. Some workflows have changed significantly, so please review the notes below carefully before upgrading.

---

## Highlights

- **Multi-Device Support**
- **Improved Channel Mapping**
- **New Profile Structure in the Editor**
- **Filter Templates**
- **Level Meters**
- **Improved Acceleration Suspension Signal**
- **New Filters**
- **Several Improvements**
- **Multiple Bug Fixes**

> ⚠️ The first three items represent fundamental workflow changes and require special attention.

---

## Multi-Device Support

Multi-device support introduces a new approach to buffer configuration.

- Existing buffer setups will not work as before.
- Please refer to the **`readme.pdf`** for detailed setup instructions and examples.

---

## Channel Mapping Improvements

The channel mapping system has been redesigned and now requires **manual configuration** of input and output channels.

### Key Changes

- Automated input mapping (`input_1` … `input_18`) has been removed.
- All input streams are now **fully customizable**.

### New Input Mapping Workflow

1. Create an **input stream** within a profile.
2. Link the input stream to a **physical input channel**.
3. Once linked, the input stream can be used in the mixer just like the previous input system.

---

## New Profile Structure

The editor now uses a new profile-based structure designed to simplify organization and future profile sharing.  
This structure is conceptually similar to **SimHub**.

### Profile Organization

- You can create **multiple profiles**.
- Each profile can contain multiple **collections**.
- Each collection groups related streams:
  - Inputs
  - Mixes
  - Generators
  - Filter Templates

Profiles and collections are organized in a **tree structure**.

---

## Editor Usage Notes

- **Create new elements**:  
  Open the context menu on the parent element.
- **Delete elements**:  
  Right-click the element you want to delete.
- **Open a mixer collection**:  
  Click the *Mixer* section or any stream within it.
- **Edit a generator**:  
  Click directly on the specific generator stream.
- **Templates and Inputs**:  
  These do not have a main view and are only visible in the tree.
- **Drag and Drop**:
  The tree supports drag and drop to move elements (only mixer streams for now)  

---

## Compatibility Notice

> ❗ **Old profiles are not compatible with version 0.12.0**

- All existing profiles must be **recreated from scratch**.
- My own profile is still a work in progress and is **not included** in this release.

---

## Recommendation

Due to the scope of changes, it is **not yet recommended** to fully switch to this pre-release.  
However, feel free to try it out and verify whether the application works correctly for your setup.

Feedback is welcome and will help stabilize future releases.