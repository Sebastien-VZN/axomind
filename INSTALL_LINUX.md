# Linux Installation - Axomind

## ⚠️ IMPORTANT WARNING

**Axomind runs on Debian 13 (Trixie) ONLY.**

**For other Linux distributions (Ubuntu, Fedora, Arch, openSUSE, etc.): NO SUPPORT will be provided.**

### Linux Support Policy

**You will only receive the compiled binary.** It's up to you to manage your Linux environment and install the necessary dependencies for your distribution.

### Why No Multi-Distro Support?

1. **Linux fragmentation**: Distros use different library versions, package managers, and paths.
2. **No Package Standardization**: Unlike Windows or macOS, there's no single standard for dependencies and distribution formats.
3. **Flatpak/Snap/AppImage Don't Meet Requirements**: Several days of testing revealed critical limitations: ~500 MB package size (vs 50 MB native binary), sandboxing incompatible with Flutter plugins, excessive memory overhead, 3-4× startup time, and distribution-specific bugs.
4. **Impossible Maintenance**: Supporting and testing on Ubuntu, Fedora, Arch, openSUSE, Gentoo, etc. would require resources and time I don't have.
5. **The Linux Philosophy**: The Linux ecosystem favors flexibility and user autonomy. This implies managing your own system and dependencies according to your distribution.

### What You Get

- A Linux binary compiled for Debian 13 (`build/linux/x64/release/bundle/`)
- Complete list of dependencies (see below)
- Documentation on required libraries

### What You Will NEVER Get

- ❌ A Flatpak package
- ❌ An AppImage package
- ❌ A Snap package
- ❌ Support for Ubuntu, Fedora, Arch, or any other distribution
- ❌ Help installing dependencies on your distro
- ❌ Specific builds for other distributions

**When Linux has a common package standardization, it will be the best of all worlds. But that's not the case today.**

---

## 🚀 Installation

### Step 1: Download the Archive

