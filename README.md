# Real-Time Signal Denoising for Spectrum Unfolding ☢️⚡

## What is this?

This repo showcases one of the key features I developed for a nuclear radiation measurement company. The full contract included:

- ✅ **Live signal denoising**
- ✅ **Pile-up correction**
- ✅ **Improved background subtraction**

This repository focuses specifically on the **live signal denoising** component.

The main challenge was handling real-time streaming data at **1.8 million samples per second** — fast enough to overwhelm traditional post-processing methods. The solution uses **template matching** with FFT-based correlation to clean noisy signals on the fly, without mistaking high-frequency radiation events for noise.

Think of it like noise-canceling headphones… but for gamma/neutron detection systems.

---

## Why

Traditional denoising methods involve storing large amounts of raw data and processing it later. But at 1.8 MS/s, storage becomes a serious bottleneck — a 3-hour test produces a **49 GB file**.

This solution was built to:

- 🔄 **Run Live** — Denoising happens in real-time as data streams in.
- 💡 **Save Space** — By filtering and compressing early, we avoid storing unneeded noise.
- ✨ **Preserve Fidelity** — Instead of generic smoothing, we use pattern detection.

---

## Achievements 🚀

- ✅ Successfully handled **1.8 million samples per second** with minimal latency
- ✅ Significantly reduced stored data size

### Other contract tasks were also completed successfully

- ✅ Resolved pile-up artifacts (offline; live deployment was limited by RAM)
- ✅ Improved peak-to-background ratio using SNIP-based background subtraction

---

## How it Works

### 🔍 Template Matching with FFT

- Define what a valid signal “should” look like (in our case, a distinct preamp pulse shape)
- Reverse and conjugate that template
- Use `scipy.signal.fftconvolve` to perform fast cross-correlation with the incoming signal
- Events are detected where the matching score exceeds a chosen threshold

---

## Example

![Example](https://github.com/AyubSherif/SpectrumUnfoldingAlgorithm/blob/main/Example.png)

---


## Example of Actual Data

**Template signal:**

![Template](https://github.com/AyubSherif/SpectrumUnfoldingAlgorithm/blob/main/Template.png)

This method retains the important structure while removing random noise.

**Raw signal vs. Filtered output:**

![Filtered](https://github.com/AyubSherif/SpectrumUnfoldingAlgorithm/blob/main/Raw%20vs%20Filtered.png)

---

## Files

- `Template_Match.py`: Main implementation of the denoising algorithm using FFT-based template matching. Includes a simple demo at the bottom.
- (The rest of the spectrum unfolding system is proprietary and not included here.)

---

## How to Use It

This code isn’t a standalone app — it’s a plug-in module meant to integrate into your existing data acquisition and spectrum unfolding system.

Basic usage:

```python
from Template_Match import template_match

# Pass in your signal and reference template
corr, match_indices = template_match(signal, template, threshold=5)
