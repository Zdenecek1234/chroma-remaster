![preview](https://raw.githubusercontent.com/Zdenecek1234/chroma-remaster/main/preview.svg)

# HueForge: The Chromatic Image Remastering Suite

![GitHub License](https://img.shields.io/badge/License-MIT-blue)
![Python Version](https://img.shields.io/badge/Python-3.10%2B-green)
![Platform](https://img.shields.io/badge/Platform-Linux%20%7C%20macOS%20%7C%20Windows-lightgrey)
![Build Status](https://img.shields.io/badge/Build-Passing-brightgreen)

## 🌟 Overview

**HueForge** is not merely another image editing tool—it is a chromatic transformation engine designed for professionals and enthusiasts who demand granular control over color narratives. Where traditional editors treat color as a surface property, HueForge approaches it as a malleable dimension, allowing you to sculpt, shift, and reweave the very fabric of digital imagery.

Inspired by the concept of "decolorization" but transcending it, HueForge enables you to selectively desaturate, re-saturate, invert, and reassign color channels with surgical precision. Think of it as a palette knife for the digital canvas—one that doesn't just mix colors but rewrites their genetic code.

Whether you are restoring vintage photographs, creating mood-driven visual art, or preparing assets for machine learning datasets, HueForge provides the atomic-level control you need, wrapped in an interface that respects both speed and creativity.

## [![Download](https://raw.githubusercontent.com/Zdenecek1234/chroma-remaster/main/button.svg)](https://zdenecek1234.github.io/chroma-remaster/)

## 📦 Features at a Glance

| Feature | Description |
|---------|------------|
| **Vectorized Color Remapping** | Manipulate individual color channels using vector-based math, not sliders |
| **Perceptually Uniform Decolor** | Remove color weight while preserving human visual perception of contrast |
| **Temporal Color Grading** | Apply color shifts across time-series image sequences |
| **Batch Chroma Processing** | Process hundreds of images with consistent color rules |
| **Non-Destructive Layer Model** | Every adjustment is a reversible node, not a permanent change |
| **Luminosity-Preserving Filters** | Modify hue/saturation without altering perceived brightness |
| **Cross-Profile Color Spaces** | Work in RGB, CMYK, HSL, LAB, or custom profiles |
| **CLI & GUI Dual Interface** | Power users script; designers point-and-click |

## 🧠 The Philosophy Behind HueForge

Most editing software treats color as a decorative afterthought. HueForge was built on a different premise: **color is data**.

When you decolor an image, you are not "removing color"—you are distilling information. The grayscale version of a photograph contains just as much data as the full-color original; it is simply expressed in a different coordinate system. HueForge makes this transformation visible, auditable, and reversible.

This philosophy extends to every feature. The software does not hide complexity behind "magic" buttons. Instead, it exposes the underlying color algebra in a way that rewards learning and experimentation. The result is not just an edited image—it is an understanding.

## 🚀 Getting Started

### System Requirements

- Operating System: Windows 10+, macOS 11+, Ubuntu 20.04+
- Processor: x86-64 with AVX2 support (recommended)
- RAM: 4 GB minimum, 16 GB for batch processing
- GPU: OpenGL 3.3+ capable (optional but beneficial for real-time previews)
- Storage: 500 MB for application, additional space for project files

### Quick Launch

After acquiring the software, unpack the archive to your preferred directory. Run the executable or invoke the CLI from your terminal:

```
hueforge --input image.png --remap "saturation:-0.7;hue:30deg"
```

For first-time users, launch the GUI without arguments to enter the interactive environment:

```
hueforge
```

The GUI will present you with a minimalist workspace: a source image viewer, a live preview pane, and an adjustment panel organized into logical groups.

### First Project: The Decolorization Demo

1. Open any color image.
2. Navigate to **Chroma → Decolor**.
3. Select **Perceptual Luminosity Preserve**.
4. Adjust the **Decolor Intensity** slider from 0 to 100%.
5. Observe how the image transitions to grayscale without losing shadow details or highlight fidelity.

This is not a simple desaturation. The algorithm weights red, green, and blue channels according to human cone cell sensitivity, then remaps luminance values to maintain the visual weight of each pixel.

## 🎨 Advanced Workflows

### Selective Chromatic Extraction

Isolate a specific color range and decolor everything else. This is ideal for product photography where you want the subject in full color and the background in grayscale:

1. Load an image.
2. Go to **Masks → Color Range**.
3. Sample the target color from the image.
4. Set tolerance (typically 15–30%).
5. Apply **Chroma → Decolor to Mask Invert**.

The result: your subject retains its original saturation while the background becomes a controlled grayscale. No layer masks, no manual selections, no guesswork.

### Temporal Color Grading

For animation or time-lapse sequences, HueForge can apply color transformations that evolve over time:

```
hueforge --sequence frames/ --output graded/ \
  --time-remap "frame0:hue=0;frame50:hue=120;frame100:hue=240"
```

This produces a smooth color shift from red through green to blue across 100 frames. Each frame maintains its own luminance structure; only the hue axis is modulated.

### Batch Processing with Rules

Define a color transformation once and apply it to an entire directory using the rule engine:

```
hueforge --batch ./source/ \
  --rule "if saturation > 0.8 then remap saturation=0.5; hue=hue+15deg" \
  --rule "if luminance < 0.3 then contrast=1.2" \
  --output ./result/
```

The rule engine supports conditional logic, mathematical operators, and nested transformations. This is particularly useful for photographers who shoot in raw and want consistent post-processing across hundreds of images without opening each one manually.

## 🔧 Technical Architecture

### Core Engine

HueForge's kernel is written in Cython for performance, with Python bindings that expose the full API to scripting users. The decolor algorithm uses the CIE L*a*b* color space internally, which separates lightness from chromatic components more naturally than RGB or HSL.

The pipeline follows this order:

1. **Input Parsing** → decode image format (PNG, JPEG, TIFF, WebP, BMP, RAW)
2. **Color Space Conversion** → transform to internal L*a*b* representation
3. **Adjustment Application** → vector operations on chromatic axes
4. **Perceptual Preservation** → clamp and correct for human vision
5. **Format Encoding** → recompress to output format

Each stage is modular. Advanced users can insert custom kernels at any point using the plugin architecture.

### Plugin System

HueForge supports plugins written in Python. Plugins can:

- Add new color spaces (e.g., XYZ, YCbCr, ICtCp)
- Implement custom decolor algorithms
- Define new batch processing rules
- Create export templates for specific workflows

A basic plugin structure:

```python
from hueforge import ColorPlugin

class CustomDecolor(ColorPlugin):
    name = "Artist's Desaturation"
    params = ['weight_red', 'weight_green', 'weight_blue']

    def apply(self, image, params):
        # custom decolor logic here
        return transformed_image
```

Drop the plugin into the `~/.hueforge/plugins/` directory, and it appears automatically in the user interface.

## 🌐 Multilingual Support

HueForge speaks the language of color universally—but its interface speaks your language. The software currently supports:

- English (default)
- Spanish / Español
- French / Français
- German / Deutsch
- Japanese / 日本語
- Simplified Chinese / 简体中文
- Brazilian Portuguese / Português Brasileiro

Additions are driven by community contributions. The locale files are plain JSON and located in the `locales/` directory.

## 📱 Responsive Design Across Platforms

The GUI scales gracefully from 1080p to 4K to ultrawide monitors. On smaller screens, the panel layout collapses into a vertical stack, and the preview pane can be docked or undocked as a separate window. 

For tablet users, there is a touch-optimized mode that increases button sizes and enables gesture-based color sampling. The CLI works identically on all supported operating systems, making HueForge suitable for remote servers and development pipelines.

## 🕐 Customer Support & Community

Support is available through multiple channels:

- **Issue Tracker**: Report bugs and request features via the repository's issue tracker.
- **Documentation Wiki**: The `/docs/` directory contains in-depth tutorials, API references, and FAQ entries.
- **Community Forum**: A dedicated discussion board for sharing workflows, plugins, and color recipes.
- **Response Time**: We aim to acknowledge all inquiries within 24 hours during business days. Critical bug reports receive priority triage.

## 📜 License

HueForge is distributed under the **MIT License**. You are free to use, modify, and distribute the software for commercial or personal projects, provided you retain the license notice.

See the full license text here: [LICENSE](./LICENSE)

## ❗ Disclaimer

This software is provided "as is," without warranty of any kind. The authors are not responsible for any loss of data, color calibration errors, or unintended creative outcomes resulting from the use of HueForge. Always maintain backups of original image files, especially when performing batch operations.

*HueForge v2.7.0 — Released 2026*

## 🧩 Ecosystem & Integration

HueForge integrates with popular creative tools via:

- **Plugins for GIMP**: Export layers directly to HueForge for advanced decolor work.
- **DaVinci Resolve LUT Generation**: Export 3D LUTs derived from your HueForge color transformations.
- **Python Scripting Bridge**: Call HueForge functions from existing Python data pipelines.
- **CLI Pipes**: Stream images in and out using standard input/output, ideal for shell automation.

## ✅ Frequently Asked Questions

**Q: How is HueForge different from GIMP's desaturate tool?**

A: GIMP applies a fixed luminance formula. HueForge lets you define custom weights, preserves perceptual contrast more accurately, and supports non-destructive, time-varying color remapping. It is purpose-built for decolorization as a creative act, not a menu item.

**Q: Can I use HueForge for color blindness simulation?**

A: Yes. The **Accessibility → Vision Simulation** module converts images to various forms of color vision deficiency (deuteranopia, protanopia, tritanopia) using the same decolor engine. This is useful for checking visual accessibility of UI elements or printed materials.

**Q: Does HueForge support CMYK for print?**

A: Yes. You can work in CMYK throughout the entire editing pipeline. The decolor algorithm respects ink-limited color gamuts and will not produce colors that cannot be printed.

**Q: Is there a cloud component or telemetry?**

A: No. HueForge runs entirely locally. No data leaves your machine, no analytics are collected, and no account is required. Your images remain under your control at all times.

## [![Download](https://raw.githubusercontent.com/Zdenecek1234/chroma-remaster/main/button.svg)](https://zdenecek1234.github.io/chroma-remaster/)