# vesida-agent

Part of [Project Vesida](https://github.com/project-vesida). AGPL-3.0. **Roadmap only: no code yet.**

The Vesida agent runs one optical array. It coordinates the array's OpTA nodes (or compatible
cameras that can write I-01 frames) and turns their observations into an unattended contribution
to the shared catalog.

## What it will do

Each night, on the array controller:

1. **Schedule**: compute twilight and the observable window for the site; gate on weather where a sensor exists.
2. **Capture**: drive the camera daemon that writes I-01 FITS frames with GNSS-disciplined `DATE-OBS` (interfaces I-01 and I-05 in [opta-engineering/SYSTEMS.md](https://github.com/project-vesida/opta-engineering/blob/main/SYSTEMS.md)).
3. **Reduce**: run [opta-pipeline](https://github.com/project-vesida/opta-pipeline) on the night's frames and export CCSDS TDM.
4. **Upload**: sign the TDM with the station key and push it to [vesida-platform](https://github.com/project-vesida/vesida-platform), with retry and local spooling when offline.
5. **Report**: heartbeat, disk and temperature health, nightly counts; accept over-the-air updates of the image.

The agent depends on opta-pipeline as a library and copies none of its code: the pipeline stays
Apache-2.0, the agent is AGPL-3.0.

## Why AGPL

A station image that is modified and run for a closed network must publish its changes. That is
the point of Vesida: the network cannot be taken closed.

## Roadmap

1. Define the array configuration and service boundaries.
2. Implement capture, scheduling, local spooling, and health reporting.
3. Integrate `opta-pipeline` and signed TDM upload.
4. Package a reproducible array-controller image with safe remote updates.

## Get involved

Open an issue on this repository for any of the five components above. The capture daemon and the
GNSS timestamp path are the first two things a contributor with a Pi and a camera can start on.
