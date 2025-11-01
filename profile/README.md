# Hyprside

Windows 10 is coming to an end, Windows 11 is an absolute mess, Linux distributions are not a replacement for most users, and macOS is too expensive. Mobile operating systems like iOS and Android have evolved tremendously in usability, stability, ease of management, integrations, and ecosystems — but desktop operating systems have stagnated.

**Hyprside** is a Linux-based operating system designed to be the true successor to traditional distributions and even Windows itself.
The focus is on delivering **stability, performance, and a cohesive, elegant user experience**, free from the fragmentation and fragility of existing systems.

> The name *Hyprside* comes from the idea of being *“the other side.”*

**WARNING: The project is still under development, this readme doesn't describe the current state of the project, but rather what I want to create**


---

## ✨ Main Goals

* UX/UI on par with Apple — the best design and visual experience in the Linux ecosystem.
* Consistent and uniform experience across the entire system.
* Modern design, smooth animations, polished transitions.
* Intuitive interface, free of bugs.
* **Visual and behavioral consistency** across all applications, regardless of toolkit.
* **GUI-first**: all essential operations are possible without the terminal (the terminal exists only as a development tool and is disabled by default).
* Transparent integration with Windows and Android applications.
* Btrfs snapshots and rollback without relying on external tools.

**Hyprside** is not marketed as “just another Linux distribution,” but rather as an **independent system**, similar to Android or ChromeOS.

---

## 🖥️ Core Components

### 1. tibs — Tiago’s Incredible Boot Screen (Display Manager & Boot Handler)

* Standalone, without Wayland/X11.
* Technical stack: **Clay + Skia + OpenGL** (same as HyprUI).
* Handles login and lock screen.
* **Replaces Plymouth completely**: manages the boot splash and experience.
* Tracks boot progress by checking **systemd services**.
* Already mostly implemented, needs to be adapted to work with the hyprside architecture

### 2. HyprDE (Desktop Environment)

* Based on **Hyprland**, fully integrated with HyprUI and universal themes.
* UX/UI on Apple’s level, with smooth transitions and polished usability.
* **Windows Subsystem**: Wine/Proton integration, extended support for problematic apps (Adobe Suite, MS Office).
* **Android Subsystem**: seamless WayDroid integration.
* **Advanced notifications**: Android-style, interactive and persistent.
* **Intelligent clipboard**: history, editing, and sharing.
* **Unified drawers**: sudo, polkit, file picker, screen share, etc.
* **Universal file picker**: injected across all toolkits.

### 3. App Store

* Custom backend (no PackageKit), fast and resilient.
* Integration with flatpak and the package manager in developer mode 
* Integration with the Windows subsystem, allowing for Windows applications in the same store

### 4. Settings App

* Full system management via GUI (replaces terminal).
* Multi-GPU management, permissions, theming, animations, screen recording, etc.
* Export/import system for replicating setups across devices.
* **Hyprtheme**: a single TOML file inspired by Tailwind, applied across GTK, Qt, HyprUI, etc.

---

## 🎨 UI / Toolkit Architecture

### HyprUI

* Custom toolkit written in **Rust**.
* Based on **Clay + Skia**, with a declarative, React-inspired API.
* Unified API for native apps and Wayland shells.

### Hyprtheme

* Universal TOML format for theming.
* Resolves GTK/Qt fragmentation.
* Ensures visual and behavioral consistency across the system.

---

## 🔀 HyprSessionManager (HSM)

* Compositors (Hyprland, tibs) render to **off-screen framebuffers** and send them to the HSM.
* The HSM decides which buffer appears on which display, applies transformations, and performs final composition to DRM/KMS.
* **Two rendering modes**:

  * *Pass-through (zero-copy)*: ideal for gaming (direct scanout).
  * *Hybrid composition*: for animations, overlays, and multiple sessions.
* Controls who can receive keyboard and mouse inputs
* Handles privileged shortcuts (Win+L, Ctrl+Alt+Del)
* Handles animations and transitions between screens

```

tibs           (🔒) ┅┅┅> session manager ━> DRM (screen)
user1 hyprland (✅) ━━━━━━━━━┫
user2 hyprland (🔒) ┅┅┅┅┅┅┅┅┅┛
```

---

## 📋 Additional Features

* GPU-based screen recording integrated into the shell.
* Debounce for repeated permission requests.
* Unified file picker (replaces native GTK/Qt pickers).

---

## 🔐 Philosophy

* Simple, elegant, cohesive, and performant.
* Total consistency between apps, UI, and system behavior.
* GUI-first: the terminal is never mandatory.
* Security and design as central pillars.

---

## 📦 System Architecture

Hyprside is based on the concept of an **immutable ROM**, inspired by Android and ChromeOS:

* The system is distributed as a **read-only image (**`.img`**)**.
* Only `/home` and specific directories are mutable.
* Atomic updates: download new image → replace → reboot.
* No risk of breaking the system as with `apt upgrade` or `pacman -Syu`.
* You can treat your computer like an appliance
### Software Management

* Apps via **Flatpak**, with looser restrictions.
* Developer mode enables a full package manager.
* Custom images possible (for self-hosting or custom builds).

### User Profiles

* **Normal user:** access only to `/home`.
* **Administrator:** system-wide apps and configurations.
* **root/SYSTEM:** reserved for critical system operations.

### Advantages

* **Stability:** base system never corrupted.
* **Reproducibility:** setups easily replicable across machines.
* **Simplicity:** formatting is trivial (just clear mutable subvolumes).
* **Portability:** design makes it easier to port Hyprside to Android smartphones later.
