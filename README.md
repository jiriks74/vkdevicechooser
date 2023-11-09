# Vulkan Device Chooser Layer

This is a quick and dirty implementation of a Vulkan layer to force a specific
physical device to be used. This is useful for Vulkan games which do not provide
an option to choose the device themselves.

## Requirements

Compiling requires `vulkan/vulkan.h`, `vulkan/vk_layer.h` and `vulkan/utility/vk_dispatch_table.h`.
You will also need a C++ compiler and `meson`

- On `Arch Linux`

```bash
sudo pacman -S vulkan-header vulkan-utility-libraries meson gcc g++
```

- On `OpenSUSE Tumbleweed`

```bash
sudo zypper search vulkan-devel vulkan-utility-libraries-devel meson gcc gcc-c++
```

- On `Fedora`, `Debian`, `Ubuntu` and other distributions with older packages

  - Until packages for the [Vulkan libraries](https://github.com/KhronosGroup/Vulkan-Utility-Libraries)
  are updated you need to use the [original project](https://github.com/aejsmith/vkdevicechooser)

## Build and install with

```bash
meson builddir --prefix=/usr
meson compile -C builddir
sudo meson install -C builddir
```

This will install to the system's Vulkan layer directory, `/usr/share/vulkan/implicit_layer.d/`.

To run a Vulkan application forcing a specific device to be used,
launch it with these environment variables:

```bash
ENABLE_DEVICE_CHOOSER_LAYER=1 VULKAN_DEVICE_INDEX=<device index>
```

Replace `<device index>` with the "GPU id" for the desired device as reported by
`vulkaninfo` (without the layer enabled).

For example:

```bash
ENABLE_DEVICE_CHOOSER_LAYER=1 VULKAN_DEVICE_INDEX=1 vulkaninfo
```

should give info for the device which had GPU id 1 when running `vulkaninfo`
without the environment variable set.

The layer can be used with Steam games by setting their launch options to:

```bash
ENABLE_DEVICE_CHOOSER_LAYER=1 VULKAN_DEVICE_INDEX=<device index> %command%
```
