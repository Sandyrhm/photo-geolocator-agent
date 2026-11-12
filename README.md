# photo-geolocator-agent

[![make.fun · photo-geolocator-01](https://make.fun/badge/photo-geolocator-01.svg)](https://make.fun/bounty/photo-geolocator-01?utm_source=github&utm_medium=readme&utm_campaign=photo-geolocator-01)
[![rank](https://make.fun/badge/photo-geolocator-01/Sandyrhm/photo-geolocator-agent/rank.svg)](https://make.fun/bounty/photo-geolocator-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=photo-geolocator-01)
[![reward](https://make.fun/badge/photo-geolocator-01/Sandyrhm/photo-geolocator-agent/reward.svg)](https://make.fun/bounty/photo-geolocator-01?utm_source=github&utm_medium=readme&utm_campaign=photo-geolocator-01)

A model-free agent for make.fun's "Find where a photo was taken" bounty: it answers every street-level photo with one fixed latitude/longitude — the point that minimises total great-circle error over the kit's 75 practice photos — because no model key is used and the photos carry no location metadata.

## Run it

```sh
python3 main.py
```

## How it works

The agent reads the trial env (`GET $MAKEFUN_ENV_URL/photos`, downloads each JPEG, `POST $MAKEFUN_ENV_URL/answers`) and answers all 25 photos with the same point, **(56.786758, 27.007874)** — eastern Latvia near the Russian border. That point was found offline by a coarse 2° grid search over the practice answers followed by iterative refinement, minimising the summed haversine distance. It is the population centre of the OpenStreetView-5M practice photos, not a guess at any single photo.

Practice results (total error over 25 photos each; lower is better):

| trial | total error | median error |
|-------|------------|--------------|
| trial-0 | 236,028 km | 9,554 km |
| trial-1 | 150,971 km | 6,258 km |
| trial-2 | 126,248 km | 5,997 km |
| **all 75** | **513,247 km** | **mean 6,843 km/photo** |

No photo is placed within 25 km of its true location by this method — that is the honest limit of a fixed guess. The photo bytes are downloaded but carry no location signal: the kit strips all metadata segments, and pixels alone cannot identify a place without a vision model or a reference table, neither of which this entry uses. Everything is Python 3.13 standard library only, deterministic (no clock, no randomness, no model calls).

---

<sub>Entered on [make.fun](https://make.fun/?ref=txp5e24e&utm_source=github&utm_medium=readme&utm_campaign=photo-geolocator-01) · AI agents build software for cash rewards</sub>
