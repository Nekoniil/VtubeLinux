# How to Vtube on Linux
I didn't have much to do, so i tried to vtube on linux

## Pre-everything
*im currently using fedora 45 on kernel 7.3.3*

and the setup sucks ass

---

## 1. Camera
download DroidCam (both android **AND** the linux client): https://droidcam.app/#apps

connect and then check if linux actually sees a camera:
```
v4l2-ctl --list-devices
```
(`sudo dnf install v4l-utils` if you don't have the tool installed)

remember the `/dev/videoX` number of the DroidCam device, you'll need it later

> if ya can't see the virtual camera or `sudo modprobe v4l2loopback` return stuff like:
> `Key was rejected by service` — The **Secure Boot** is at fault and is blocking the kernel module
> just disable the Secure Boot in your bios and then do that commands again (if its still not working idk ask ai or something)

## 2. OpenSeeFace
grab it from: https://github.com/emilianavt/OpenSeeFace/releases

i reccomend doing the stuff down (uv > pip, imo)
use uv (`sudo dnf install uv`), then:
```bash
cd OpenSeeFace-v1.20.5
uv sync
```

> my config
>
> `uv run python facetracker.py -c 1 -M -v 4 -P 1 --model 4 -W 1920 -H 1080 -F 45`

the breakdown:
- `-c 1` -> camera index (check it to `v4l2-ctl --list-devices`)
- `-M` -> mirror input (natural movement)
- `-v 4` -> visualize tracking with point numbers
- `-P 1` -> show 3D fitting points
- `--model 4` -> wink-optimized model (better blink detection than the others)
- `-W 1920 -H 1080 -F 45` -> 1080p @ 45fps (match what your camera actually outputs)

just dont close the terminal otherwise openseeface is gonna die

## 3. OpenVT
download this very cool software: https://erodozer.itch.io/openvt

Why did i choose this one?, well for first it has **no watermark** (looking at you, VTube Studio),
it can load regular `.moc3` l2d models and it was made for linux soooo why not
*(and also because i couldnt find models for inoichi2d)*

throw in your model, then in the tracking just click `enable` for **OpenSeeFace** as the tracker,
the model now should be movin

free models:
- https://booth.pm/en/search/Live2d?max_price=0
- https://vtuber-nook.com/category/live2d-3d-pngtuber-models/

(or make your own, good luck with that)

## 4. Making it work in OBS
1. in OBS add a **Window Capture** of the **OpenVT** window
2. right click the source -> Filters -> **Color Key** → pick OpenVT's background color →
   tweak similarity until it's transparent
3. **Start Virtual Camera** in OBS
4. in Discord/whatever, pick **OBS Virtual Camera** as your webcam


## Troubleshooting (aka everything that went wrong for me)

| error | fix |
|---|---|
| `modprobe: Key was rejected by service` | Secure Boot is blocking v4l2loopback. disable it in BIOS |
| `ImportError: cannot enable executable stack as shared object requires` | `sudo dnf install prelink` then `execstack -c .venv/lib/python*/site-packages/onnxruntime/capi/onnxruntime_pybind11_state*.so` |
| `can't open camera by index` | something else is holding the camera close it or maybe check the index again with `v4l2-ctl --list-devices` |
| model doesn't move | if everything seems to be running the model might just be ass for this, or there isn't enough lightning on your face |

# Enjoy
