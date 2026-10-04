---
title: The work by Valve's Timur Kristóf on improving old AMD GPUs on Linux
link: https://www.phoronix.com/news/XDC-2026-Valve-Timur-AMDGPU
source: hnrss-org
published: 2026-10-03T19:14:48Z
updated: 2026-10-03T19:14:48Z
first_seen: 2026-10-04T06:19:25.883170106Z
authors:
- speckx
content: extracted
html: 2026-10-03-the-work-by-valve-s-timur-kristof-on-improving-old-amd-gpus.html
preview:
  file: 2026-10-03-the-work-by-valve-s-timur-kristof-on-improving-old-amd-gpus.preview-6624fea86639.webp
  width: 256
  height: 104
  alt: Timur
  color: '#7f8992'
images:
- source: https://www.phoronix.net/image.php?id=2026&image=timur
  original:
    file: 2026-10-03-the-work-by-valve-s-timur-kristof-on-improving-old-amd-gpus.image-3938416401ee.webp
    width: 880
    height: 357
  color: '#fbfbfb'
- source: https://www.phoronix.com/assets/categories/radeon.webp
  original:
    file: 2026-10-03-the-work-by-valve-s-timur-kristof-on-improving-old-amd-gpus.image-2de04432556f.webp
    width: 500
    height: 500
  color: '#fdfdfd'
---

![RADEON](https://www.phoronix.com/assets/categories/radeon.webp)

Over the past year Timur Kristóf of Valve's Linux graphics driver team has made multiple very nice improvements to the AMDGPU kernel driver for enhancing support for old (GCN 1.0/1.1 era from a decade ago) graphics cards so that they can better handle Linux gaming and other tasks. This week in Toronto, Kristóf presented on this AMDGPU work that he initially began as a kernel driver development exercise after initially spending years in user-space focused on the Mesa 3D driver code.

Timur Kristóf has become a legend for those with old AMD GCN 1.0/1.1 graphics cards and APUs for transitioning them from the legacy Radeon driver over to the modern AMDGPU kernel driver, which unlocks being able to use the RADV Vulkan driver, better performance, and all-around better functionality than the legacy Radeon code. In the process he had to address defects in the AMDGPU display code for these legacy graphics cards along with various power management issues and more. Then he went on to improve these graphics cards further with soft reset support and other enhancements all to make these aging graphics cards more viable for Linux gaming use in 2026 and beyond.

For getting an idea of the impact of transitioning the old Radeon graphics cards to AMDGPU, see last year's [Linux 6.19's Significant ~30% Performance Boost For Old AMD Radeon GPUs](https://www.phoronix.com/review/linux-619-amdgpu-radeon). With AMD not devoting much resources to these aging graphics card driver improvements in years, Timur on Valve's Linux team really picked up the slack to make for a much better experience.

![Timur](https://www.phoronix.net/image.php?id=2026&image=timur)

For those wishing to hear him recount his efforts for improving AMD Radeon GCN 1.0/1.1 era graphics on Linux, embedded below is his XDC2026 presentation along with the [PDF slides](https://indico.freedesktop.org/event/12/contributions/545/attachments/380/545/xdc2026_kernel_dev_old_amd_gpus.pdf). The presentation also goes over his experiences in getting involved in AMDGPU kernel driver development and other tid-bits for those that may want to get involved with the open-source AMD Linux kernel driver development.

[YouTube video player](https://www.youtube.com/watch?v=j5W5ErEMnvM)
