# AnyKernel3 Packaging

This repository provides `scripts/package_anykernel3.sh` to package a built
arm64 kernel image into an AnyKernel3 flashable zip.

## Local Packaging

Build the kernel first, then point `KERNEL_IMAGE` to the generated image:

```bash
KERNEL_IMAGE=/path/to/Image.lz4 scripts/package_anykernel3.sh
```

You can also point the script to a dist directory:

```bash
DIST_DIR=/path/to/dist scripts/package_anykernel3.sh
```

If neither `KERNEL_IMAGE` nor `DIST_DIR` is set, the script searches common
locations under `out/` and `bazel-bin/`.

The generated zip is written to:

```text
out/anykernel3-dist/
```

## GitHub Actions Packaging

Use the `Package AnyKernel3` workflow from the Actions tab. You can either:

- Provide `kernel_image_url` for a downloadable `Image`, `Image.gz`, or
  `Image.lz4`.
- Leave `kernel_image_url` empty and set `kernel_image_path` to an image that
  already exists in the checked out workspace.

The workflow uploads the flashable zip as the `anykernel3-flashable-zip`
artifact.

## Important Notes

- The generated AnyKernel3 zip flashes the active slot boot partition through
  AnyKernel3 with `is_slot_device=auto`.
- Back up `boot`, `init_boot`, `vendor_boot`, and `dtbo` for both slots before
  flashing.
- If your final release process needs additional modules or vendor ramdisk
  changes, extend `scripts/package_anykernel3.sh` before flashing.
