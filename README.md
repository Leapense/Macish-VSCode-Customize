# VS Code Neon + Macish-VSCode Custom CSS/JS

Modern UI polish for VS Code with neon highlights, rounded widgets, macish blur, and an optional in-editor music player.

What you get

- Title/Activity/Status bar tweaks and gradients
- Rounded lists, command palette glow, suggest/menus styled like native widgets
- Neon syntax glow for Monaco tokens
- Subtle blur overlay when the Command Palette/Quick Input opens
- "MacOS" visuals for overlays and modals
- Top-right music player: drag/drop files, local playlists, speed, pitch toggle, URL add

Works with the "Custom CSS and JS Loader" VS Code extension.

Note: The modifies VS Code's installation. Use only if you're comfortable with that and understand the security trade-offs.

---

## Quick Install

1. Install the extension

- From Markerplace: search for "Custom CSS and JS Loader" (publisher: be5invis), or:
- CLI:
  ```bash
  code --install-extension be5invis.vscode-custom-css
  ```

2. Save the files

- Create a folder and add your custom files:
  ``` bash
  mkdir -p ~/.config/vscode-custom
  # Save your CSS to:
  #   ~/.config/vscode-custom/custom-vscode-css2.css
  # Save your JS to:
  #   ~/.config/vscode-custom/custom-vscode-script2.js
  ```

3. Point the extension at your files (Settings JSON)

- Open: File → Preferences → Settings → gear icon (Open Setting JSON)
- Add:
  ```JSON
  {
    "vscode_custom_css.imports": [
      "file://${userHome}/.config/vscode-custom/custom-vscode-css2.css",
      "file://${userHome}/.config/vscode-custom/custom-vscode-js2.js"
    ]
  }
  ```

The imports must be file:// URIs (not plain paths).

4. Allow patching on Your OS (one-time, with caution)

- The extension needs write access to VS Code's install dir to inject code:
  ```Bash
  sudo chown -R "$(whoami)" "$(which code)"
  sudo chown -R "$(whoami)" /usr/share/code
  ```

5. Enable and reload

- Press F1 → "Enable Custom CSS and JS"
- Reload when prompted

You should now see the neon/macos styling, the blur overlay on Command Palette, and a small music player in the top-right of the editor.

---

## Fedora notes and limitations

- Supported build: RPM (Microsoft "code" repo). Flatpak/Snap builds are containerized and cannot be patched by this extension.
- Insiders: If you use Code Insiders, paths may be /usr/share/code-insiders and the command is code-insiders.
- After every VS Code update re-run "Enable Custom CSS and JS." If ownership was reverted to root, you'll need to chown again before enabling.

---

## Using the music player

- Buttons: ⏮ previous, ▶/⏸ play/pause, ⏭ next
- Drag & drop audio files onto the player
- Add files/folder/URL: click the + button
- Volume: slider with a live tooltip
- Speed: 0.5x, 0.75x, 1x, 1.25x, 1.5x, 2x
- Pitch toggle: 🔒 preserves pitch when changing speed; 🔓 disables that
- Playlist: ♪ shows the list; remove items with ✕; row highlights the current track with a live progress fill
- Settings (⚙️): control play order (repeat all, repeat one, random) and playlist highlighting

Tips

- The player remembers your volume, rate, and UI state via localStorage.
- Errors are logged to Help → Toggle Developer Tools (look for [MusicPlayer] in the console).

---

## Customization

- Edit variables at the top of the CSS to change accents, radii, and fonts:
  - --accent-green, --accent-pink, --accent-purple, --accent-orange
  - --border-radius, --vscode-custom-font
- Performance: Backdrop filters (blur) look great but can be GPU-heavy. To reduce cost:
  - In the CSS, set backdrop-filter: none for menus/suggest widgets
  - In JS (ensureLiquidGlassStyles), lower blur/saturate values or remove overlays

 ---

 ## Troubleshooting

 - Flatpak/Snap build? The extension can't patch those. Install the RPM build:

 ```Bash
sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
sudo sh -c 'echo -e "[code]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/yum.repos.d/vscode.repo'
sudo dnf check-update
sudo dnf install code
 ```

- “Corrupt installation” or “Unsupported” banner: Expected after injection. Use at your own risk.
- Nothing changes after enabling:
  - Confirm the file URIs in settings are correct and reachable
  - Make sure you actually ran “Enable Custom CSS and JS” and reloaded
  - Check the DevTools console for errors (Help → Toggle Developer Tools)
  - Ensure ownership was granted (see chown commands above)
- Quick Input blur doesn’t appear: Wait a second after startup; the script observes the widget and adds the overlay once it exists.
- Music won’t play a URL: Remote servers may block cross‑origin media; add local files instead.

---

## Disable, revert, or uninstall

- Temporarily disable:
  - F1 → “Disable Custom CSS and JS” → Reload
- Remove permanently:
  - Remove the imports from Settings JSON
  - Delete the files in ~/.config/vscode-custom
- Restore secure ownership (recommended after enabling):

```Bash
sudo chown -R root:root /usr/share/code
sudo chown root:root "$(which code)"
```
Note: You'll need to chown back to your user again the next time you re-enable custom CSS/JS.

If thing get messy, you can always reinstall VS Code via dnf.

---

## Credits

- Inspired by SynthWave '84 and Moonlight themes
- Neon token glow approach adapted from examples by hanjeahwan
- Requires "Custom CSS and JS Loader" by be5invis

---

## FAQ

- Will this work on macOS/Windows?

  - Yes, conceptually—but the Linux‑specific chown step differs. On macOS/Windows you’ll need to give the extension write access to the install directory by other means. This README targets Fedora 42 specifically.

- Is this safe?

  - It’s a deliberate modification of VS Code’s installed files. Only use code you trust, and consider reverting ownership to root after enabling to reduce the attack surface.
 
---

## Preview

<img width="1130" height="711" alt="image" src="https://github.com/user-attachments/assets/0a8f8813-9e7e-4f09-8bcb-a182e718aa5e" />

