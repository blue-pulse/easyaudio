<div align="center">

# EasyAudio

![Status: active](https://img.shields.io/badge/status-active-green)
[![License: MIT](https://img.shields.io/badge/license-MIT-yellow)](./LICENSE)
[![GameMaker LTS 2026](https://img.shields.io/badge/gamemaker-LTS%202026-blue)](https://gamemaker.io)

</div>

EasyAudio plays music, sound effects and voice clips in GameMaker. It puts each kind of sound in its own audio group, gives it its own priority and adds what `audio_play_sound` leaves out, such as loop points for music or skipping a sound that is already playing.

## What you can do with it

- Loop music between two points, and restart it cleanly when the same track is played again.
- Play a sound effect so it overlaps, is skipped when already playing, or restarts.
- Play a voice clip that cuts the previous one, or pick one at random from an array.
- Know when the three audio groups have finished loading.
- Get a warning in the output when a sound is played before its group has loaded.

## Use it

Download [`easyaudio.yymps`](https://github.com/blue-pulse/easyaudio/releases/latest/download/easyaudio.yymps) from the [latest release](https://github.com/blue-pulse/easyaudio/releases/latest) and import it into your project as a local package. It needs GameMaker LTS 2026 or later.

The version is in the macros `EASYAUDIO_VERSION` and `EASYAUDIO_DATE`, and the library prints it to the output when the game starts.

## Set up

Create three audio groups in your project: one for the music, one for the sound effects and one for the voice clips. Put each sound in the group that matches it.

Pass the groups to `easyaudio_initialize` when the game starts. The groups load in the background, so wait for `easyaudio_is_loaded` before you play anything.

```js
// Create event of the first object
easyaudio_initialize(audiogroup_music, audiogroup_effects, audiogroup_voices);

// Step event
if (easyaudio_is_loaded()) {
	easyaudio_play_music(mus_town, 4.5);              // loop from second 4.5 to the end
	easyaudio_play_effect(snd_jump);                  // play a sound effect
	easyaudio_play_voice([vo_hello_1, vo_hello_2]);   // play one of them at random
}
```

## Functions

| Function | What it does |
| --- | --- |
| `easyaudio_initialize(music_group, effect_group, voice_group)` | Sets the three audio groups and starts loading them |
| `easyaudio_is_loaded()` | Returns whether the three groups have loaded, which is false until `easyaudio_initialize` is called |
| `easyaudio_play_music(sound, [loop_start], [loop_end], [position], [volume])` | Plays a track in a loop between two points, after stopping the same track |
| `easyaudio_play_effect(sound, [mode], [loop], [volume])` | Plays a sound effect with the chosen mode |
| `easyaudio_play_voice(sound, [volume])` | Stops the voice clips that are playing and plays another one, or one of an array |

The loop points of `easyaudio_play_music` are set on the asset, so they apply to every instance of it. A `loop_end` of 0 means the end of the track.

### Effect modes

| Mode | What happens when the sound is already playing |
| --- | --- |
| `EASYAUDIO_MODE.OVERLAP` | Plays it again on top. This is the default |
| `EASYAUDIO_MODE.SINGLE` | Skips it and returns `noone` |
| `EASYAUDIO_MODE.SWAP` | Stops the playing one and plays it again |

## Settings

The priorities are macros in the script `__easyaudio_config`.

| Macro | Default | Used for |
| --- | --- | --- |
| `EASYAUDIO_PRIORITY_MUSIC` | 100 | Music, which is kept above everything else |
| `EASYAUDIO_PRIORITY_EFFECT` | 1 | Sound effects |
| `EASYAUDIO_PRIORITY_VOICE` | 2 | Voice clips |

## Licence

EasyAudio is released under the MIT licence by Blue Pulse. See [LICENSE](./LICENSE).
