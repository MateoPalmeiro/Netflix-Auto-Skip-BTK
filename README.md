# Netflix Auto Skip BTK V1.12

**Netflix Auto Skip BTK** was born out of pure frustration — skipping the same intros, recaps, and credit rolls on repeat for every single episode of whatever you’re watching. I actually got the idea while watching Dexter and its never‑ending intros, recaps, and credits; it got me thinking, “There has to be a faster way to get back into the action.” And being the masochistic programmer I am — who’d rather tinker than get paid or build something with actual commercial use — I embarked on this unhinged quest to automate the whole ordeal.

## Overview

This UserScript runs silently in the background, auto‑clicking Netflix’s skip‑intro button, recap overlays, credit skip, and even advancing to the next episode — all without a single manual click. It detects and fires the appropriate button the instant it appears, often before you even notice it on screen.

## Motivation

If you’ve ever banged out a multi‑hour binge session, you know the drill:

* **Blocky intros** that stretch five, ten, twenty seconds or more. They’re usually cool — but not when you’re two hours into a marathon and caught up in a top‑tier tense moment.
* **“Previously on…” recaps** that spoil nothing but still waste time. I swear I hate ’em. I can understand them for occasional viewers who forget what happened, but if you’re here, you’re not one of ’em.
* **End credits** that feel endless — let’s be real, no one cares about the cast list when the main character just got kidnapped and you need to know what happens next.

Dexter’s gritty world deserves your full attention, not button‑mashing. And Netflix’s React‑driven markup changes so often that most scripts break after a minor UI tweak. This one is designed for easy selector maintenance and minimal CPU overhead. Seriously, go watch Dexter if you haven’t yet.

Also worth mentioning when you’re eating with greasy hands and don’t want to stain your custom keyboard or your G502 mouse, or when your PC/laptop is hooked up to a TV and you’re lounging on the sofa — you don’t want to crawl across the room to hit “Next episode.” In both cases, I had to choke down all this skippable crap.

## How It Works

A short polling loop (every 300 ms — change it as you want) scans for known button selectors and simulates a click the moment a target element is rendered. You can’t even see it on screen because the script triggers it before it visually loads — but it **is** there:

```js
// Key selectors extracted from Netflix’s minified chaos
const selectors = [
  "[data-uia='next-episode-seamless-button']",
  "[data-uia='next-episode-seamless-button-draining']",
  ".watch-video--skip-content-button",
  ".watch-video--skip-preplay-button"
];

setInterval(() => {
  for (const sel of selectors) {
    const btn = document.querySelector(sel);
    if (btn && btn.offsetParent !== null) {
      btn.click();       // Trigger skip or next episode
      return;            // One click per cycle
    }
  }
}, 300);
```

1. **Selectors array**: Each CSS selector targets a specific Netflix UI element (skip intro, skip recap, advance episode).
2. **Polling interval**: 300 ms strikes a balance between responsiveness and CPU usage.
3. **Element detection**: `offsetParent !== null` ensures the element is rendered before clicking — oopsie woopsie from V1.0 that I fixed later on.

> **Note:** Netflix frequently updates its front‑end. If the script stops working, inspect the new `data-uia` attributes or class names and update the `selectors` array accordingly. Fork, branch, submit a pull request, or wait five minutes for me to update it — I’ll probably update it before you even notice Netflix made some crappy, senseless DOM changes.

## Installation

1. Install a UserScript manager of your choice:
   * [Violentmonkey](https://violentmonkey.github.io/)
   * [Tampermonkey](https://www.tampermonkey.net/)
   * [Greasemonkey](https://www.greasespot.net/)
2. Install directly from GreasyFork: [Netflix Auto Skip BTK v1.12](https://greasyfork.org/es/scripts/535353-netflix-auto-skip-btk-v1-12)
3. Create a new script in your manager and paste the contents of `Netflix Auto Skip BTK V1.12`.
4. Save and reload any Netflix tab (`http*://*.netflix.com/*`).

The script will automatically run on pages matching Netflix’s domain pattern.

## Configuration

* **Polling Interval:** Adjust the `300` ms value in the `setInterval` call if you encounter performance issues.
* **Selectors:** Add, remove, or modify entries in the `selectors` array to match Netflix’s updated UI.

## Compatibility & Limitations

* Works in all modern browsers supported by popular UserScript managers.
* Does not interfere with video playback; it only simulates button clicks when necessary.
* Requires manual selector updates if Netflix overhauls its front‑end architecture.

## Credits & License

* **Author:** Mateo Palmeiro
* **Namespace:** [https://github.com/MateoPalmeiro](https://github.com/MateoPalmeiro)
* **License:** All Rights Reserved

---

Built by a fellow dev who refuses to waste another second on unskippable content. Happy binging — go save your clicks!
