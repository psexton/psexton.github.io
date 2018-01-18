---
layout: post
title:  "Technote: Synology NAS Model Naming Scheme"
date:   2018-01-16
link-state: notebook
description: They're not indecipherable gibberish after all.
---
I recently purchased a Synology NAS. The model names initially appeared to be hugely unfriendly slugs like "DS1814+" and "RS3617xs".

After some time, I realized that they're much more easily understood once you know how those model names are built:

```
[Family][Max Drives][Refresh Cycle][Tier]
```

# The Breakdown

## Family

The Family is the broadest grouping, and generally a form factor:
 * DS<i>xxx</i> for the desktop DiskStation line
 * RS<i>xxx</i> for the rack-mounted RackStation line
 * etc.

## Max Drives

The Max Drives is the key distinguishing factor within a family:
 * A DS2<i>xx</i> has two bays.
 * A DS4<i>xx</i> has four bays.
 * A DS18<i>xx</i> has eight bays, but you can connect up to two 5-bay expansion units, for a total of eighteen.

## Refresh Cycle

Like most (all?) manufacturers, Synology periodically updates all of their products with better CPUs, more RAM, or some other improvement that doesn't change the Family or Max Drive characteristics. These generally happen at the same time across all products.

DS418<i>x</i> are from the "18" cycle. They replaced the DS416<i>x</i>, which replaced the DS414<i>x</i>, and so on.

## Tier

Synology also makes several "series" of drives that share characteristics. Think of them as tiers like basic, mid-level, premium.

DS218+ and DS218 are both desktop 2 bay NAS from the 18 lineup, but the former is from the "Plus" series and the latter is from the "Value" series.

# Sidenote

The expansion units can lead to some non-obvious results. For four bay desktop units, you may be looking at the DS418, DS418play, and DS918+. Upgrading to the "play" version gets you beefier specs for transcoding and streaming media. Upgrading to the "plus" version lets you use a 5 bay expansion unit. Which is why it is not called the DS418+ but the DS918+.
