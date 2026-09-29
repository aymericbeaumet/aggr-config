---
title: Tcl/Tk 9.1 Released
link: https://www.tcl-lang.org/software/tcltk/9.1.html
source: hnrss-org
published: 2026-09-29T17:13:38Z
updated: 2026-09-29T17:13:38Z
first_seen: 2026-09-29T19:40:04.433827810Z
authors:
- dmux
content: extracted
html: 2026-09-29-tcl-tk-9-1-released.html
preview:
  file: 2026-09-29-tcl-tk-9-1-released.preview-8561b2e01f8d.webp
  width: 256
  height: 128
  alt: Tcl Home
  color: '#222518'
images:
- source: https://www.tcl-lang.org/images/logo.png
  original:
    file: 2026-09-29-tcl-tk-9-1-released.image-f4c924753ef7.png
    width: 2084
    height: 1042
  variants:
  - file: 2026-09-29-tcl-tk-9-1-released.image-8149070af791.webp
    width: 320
    height: 160
  - file: 2026-09-29-tcl-tk-9-1-released.image-a7096265381e.webp
    width: 640
    height: 320
  - file: 2026-09-29-tcl-tk-9-1-released.image-17e2e0ad4b0d.webp
    width: 960
    height: 480
  - file: 2026-09-29-tcl-tk-9-1-released.image-8e8db839f163.webp
    width: 1280
    height: 640
  - file: 2026-09-29-tcl-tk-9-1-released.image-01c024360330.webp
    width: 1600
    height: 800
  - file: 2026-09-29-tcl-tk-9-1-released.image-a9f07654803a.webp
    width: 2084
    height: 1042
  color: '#676767'
- source: https://www.tcl-lang.org/images/Tcl9.png
  original:
    file: 2026-09-29-tcl-tk-9-1-released.image-f4c924753ef7.png
    width: 2084
    height: 1042
  variants:
  - file: 2026-09-29-tcl-tk-9-1-released.image-8149070af791.webp
    width: 320
    height: 160
  - file: 2026-09-29-tcl-tk-9-1-released.image-a7096265381e.webp
    width: 640
    height: 320
  - file: 2026-09-29-tcl-tk-9-1-released.image-17e2e0ad4b0d.webp
    width: 960
    height: 480
  - file: 2026-09-29-tcl-tk-9-1-released.image-8e8db839f163.webp
    width: 1280
    height: 640
  - file: 2026-09-29-tcl-tk-9-1-released.image-01c024360330.webp
    width: 1600
    height: 800
  - file: 2026-09-29-tcl-tk-9-1-released.image-a9f07654803a.webp
    width: 2084
    height: 1042
  color: '#676767'
---

![](https://www.tcl-lang.org/images/Tcl9.png) **Latest Release: Tcl/Tk 9.1.0 (Sep 29, 2026)**

Tcl/Tk 9.1.0 is the current development work on Tcl and Tk, aiming toward stable releases in September 2026. They add new features and interfaces to the foundation of Tcl/Tk 9.0.

[Download Tcl/Tk 9.1.0 Source Releases](https://www.tcl-lang.org/software/tcltk/download.html)

### Highlights of Tcl 9.1

- New command **unicode**: Unicode normalization
- New C routines **Tcl\_UtfToNormalized\***: Unicode normalization
- New command **timer**: monotonic clock; microsecond resolution
- New command **lfilter**: select items from list
- New command **interp set**: variable access in child interpreter
- New **subst** options: **-backslashes**, **-commands**, **-variables**.
- New **switch** options: **-integer**.
- Many C99 math routines now available as **expr** functions.
- Applications now required to call an initialization routine, either **Tcl\_FindExecutable** or **TclZipfs\_AppHook**.
- New C routine **Tcl\_IsEmpty**.
- New C routine **Tcl\_GetEncodingNameForUser**.
- New C routine **Tcl\_AttemptCreateHashEntry**.
- New C routines **Tcl\_ListObjRange**, **Tcl\_ListObjRepeat**, **Tcl\_ListObjReverse**.
- New C time API using **long long** in place of **Tcl\_Time**
- Case-insensitive filesystem paths on macOS.
- **auto\_execok** and **exec** search reform on Windows.
- Revised searches for script library and encodings.
- Improved list internals for memory efficiency of large lists.
- Extended support for 64-bit sizes.

### Highlights of Tk 9.1

- Accessibility screen reader support.
- Initial support for bidirectional text / RTL languages.
- New widget **ttk::toggleswitch**.
- New command **tk attribtable**.
- **send** command revised and improved on Aqua.
- Handling of negative screen distances.
- Extended states in **ttk::treeview** and **ttk::notebook**
- Improvements to **Tk\_CanvasTextInfo**
- Rotated text on labels
- Limit message box and dialogs to physical screen width.
- Improved listbox selection colors.
- Removed obsolete support for Windows XP appearances.
