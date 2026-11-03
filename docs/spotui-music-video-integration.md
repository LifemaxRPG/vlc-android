# Spotui-inspired Music + Video UI integration

Status: architecture and repository reconnaissance complete; implementation not yet merged or built.

## Baseline
Branch `feature/spotui-music-video` forks `feature/audio-to-popup-test` at commit `99f2a8658233f5b846c518a46448f05cecf5d513`. Preserve existing floating-overlay and PiP behavior.

## Existing Android integration points
- `application/vlc-android/src/org/videolan/vlc/gui/MainActivity.kt`: main navigation setup and audio-player container.
- `application/vlc-android/src/org/videolan/vlc/gui/audio/AudioBrowserFragment.kt`: existing music library.
- `application/vlc-android/src/org/videolan/vlc/gui/video/VideoGridFragment.kt`: existing video library.

## Target UX
A unified dark media home with All / Music / Videos / Podcasts filters, recently played and favorites shelves, persistent VLC mini-player, and Home / Search / Library navigation. Video tab must expose existing local video library and retain video options (subtitles, audio delay, floating player, PiP).

## Implementation sequencing
1. Inspect existing navigation resources, lifecycle, and media-library view models. Add an opt-in entry point, leaving original audio/video navigation intact.
2. Implement a unified media home using VLC's existing local library as source of truth. No fake recommendations or placeholder playback controls.
3. Wire Music and Videos filters to VLC media-library screens, preserving back-stack, permission prompts, tablet layouts, Android TV, and playback service ownership.
4. Reuse existing VLC mini-player and playback service; do not create a second audio session.
5. Add tests for navigation, empty library, permission denial, rotation, resumed playback, floating overlay and PiP.
6. Build the dedicated debug APK through existing GitHub Actions and perform a device smoke test before considering replacement of default navigation.

## Spotui integration constraints
Treat Spotui as a UI reference until its exact source dependencies, license notices, and component compatibility are audited. Avoid directly importing its third-party stream-resolving mechanisms or embedding Spotify credentials. Spotify catalog authentication and playback are separate workstreams; Spotify's SDK does not supply raw audio for VLC to decode.

## Acceptance criteria
- Existing VLC music/video playback continues to work.
- Music and Video are accessible from the unified UI.
- No loss of existing custom floating-player action.
- No regression to app startup or navigation after rotation.
- Debug APK builds successfully; do not claim this until CI confirms.
