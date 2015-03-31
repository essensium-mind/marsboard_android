Android 4.4.3 for Embest MarS Board
====================================

This repository contains the device configuration tree to build Android 4.4.3
for the Embest MarS Board.  It is a merge of [Embest's 4.3 tree][embest] into
Freescale's 4.4.3 tree.

The kernel is based on [Freescale's Android kernel][fslkernel] with a device
tree created from [Embest's legacy board-mx6q\_marsboard.c][embestboard].

[embest]: https://github.com/embest-tech/android_device_fsl/tree/embest_imx_jb4.3_1.0.0-ga
[fslkernel]: http://git.freescale.com/git/cgit.cgi/imx/linux-2.6-imx.git/tag/?id=kk4.4.3_2.0.0-ga
[embestboard]: https://github.com/embest-tech/linux-imx/blob/embest_jb4.3_1.0.0-ga/arch/arm/mach-mx6/board-mx6q_marsboard.c


Prerequisites
--------------

* The [repo][] tool
* A [build environment for AOSP][aosp]

[repo]: http://source.android.com/source/downloading.html#installing-repo
[aosp]: http://source.android.com/source/initializing.html


Getting the source
-------------------

First, fetch the Android 4.4.3 source code from the Android Open Source Project
(AOSP):

    repo init -u https://android.googlesource.com/platform/manifest -b android-4.4.3_r1
    repo sync

Next, download [Freescale's patches][fslbsp] (login required) and apply them:

    source /path/to/freescale_source/code/and_patches.sh
    c_patch /path/to/freescale_source/code imx

[fslbsp]: https://www.freescale.com/webapp/Download?colCode=IMX6_KK443_200_ANDROID_SOURCEBSP&appType=license&location=null&fpsp=1&WT_TYPE=Board%20Support%20Packages&WT_VENDOR=FREESCALE&WT_FILE_FORMAT=gz&WT_ASSET=Downloads&fileExt=.gz

Finally, replace Freescale's device configuration with this repository and fetch
kernel and bootloader:

    rm -rf device/fsl
    git clone --single-branch -b kk4.4.3_2.0.0_marsboard https://github.com/essensium-mind/marsboard_android.git device/fsl
    git clone --single-branch -b kk4.4.3_2.0.0-ga_marsboard https://github.com/essensium-mind/marsboard_linux.git kernel_imx
    git clone --single-branch -b 2015.01_marsboard https://github.com/essensium-mind/marsboard_uboot.git bootable/bootloader/uboot-imx


Building
---------

To build the complete Android stack, run the following commands:

    source build/envsetup.sh
    lunch marsboard_6q-eng
    make -j4

The output should be available at `out/target/product/marsboard_6q`.  To put the
images on an SD card, use the `fsl-sdcard-partition.sh` tool from the
`common/tools` directory.
