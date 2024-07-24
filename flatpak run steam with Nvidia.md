#flatpak #linux #nvidia

```bash
flatpak override --user --env=__NV_PRIME_RENDER_OFFLOAD=1 com.valvesoftware.Steam flatpak override --user --env=__GLX_VENDOR_LIBRARY_NAME=nvidia com.valvesoftware.Steam flatpak override --user --device=dri com.valvesoftware.Steam
```