Download the `axomind_X.X.X.tar.gz` archive from [the releases page](https://github.com/Sebastien-VZN/axomind/releases).

```bash
# Example extraction
tar -xzf axomind_X.X.X.tar.gz
cd build/linux/x64/release/bundle/
```

### Step 2: Install System Dependencies

**On Debian 13 only:**

```bash
sudo apt update && sudo apt install -y \
  libgtk-3-0 \
  libgdk-pixbuf-2.0-0 \
  libcairo2 \
  libpango-1.0-0 \
  libglib2.0-0 \
  libgstreamer1.0-0 \
  libgstreamer-plugins-base1.0-0 \
  libayatana-appindicator3-1 \
  libayatana-indicator3-7 \
  libayatana-ido3-0.4-0 \
  libdbusmenu-glib4 \
  libdbusmenu-gtk3-4 \
  libx11-6 \
  libxext6 \
  libxfixes3 \
  libxi6 \
  libwayland-client0 \
  libwayland-cursor0 \
  libwayland-egl1 \
  libxkbcommon0 \
  libfontconfig1 \
  libfreetype6 \
  libharfbuzz0b \
  libpng16-16 \
  libjpeg62-turbo \
  libdbus-1-3 \
  libsystemd0
```

**For other distributions:** You must identify equivalent packages in your package manager (see the detailed dependency list below).

### Step 3: Make the Application Executable

**Via graphical interface:**
1. Right-click on the `axomind` file
2. Go to **Properties** → **Permissions** tab
3. Check the box **"Allow executing file as program"**

**Via terminal:**
```bash
chmod +x axomind
```

### Step 4: Verify Dependencies

```bash
ldd axomind | grep "not found"
```

If this command returns nothing, all dependencies are installed correctly.

### Step 5: Launch the Application

**Via double-click** on the `axomind` file or **via terminal:**

```bash
./axomind
```

---

## Dependency Details by Category

This section details all system dependencies required by Axomind on Debian 13. This information can help you identify equivalent packages on other Linux distributions.

### 1. GTK3 & Graphical Interface

**Packages:**
- `libgtk-3-0` - GTK3 toolkit (widgets, windows)
- `libgdk-3-0` - GDK (Drawing Kit, low-level graphics)
- `libgdk-pixbuf-2.0-0` - Image loading and manipulation
- `libatk-1.0-0` - Accessibility Toolkit
- `libatk-bridge-2.0-0` - Bridge between ATK and AT-SPI
- `libepoxy-0` - OpenGL/EGL abstraction

**Role:**
- User interface display (windows, buttons, forms)
- Flutter widget rendering via GTK3
- Accessibility support (screen readers)

**Used by:**
- `libflutter_linux_gtk.so` (Flutter engine)
- All Flutter Linux plugins

---

### 2. Cairo & Pango (Graphics Rendering)

**Packages:**
- `libcairo2` - 2D vector rendering library
- `libcairo-gobject2` - GObject bindings for Cairo
- `libpango-1.0-0` - Text rendering engine
- `libpangocairo-1.0-0` - Pango/Cairo integration
- `libpangoft2-1.0-0` - FreeType support for Pango

**Role:**
- Vector rendering (shapes, paths, gradients)
- Advanced text rendering (formatting, fonts)
- Aurora animations support (`libs_anim_bg_aurora/`)

**Used for:**
- Background animations (Aurora effects, particles)
- Message rendering in messenger
- Vector icon and image rendering

---

### 3. GLib & GIO (Foundation)

**Packages:**
- `libglib2.0-0` - C utility library (data structures, event loop)
- `libgobject-2.0-0` - GLib object system
- `libgio-2.0-0` - GLib I/O and networking (files, sockets)
- `libgmodule-2.0-0` - Dynamic module loading

**Role:**
- Foundation of GTK3 and GStreamer
- Event management (keyboard, mouse, timers)
- File and network access

**Used by:**
- All GTK3 components
- GStreamer (audio/video)
- Flutter plugins (file picker, URL launcher)

---

### 4. GStreamer (Multimedia)

**Packages:**
- `libgstreamer1.0-0` - Multimedia framework
- `libgstreamer-plugins-base1.0-0` - Base GStreamer plugins
- `gstreamer1.0-plugins-base` - Essential audio/video plugins
- `gstreamer1.0-plugins-good` - Audio/video plugins (MP3, H.264, etc.)

**Role:**
- Audio playback (sound notifications, voice messages)
- Audio recording (`record_linux` plugin)
- Video playback (media sharing)

**Used by:**
- `libaudioplayers_linux_plugin.so` (audio playback)
- `librecord_linux_plugin.so` (audio recording)
- `lib/app/libs_medias/` (media gallery, video player)
- `lib/app/shared_widgets/audio/` (player and recorder)

**Recommended packages for complete codecs:**
```bash
sudo apt install -y \
  gstreamer1.0-plugins-base \
  gstreamer1.0-plugins-good \
  gstreamer1.0-plugins-bad \
  gstreamer1.0-plugins-ugly \
  gstreamer1.0-libav
```

---

### 5. System Tray (Ayatana AppIndicator)

**Packages:**
- `libayatana-appindicator3-1` - AppIndicator API (system tray)
- `libayatana-indicator3-7` - Ayatana Indicator framework
- `libayatana-ido3-0.4-0` - Indicator Display Objects
- `libdbusmenu-glib4` - Menus via D-Bus (GLib)
- `libdbusmenu-gtk3-4` - Menus via D-Bus (GTK3)

**Role:**
- System tray icon
- System tray context menu
- System notifications

**Used by:**
- `libtray_manager_plugin.so` (Flutter plugin)
- `lib/app/shared_core/services/ctrl_systray.dart`

**⚠️ Note on warning:**
The warning `'app_indicator_new' is deprecated` comes from the `tray_manager` plugin using a deprecated AppIndicator function. This is not an error, just a maintenance warning. The system tray icon works normally.

**Supported desktop environments:**
- GNOME (with AppIndicator extension)
- KDE Plasma
- XFCE
- MATE
- Cinnamon

---

### 6. X11 & Wayland (Display Servers)

**X11 Packages:**
- `libx11-6` - X11 client (X server connection)
- `libxi6` - X11 Input Extension (mouse, keyboard, touch)
- `libxext6` - X11 Extensions (shapes, double buffering)
- `libxfixes3` - X11 Fixes Extension (cursor, region)
- `libxcursor1` - X11 cursor support
- `libxdamage1` - X11 Damage Extension (modified areas)
- `libxcomposite1` - X11 Composite Extension (compositing)
- `libxrandr2` - X11 RandR Extension (multiple screens)
- `libxinerama1` - X11 Xinerama Extension (multiple screens legacy)
- `libxrender1` - X11 Render Extension (anti-aliased rendering)
- `libxcb1` - X C Bindings (low level)
- `libxcb-render0` - XCB Render Extension
- `libxcb-shm0` - XCB Shared Memory Extension
- `libxau6` - X11 Authorization (authentication)
- `libxdmcp6` - X Display Manager Control Protocol

**Wayland Packages:**
- `libwayland-client0` - Wayland client
- `libwayland-cursor0` - Wayland cursor support
- `libwayland-egl1` - EGL for Wayland (OpenGL)
- `libxkbcommon0` - Keyboard management (Wayland and X11)

**Role:**
- Communication with display server (X11 or Wayland)
- Input management (keyboard, mouse, touch)
- Window management (movement, resizing)
- Multi-screen and high-resolution displays

**Used by:**
- `libwindow_manager_plugin.so` (window management)
- `libscreen_retriever_linux_plugin.so` (screen info)
- `lib/app/shared_core/services/ctrl_apps.dart` (window management)

**Multi-platform support:**
- ✅ X11 (Xorg) - Full support
- ✅ Wayland - Full support (via XWayland if needed)

---

### 7. Fonts & Text Rendering

**Packages:**
- `libfontconfig1` - Font configuration and discovery
- `libfreetype6` - TrueType/OpenType font rendering engine
- `libharfbuzz0b` - Text shaping engine
- `libfribidi0` - Unicode Bidirectional Algorithm (Arabic, Hebrew)
- `libthai0` - Thai language support
- `libgraphite2-3` - Graphite fonts (complex scripts)

**Role:**
- Text display in all languages
- Emoji support (NotoColorEmoji embedded in app)
- Advanced text rendering (ligatures, diacritics)

**Used for:**
- User interface (text, buttons, forms)
- Messenger (messages, usernames)
- Translation system (`lib/app/shared_core/translate/`)

**Fonts embedded in the application:**
- NotoColorEmoji (emojis) - already included in Flutter bundle
- No need to install additional fonts

---

### 8. Images & Compression

**Packages:**
- `libpng16-16` - PNG support
- `libjpeg62-turbo` - JPEG support (optimized)
- `libpixman-1-0` - Pixel manipulation (low level)
- `zlib1g` - ZLIB compression (PNG, etc.)
- `libbz2-1.0` - BZ2 compression
- `libbrotli1` - Brotli compression

**Role:**
- Image loading (PNG, JPEG)
- Data compression/decompression
- Pixel manipulation for animations

**Used for:**
- Media gallery (`lib/app/libs_medias/`)
- User avatars
- Images shared in messenger
- Application assets

---

### 9. D-Bus & Inter-Process Communication

**Packages:**
- `libdbus-1-3` - D-Bus message bus system
- `libatspi0` - AT-SPI (Assistive Technology Service Provider Interface)
- `libsystemd0` - systemd library (journal, notifications)

**Role:**
- Inter-application communication (D-Bus)
- System notifications (via systemd)
- Accessibility (screen readers)
- System tray (via D-Bus)

**Used by:**
- System tray (AppIndicator via D-Bus)
- System notifications
- URL launcher (`liburl_launcher_linux_plugin.so`)

---

### 10. Standard System Libraries

**Packages:**
- `libc6` - GNU C standard library
- `libstdc++6` - GNU C++ standard library
- `libm6` - Math library
- `libgcc-s1` - GCC runtime support
- `libpthread` - POSIX threads support
- `libdl` - Dynamic library loading
- `libatomic1` - Atomic operations
- `libffi8` - Foreign Function Interface

**Role:**
- Foundation of all C/C++ programs
- Memory management, threads, mathematics
- Plugin loading

**Used by:**
- All application components
- Flutter engine
- All plugins

---

### 11. Other System Utilities

**Packages:**
- `libmount1` - Mount management (filesystems)
- `libblkid1` - Block device identification
- `libselinux1` - SELinux (security)
- `libexpat1` - XML parser
- `libpcre2-8-0` - Regular expressions (Perl Compatible)
- `libdatrie1` - Trie data structure
- `libcloudproviders0` - Cloud provider integration (GTK3)
- `libcap2` - POSIX Capabilities

**Role:**
- File and device access
- XML configuration parsing
- Security and permissions
- Cloud integration (optional)

---

## Internal Flutter Plugins (Included in Bundle)

These libraries are **already included** in `build/linux/x64/release/bundle/lib/` and don't need to be installed separately:

| Plugin | File | Functionality |
|--------|------|---------------|
| **audioPlayers** | `libaudioplayers_linux_plugin.so` | Audio playback (notifications, voice messages) |
| **desktopDrop** | `libdesktop_drop_plugin.so` | File drag & drop |
| **emojiPickerFlutter** | `libemoji_picker_flutter_plugin.so` | Emoji picker |
| **gtk** | `libgtk_plugin.so` | Advanced GTK3 integration |
| **record** | `librecord_linux_plugin.so` | Audio recording |
| **screenRetriever** | `libscreen_retriever_linux_plugin.so` | Screen information (multi-screen) |
| **trayManager** | `libtray_manager_plugin.so` | System tray (system bar icon) |
| **urlLauncher** | `liburl_launcher_linux_plugin.so` | Opening URLs in browser |
| **windowManager** | `libwindow_manager_plugin.so` | Window management (min size: 1000×600) |
| **Flutter Engine** | `libflutter_linux_gtk.so` | Flutter rendering engine |

---

## Linux Bundle Size

```
build/linux/x64/release/bundle/
├── axomind (executable)            ~50 MB
├── lib/ (Flutter plugins)              ~30 MB
├── data/ (assets, icons, fonts)        ~20 MB
└── Total:                              ~100 MB
```

---

## Troubleshooting

### Error: "error while loading shared libraries"

```bash
# Identify the missing library
ldd build/linux/x64/release/bundle/axomind | grep "not found"

# Search for the Debian package providing this library
apt-file search <lib_name.so>

# Install the package
sudo apt install <package_name>
```

**Example:**
```bash
# Error: libgtk-3.so.0 => not found
apt-file search libgtk-3.so.0
# Result: libgtk-3-0: /usr/lib/x86_64-linux-gnu/libgtk-3.so.0
sudo apt install libgtk-3-0
```

### System Tray Not Showing

**Possible causes:**
1. Unsupported desktop environment (check the list above)
2. GNOME without AppIndicator extension:
   ```bash
   # Install AppIndicator extension for GNOME
   sudo apt install gnome-shell-extension-appindicator
   ```
3. Restart session after installation

### Audio Not Working

**Check GStreamer:**
```bash
# Test GStreamer
gst-launch-1.0 audiotestsrc ! autoaudiosink

# Install missing codecs
sudo apt install -y \
  gstreamer1.0-plugins-base \
  gstreamer1.0-plugins-good \
  gstreamer1.0-plugins-bad \
  gstreamer1.0-plugins-ugly \
  gstreamer1.0-alsa \
  gstreamer1.0-pulseaudio
```

### Missing Fonts or Emojis

**Emojis are already embedded** in the application (NotoColorEmoji), but if you encounter issues:

```bash
# Install additional fonts
sudo apt install -y \
  fonts-noto-color-emoji \
  fonts-liberation \
  fonts-dejavu-core
```

---

## References

- Flutter Linux Desktop: https://docs.flutter.dev/platform-integration/linux/building
- GTK3 Documentation: https://docs.gtk.org/gtk3/
- GStreamer Documentation: https://gstreamer.freedesktop.org/documentation/
- Ayatana Indicators: https://ayatana-indicators.org/
- [Debian Packages](https://packages.debian.org/trixie/)

---

**Last updated:** 2026-08-22

---

<details>
<summary><b>🇫🇷 Version française / French version</b></summary>

## ⚠️ AVERTISSEMENT IMPORTANT

**Axomind fonctionne sur Debian 13 (Trixie) UNIQUEMENT.**

**Pour les autres distributions Linux (Ubuntu, Fedora, Arch, openSUSE, etc.) : AUCUN SUPPORT ne sera fourni.**

### Politique de support Linux

**Vous recevrez uniquement le binaire compilé.** C'est à vous de gérer votre environnement Linux et d'installer les dépendances nécessaires pour votre distribution.

### Pourquoi aucun support multi-distros ?

1. **Fragmentation de l'écosystème Linux** : Chaque distribution a ses propres versions de bibliothèques, ses propres gestionnaires de paquets, ses propres chemins d'installation.
2. **Pas de normalisation des packages** : Contrairement à Windows ou macOS, il n'existe pas de standard unique pour les dépendances et les formats de distribution.
3. **Flatpak/Snap/AppImage** : Tests révélant des limites (500 MB vs 50 MB natif, sandboxing incompatible, surcharge mémoire, etc.).
4. **Maintenance impossible** : Supporter et tester sur Ubuntu, Fedora, Arch, openSUSE, Gentoo, etc. nécessiterait des ressources et du temps que je n'ai pas.
5. **La philosophie Linux** : L'écosystème Linux privilégie la flexibilité et l'autonomie des utilisateurs. Cela implique de gérer vous-même votre système et vos dépendances selon votre distribution.

### Ce que vous obtenez

- Un binaire Linux compilé pour Debian 13 (`build/linux/x64/release/bundle/`)
- La liste complète des dépendances (voir ci-dessous)
- Une documentation sur les bibliothèques requises

### Ce que vous ne recevrez JAMAIS

- ❌ Un package Flatpak
- ❌ Un package AppImage
- ❌ Un package Snap
- ❌ Un support pour Ubuntu, Fedora, Arch ou toute autre distribution
- ❌ Une aide pour installer les dépendances sur votre distro
- ❌ Des builds spécifiques pour d'autres distributions

**Quand Linux aura une normalisation des packages commune, on sera dans le meilleur des mondes. Mais ce n'est pas le cas aujourd'hui.**

---

## 🚀 Installation

### Étape 1 : Télécharger l'archive

Téléchargez l'archive `axomind_X.X.X.tar.gz` depuis [les releases](https://github.com/Sebastien-VZN/axomind/releases).

```bash
# Exemple d'extraction
tar -xzf axomind_X.X.X.tar.gz
cd build/linux/x64/release/bundle/
```

### Étape 2 : Installer les dépendances système

**Sur Debian 13 uniquement :**

```bash
sudo apt update && sudo apt install -y \
  libgtk-3-0 \
  libgdk-pixbuf-2.0-0 \
  libcairo2 \
  libpango-1.0-0 \
  libglib2.0-0 \
  libgstreamer1.0-0 \
  libgstreamer-plugins-base1.0-0 \
  libayatana-appindicator3-1 \
  libayatana-indicator3-7 \
  libayatana-ido3-0.4-0 \
  libdbusmenu-glib4 \
  libdbusmenu-gtk3-4 \
  libx11-6 \
  libxext6 \
  libxfixes3 \
  libxi6 \
  libwayland-client0 \
  libwayland-cursor0 \
  libwayland-egl1 \
  libxkbcommon0 \
  libfontconfig1 \
  libfreetype6 \
  libharfbuzz0b \
  libpng16-16 \
  libjpeg62-turbo \
  libdbus-1-3 \
  libsystemd0
```

**Pour d'autres distributions :** Vous devez identifier les paquets équivalents dans votre gestionnaire de paquets (voir la liste détaillée des dépendances ci-dessous).

### Étape 3 : Rendre l'application exécutable

**Via l'interface graphique :**
1. Faites un clic droit sur le fichier `axomind`
2. Allez dans **Propriétés** → onglet **Permissions**
3. Cochez la case **"Autoriser l'exécution du fichier comme un programme"**

**Via le terminal :**
```bash
chmod +x axomind
```

### Étape 4 : Vérifier les dépendances

```bash
ldd axomind | grep "not found"
```

Si cette commande ne retourne rien, toutes les dépendances sont installées correctement.

### Étape 5 : Lancer l'application

**Via double-clic** sur le fichier `axomind` ou **via terminal :**

```bash
./axomind
```

---

## Détails des Dépendances par Catégorie

Cette section détaille toutes les dépendances système requises par Axomind sur Debian 13. Ces informations peuvent vous aider à identifier les paquets équivalents sur d'autres distributions Linux.

### 1. GTK3 & Interface Graphique

**Paquets :**
- `libgtk-3-0` - Toolkit GTK3 (widgets, fenêtres)
- `libgdk-3-0` - GDK (Drawing Kit, bas niveau graphique)
- `libgdk-pixbuf-2.0-0` - Chargement et manipulation d'images
- `libatk-1.0-0` - Accessibility Toolkit (accessibilité)
- `libatk-bridge-2.0-0` - Pont entre ATK et AT-SPI
- `libepoxy-0` - Abstraction OpenGL/EGL

**Rôle :**
- Affichage de l'interface utilisateur (fenêtres, boutons, formulaires)
- Rendu des widgets Flutter via GTK3
- Support de l'accessibilité (lecteurs d'écran)

**Utilisé par :**
- `libflutter_linux_gtk.so` (moteur Flutter)
- Tous les plugins Flutter Linux

---

### 2. Cairo & Pango (Rendu Graphique)

**Paquets :**
- `libcairo2` - Bibliothèque de rendu 2D vectoriel
- `libcairo-gobject2` - Bindings GObject pour Cairo
- `libpango-1.0-0` - Moteur de rendu de texte
- `libpangocairo-1.0-0` - Intégration Pango/Cairo
- `libpangoft2-1.0-0` - Support FreeType pour Pango

**Rôle :**
- Rendu vectoriel (formes, chemins, gradients)
- Rendu de texte avancé (mise en forme, polices)
- Support des animations Aurora (`libs_anim_bg_aurora/`)

**Utilisé pour :**
- Animations de fond (Aurora effects, particles)
- Rendu des messages dans le messenger
- Rendu des icônes et images vectorielles

---

### 3. GLib & GIO (Fondation)

**Paquets :**
- `libglib2.0-0` - Bibliothèque utilitaire C (structures de données, boucle d'événements)
- `libgobject-2.0-0` - Système d'objets GLib
- `libgio-2.0-0` - I/O et réseau GLib (fichiers, sockets)
- `libgmodule-2.0-0` - Chargement dynamique de modules

**Rôle :**
- Fondation de GTK3 et GStreamer
- Gestion des événements (clavier, souris, timers)
- Accès aux fichiers et réseau

**Utilisé par :**
- Tous les composants GTK3
- GStreamer (audio/vidéo)
- Plugins Flutter (file picker, URL launcher)

---

### 4. GStreamer (Multimédia)

**Paquets :**
- `libgstreamer1.0-0` - Framework multimédia
- `libgstreamer-plugins-base1.0-0` - Plugins de base GStreamer
- `gstreamer1.0-plugins-base` - Plugins audio/vidéo essentiels
- `gstreamer1.0-plugins-good` - Plugins audio/vidéo (MP3, H.264, etc.)

**Rôle :**
- Lecture audio (notifications sonores, messages vocaux)
- Enregistrement audio (plugin `record_linux`)
- Lecture vidéo (partage de médias)

**Utilisé par :**
- `libaudioplayers_linux_plugin.so` (lecture audio)
- `librecord_linux_plugin.so` (enregistrement audio)
- `lib/app/libs_medias/` (galerie médias, lecteur vidéo)
- `lib/app/shared_widgets/audio/` (player et recorder)

**Paquets recommandés pour codecs complets :**
```bash
sudo apt install -y \
  gstreamer1.0-plugins-base \
  gstreamer1.0-plugins-good \
  gstreamer1.0-plugins-bad \
  gstreamer1.0-plugins-ugly \
  gstreamer1.0-libav
```

---

### 5. System Tray (Ayatana AppIndicator)

**Paquets :**
- `libayatana-appindicator3-1` - API AppIndicator (system tray)
- `libayatana-indicator3-7` - Framework Ayatana Indicator
- `libayatana-ido3-0.4-0` - Indicateur Display Objects
- `libdbusmenu-glib4` - Menus via D-Bus (GLib)
- `libdbusmenu-gtk3-4` - Menus via D-Bus (GTK3)

**Rôle :**
- Icône dans la barre système (system tray)
- Menu contextuel du system tray
- Notifications système

**Utilisé par :**
- `libtray_manager_plugin.so` (plugin Flutter)
- `lib/app/shared_core/services/ctrl_systray.dart`

**⚠️ Note sur le warning :**
Le warning `'app_indicator_new' is deprecated` vient du plugin `tray_manager` qui utilise une fonction dépréciée d'AppIndicator. Ce n'est pas une erreur, juste un avertissement de maintenance. L'icône system tray fonctionne normalement.

**Environnements de bureau supportés :**
- GNOME (avec extension AppIndicator)
- KDE Plasma
- XFCE
- MATE
- Cinnamon

---

### 6. X11 & Wayland (Serveurs d'affichage)

**Paquets X11 :**
- `libx11-6` - Client X11 (connexion au serveur X)
- `libxi6` - X11 Input Extension (souris, clavier, touch)
- `libxext6` - Extensions X11 (formes, double buffering)
- `libxfixes3` - X11 Fixes Extension (curseur, région)
- `libxcursor1` - Support des curseurs X11
- `libxdamage1` - X11 Damage Extension (zones modifiées)
- `libxcomposite1` - X11 Composite Extension (compositing)
- `libxrandr2` - X11 RandR Extension (écrans multiples)
- `libxinerama1` - X11 Xinerama Extension (écrans multiples legacy)
- `libxrender1` - X11 Render Extension (rendu anti-aliasé)
- `libxcb1` - X C Bindings (bas niveau)
- `libxcb-render0` - XCB Render Extension
- `libxcb-shm0` - XCB Shared Memory Extension
- `libxau6` - X11 Authorization (authentification)
- `libxdmcp6` - X Display Manager Control Protocol

**Paquets Wayland :**
- `libwayland-client0` - Client Wayland
- `libwayland-cursor0` - Support des curseurs Wayland
- `libwayland-egl1` - EGL pour Wayland (OpenGL)
- `libxkbcommon0` - Gestion du clavier (Wayland et X11)

**Rôle :**
- Communication avec le serveur d'affichage (X11 ou Wayland)
- Gestion des entrées (clavier, souris, touch)
- Gestion des fenêtres (déplacement, redimensionnement)
- Multi-écrans et écrans haute résolution

**Utilisé par :**
- `libwindow_manager_plugin.so` (gestion fenêtre)
- `libscreen_retriever_linux_plugin.so` (infos écrans)
- `lib/app/shared_core/services/ctrl_apps.dart` (gestion fenêtre)

**Support multi-plateforme :**
- ✅ X11 (Xorg) - Support complet
- ✅ Wayland - Support complet (via XWayland si nécessaire)

---

### 7. Polices & Rendu de Texte

**Paquets :**
- `libfontconfig1` - Configuration et découverte de polices
- `libfreetype6` - Moteur de rendu de polices TrueType/OpenType
- `libharfbuzz0b` - Moteur de façonnage de texte (shaping)
- `libfribidi0` - Unicode Bidirectional Algorithm (arabe, hébreu)
- `libthai0` - Support de la langue thaïe
- `libgraphite2-3` - Polices Graphite (scripts complexes)

**Rôle :**
- Affichage de texte dans toutes les langues
- Support des émojis (NotoColorEmoji embarqué dans l'app)
- Rendu de texte avancé (ligatures, diacritiques)

**Utilisé pour :**
- Interface utilisateur (textes, boutons, formulaires)
- Messenger (messages, noms d'utilisateurs)
- Système de traduction (`lib/app/shared_core/translate/`)

**Polices embarquées dans l'application :**
- NotoColorEmoji (émojis) - déjà inclus dans le bundle Flutter
- Pas besoin d'installer de polices supplémentaires

---

### 8. Images & Compression

**Paquets :**
- `libpng16-16` - Support PNG
- `libjpeg62-turbo` - Support JPEG (optimisé)
- `libpixman-1-0` - Manipulation de pixels (bas niveau)
- `zlib1g` - Compression ZLIB (PNG, etc.)
- `libbz2-1.0` - Compression BZ2
- `libbrotli1` - Compression Brotli

**Rôle :**
- Chargement d'images (PNG, JPEG)
- Compression/décompression de données
- Manipulation de pixels pour les animations

**Utilisé pour :**
- Galerie médias (`lib/app/libs_medias/`)
- Avatars utilisateurs
- Images partagées dans le messenger
- Assets de l'application

---

### 9. D-Bus & Communication Inter-Processus

**Paquets :**
- `libdbus-1-3` - Système de bus de messages D-Bus
- `libatspi0` - AT-SPI (Assistive Technology Service Provider Interface)
- `libsystemd0` - Bibliothèque systemd (journal, notifications)

**Rôle :**
- Communication entre applications (D-Bus)
- Notifications système (via systemd)
- Accessibilité (lecteurs d'écran)
- System tray (via D-Bus)

**Utilisé par :**
- System tray (AppIndicator via D-Bus)
- Notifications système
- Launcher d'URLs (`liburl_launcher_linux_plugin.so`)

---

### 10. Bibliothèques Système Standard

**Paquets :**
- `libc6` - Bibliothèque C standard GNU
- `libstdc++6` - Bibliothèque C++ standard GNU
- `libm6` - Bibliothèque mathématique
- `libgcc-s1` - Support runtime GCC
- `libpthread` - Support des threads POSIX
- `libdl` - Chargement dynamique de bibliothèques
- `libatomic1` - Opérations atomiques
- `libffi8` - Foreign Function Interface

**Rôle :**
- Fondation de tous les programmes C/C++
- Gestion mémoire, threads, mathématiques
- Chargement de plugins

**Utilisé par :**
- Tous les composants de l'application
- Moteur Flutter
- Tous les plugins

---

### 11. Autres Utilitaires Système

**Paquets :**
- `libmount1` - Gestion des montages (filesystems)
- `libblkid1` - Identification de périphériques bloc
- `libselinux1` - SELinux (sécurité)
- `libexpat1` - Parser XML
- `libpcre2-8-0` - Expressions régulières (Perl Compatible)
- `libdatrie1` - Structure de données Trie
- `libcloudproviders0` - Intégration fournisseurs cloud (GTK3)
- `libcap2` - POSIX Capabilities

**Rôle :**
- Accès aux fichiers et périphériques
- Parsing de configuration XML
- Sécurité et permissions
- Intégration cloud (optionnel)

---

## Plugins Flutter Internes (Inclus dans le Bundle)

Ces bibliothèques sont **déjà incluses** dans `build/linux/x64/release/bundle/lib/` et n'ont pas besoin d'être installées séparément :

| Plugin | Fichier | Fonctionnalité |
|--------|---------|----------------|
| **audioPlayers** | `libaudioplayers_linux_plugin.so` | Lecture audio (notifications, messages vocaux) |
| **desktopDrop** | `libdesktop_drop_plugin.so` | Drag & drop de fichiers |
| **emojiPickerFlutter** | `libemoji_picker_flutter_plugin.so` | Sélecteur d'émojis |
| **gtk** | `libgtk_plugin.so` | Intégration GTK3 avancée |
| **record** | `librecord_linux_plugin.so` | Enregistrement audio |
| **screenRetriever** | `libscreen_retriever_linux_plugin.so` | Informations écrans (multi-écrans) |
| **trayManager** | `libtray_manager_plugin.so` | System tray (icône barre système) |
| **urlLauncher** | `liburl_launcher_linux_plugin.so` | Ouverture d'URLs dans le navigateur |
| **windowManager** | `libwindow_manager_plugin.so` | Gestion fenêtre (min size: 1000×600) |
| **Flutter Engine** | `libflutter_linux_gtk.so` | Moteur de rendu Flutter |

---

## Taille du Bundle Linux

```
build/linux/x64/release/bundle/
├── axomind (exécutable)            ~50 MB
├── lib/ (plugins Flutter)              ~30 MB
├── data/ (assets, icns, fonts)         ~20 MB
└── Total:                              ~100 MB
```

---

## Résolution de Problèmes

### Erreur: "error while loading shared libraries"

```bash
# Identifiez la bibliothèque manquante
ldd build/linux/x64/release/bundle/axomind | grep "not found"

# Recherchez le paquet Debian qui fournit cette bibliothèque
apt-file search <nom_de_la_lib.so>

# Installez le paquet
sudo apt install <nom_du_paquet>
```

**Exemple :**
```bash
# Erreur: libgtk-3.so.0 => not found
apt-file search libgtk-3.so.0
# Résultat: libgtk-3-0: /usr/lib/x86_64-linux-gnu/libgtk-3.so.0
sudo apt install libgtk-3-0
```

### System Tray ne s'affiche pas

**Causes possibles :**
1. Environnement de bureau non supporté (vérifiez la liste ci-dessus)
2. GNOME sans extension AppIndicator :
   ```bash
   # Installer l'extension AppIndicator pour GNOME
   sudo apt install gnome-shell-extension-appindicator
   ```
3. Redémarrez la session après installation

### Audio ne fonctionne pas

**Vérifiez GStreamer :**
```bash
# Testez GStreamer
gst-launch-1.0 audiotestsrc ! autoaudiosink

# Installez les codecs manquants
sudo apt install -y \
  gstreamer1.0-plugins-base \
  gstreamer1.0-plugins-good \
  gstreamer1.0-plugins-bad \
  gstreamer1.0-plugins-ugly \
  gstreamer1.0-alsa \
  gstreamer1.0-pulseaudio
```

### Polices ou émojis manquants

**Les émojis sont déjà embarqués** dans l'application (NotoColorEmoji), mais si vous rencontrez des problèmes :

```bash
# Installez des polices supplémentaires
sudo apt install -y \
  fonts-noto-color-emoji \
  fonts-liberation \
  fonts-dejavu-core
```

---

## Références

- Flutter Linux Desktop: https://docs.flutter.dev/platform-integration/linux/building
- GTK3 Documentation: https://docs.gtk.org/gtk3/
- GStreamer Documentation: https://gstreamer.freedesktop.org/documentation/
- Ayatana Indicators: https://ayatana-indicators.org/
- [Debian Packages](https://packages.debian.org/trixie/)

---

**Dernière mise à jour :** 2026-08-22

</details>
