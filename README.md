# earthquake-explorer
# Earthquake Explorer

Exploratory analysis and spatial clustering of global earthquake activity, using
the USGS live 24-hour feed.

A short, self-contained notebook: pull every earthquake recorded worldwide in the
last day, map it, look at how depth relates to magnitude, and use K-Means to group
events into geographic zones.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/hobokelamwakajumba/earthquake-explorer/blob/main/earthquake_explorer.ipynb)

---

## Data

USGS Earthquake Hazards Program — real-time CSV feed:

```
https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/all_day.csv
```

Every seismic event detected globally in the preceding 24 hours: time, latitude,
longitude, depth, magnitude, magnitude type, location description, and station
metadata (22 columns).

**This is a rolling feed, not a fixed dataset.** Run the notebook today and you
get today's earthquakes. The outputs saved in the notebook are a snapshot from
**8 June 2026**, containing **268 events**. Numbers below refer to that snapshot;
your run will differ.

No API key, no download, no local data files.

---

## What the notebook does

1. **Load** the live feed directly into pandas.
2. **Clean** — drop events missing latitude, longitude, or magnitude.
3. **Map** — plot every event on an interactive Folium world map, marker radius
   scaled by magnitude, click for location and magnitude.
4. **Rank** — list the ten strongest events of the period.
5. **Relate** — scatter depth against magnitude and compute their correlation.
6. **Count** — break events down by region.
7. **Cluster** — K-Means (k=5) on latitude/longitude to group events into
   geographic zones, then plot the result coloured by cluster.

---

## Findings from the 8 June 2026 snapshot

**The day was dominated by one sequence.** The largest event was **M7.8, 26 km SW
of Kablalan, Philippines**, at 55 km depth — followed by M6.5, M6.0, M6.0 and
several M5.5+ events in the same region. Seven of the ten strongest events
worldwide that day were Philippine.

**Where the detections came from:**

| Region | Events |
|---|---:|
| California | 87 |
| Alaska | 67 |
| Philippines | 37 |
| Texas | 19 |
| Hawaii | 11 |

**Depth vs. magnitude:** Pearson correlation of **r = 0.37** — a weak-to-moderate
positive relationship in this sample. Deeper events skewed larger, but the
relationship is loose, and this figure is heavily influenced by the Philippine
sequence, which was both deep and strong.

**K-Means (k=5)** split the events into zones of 135, 65, 44, 12 and 12 events.

<!-- TODO: write two or three sentences of geological interpretation here.
     This is the part only you can write, and it's what makes this notebook
     worth more than a generic tutorial. Worth addressing:
     - Why California and Alaska dominate the *count* while the Philippines
       dominates the *magnitude* — network density vs. actual seismic release.
     - Why depth and magnitude correlate at all: subduction-zone geometry.
     - What the clusters correspond to tectonically, if anything. -->

---

## Limitations

Stated plainly, because they matter for how the results should be read:

- **A 24-hour window is not a sample of global seismicity.** It is one day,
  usually shaped by whatever sequence happened to occur. Conclusions here
  describe that day, not the Earth.
- **Event counts reflect detection networks, not seismic activity.** California
  and Alaska top the table because they are densely instrumented, not because
  they are the most seismically active places on Earth. A magnitude recorded in
  California would go undetected across much of the ocean floor.
- **K-Means on raw latitude/longitude is geometrically wrong.** Euclidean
  distance on degrees distorts with latitude (a degree of longitude shortens
  toward the poles) and does not wrap at the antimeridian. The clusters are
  convenient spatial groupings, not tectonic provinces.
- **Aftershocks are not independent events.** Treating a mainshock and its
  aftershock sequence as separate observations inflates correlations.
- **k=5 was chosen arbitrarily**, not by elbow method, silhouette score, or any
  tectonic reasoning.

---

## Running it

Easiest: click the Colab badge above — everything is preinstalled there.

Locally:

```bash
pip install pandas folium scikit-learn matplotlib
jupyter notebook earthquake_explorer.ipynb
```

Run cells top to bottom. Requires an internet connection, since the data is
fetched live.

---

## Possible next steps

Not yet done — listed so it's clear where this stops:

- Pull the 30-day or archive feed for a sample large enough to say something
  about seismicity rather than about one day.
- Project coordinates properly, or cluster with DBSCAN using haversine distance,
  which handles geographic data correctly and doesn't require choosing k.
- Choose k by silhouette score rather than by hand.
- Add a genuine supervised task — for example, predicting whether an event is
  shallow or deep from its other properties — which would make a
  "classification" label accurate.
- Compare cluster boundaries against a plate-boundary dataset.

---

## Author

Hobokela Mwakajumba — geologist (BSc Geology & Geothermal Resources), working at
the intersection of geoscience and data analysis.
[hobokelamwakajumba.com](https://hobokelamwakajumba.com)
