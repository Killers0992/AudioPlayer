![GitHub Downloads (all assets, all releases)](https://img.shields.io/github/downloads/Killers0992/AudioPlayer/total?label=Downloads&labelColor=2e343e&color=00FFFF&style=for-the-badge)
[![Discord](https://img.shields.io/discord/1216429195232673964?label=Discord&labelColor=2e343e&color=00FFFF&style=for-the-badge)](https://discord.gg/czQCAsDMHa)
# AudioPlayer

**AudioPlayer** is a dependency for plugins that provides advanced capabilities for managing and playing audio clips, complete with support for spatial audio and multiple speakers. This plugin is designed for use in SCP: SL Dedicated servers.

---

## Features

- Create, manage, and play multiple audio clips.
- Spatial audio support for immersive sound experiences.
- Add multiple speakers with individual configurations.
- Seamless integration with game events and player-specific audio setups.

---

# Audio requirements

- Format .ogg
- Channels: 1 ( MONO )
- Freq: 48Khz

---

## Installation

1. Download latest [dependencies.zip](https://github.com/Killers0992/AudioPlayer/releases/latest/download/dependencies.zip) from ``Releases``.
2. Extract all files and put in your dependencies folder. ( this path may be different depending on which plugin framework you use )

---

## Examples

<details>
<summary>
1. Loading audio clips
</summary>

```C#
// This method should be called when plugin loads.
public void OnPluginLoad()
{
    // Specify path for your ogg file and name it, this name will be used later for adding clips to audio players.
    //
    //  Make sure that ogg file is MONO and frequency is 48Khz 
    //
    AudioClipStorage.LoadClip("C:\\Users\\Kille\\Documents\\Serwer\\com.ogg", "shot");
}
```
</details>

<details>
<summary>2. Audio player attached to players.</summary>

```C#
// Creating audio player which is attached to player which means any added clip to this audio player will be directly at player position.
public void CreateForPlayer(Player player)
{
    AudioPlayer audioPlayer = AudioPlayer.CreateOrGet($"Player {player.Nickname}", onIntialCreation: (p) =>
    {        
        // Attach created audio player to player.
        p.transform.parent = player.GameObject.transform;

        // This created speaker will be in 3D space.
        Speaker speaker = p.AddSpeaker("Main", isSpatial: true, minDistance: 5f, maxDistance: 15f);

        // Attach created speaker to player.
        speaker.transform.parent = player.GameObject.transform;

        // Set local positino to zero to make sure that speaker is in player.
        speaker.transform.localPosition = Vector3.zero;
    });

    // As example we will add clip
    audioPlayer.AddClip("shot");
}

// Creates global audio player which everyone can hear from any location.
public void CreateGlobal()
{
    AudioPlayer audioPlayer = AudioPlayer.CreateOrGet($"Global AudioPlayer", onIntialCreation: (p) =>
    {
        // This created speaker will be in 2D space ( audio will be always playing directly on you not from specific location ) but make sure that max distance is set to some higher value.
        Speaker speaker = p.AddSpeaker("Main", isSpatial: false, maxDistance: 5000f);
    });

    audioPlayer.AddClip("shot");
}
```
</details>

<details>
<summary>2. Global audio players.</summary>

```C#
// Creates global audio player which everyone can hear from any location.
public void CreateGlobal()
{
    AudioPlayer audioPlayer = AudioPlayer.CreateOrGet($"Global AudioPlayer", onIntialCreation: (p) =>
    {
        // This created speaker will be in 2D space ( audio will be always playing directly on you not from specific location ) but make sure that max distance is set to some higher value.
        Speaker speaker = p.AddSpeaker("Main", isSpatial: false, maxDistance: 5000f);
    });

    audioPlayer.AddClip("shot");
}
```
</details>

---

## API Overview

### `AudioPlayer` Class

#### Static Methods

- `AudioPlayer.Create(string name)`: Creates a new `AudioPlayer` instance with the specified name.

#### Properties

- `Name`: The name of the `AudioPlayer` instance.
- `ControllerID`: The ID of the associated controller.

#### Methods

- `AddClip(string clipName, float volume = 1f, bool loop = false, bool destroyOnEnd = true)`: Adds a new audio clip to the player.
- `AddSpeaker(string name, ...)`: Adds a new speaker to the player.
- `RemoveSpeaker(string name)`: Removes a speaker by name.

### `Speaker` Class

Use `Speaker` instances for spatial and directed audio. Methods include creation and positioning.

---

## Usage Notes

- Always preload audio clips using `AudioClipStorage.LoadClip(path, alias)` before referencing them in your plugin.
- Attach audio players and speakers to relevant game objects for spatial audio effects.
- Utilize the `TemporaryData` storage for per-player audio setups.

---
