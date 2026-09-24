# NOMNOM Schema

The manifest items consist of two main models:
- Mod
- Artifact

Mod is the main manifest catlog item, which has Artifacts, that are versioned representations of the actual content for the game.

For Example:
- Mod Xyz.ExampleMod
    - Xyz.ExampleMod-1.11.0
    - Xyz.ExampleMod-1.10.1
    - Xyz.ExampleMod-1.9.9

This structure allows forming the following relationships:
- Dependency Chains
- Incompatibility Flags
- Add-On-Mod Relationships (such as Voice Packs for WSO Yappinator, or Tacview Asset Packs for modded content, for example)

---

To contribute your own Mod Manifests, please see [HOW TO CONTRIBUTE MOD MANIFESTS](#how-to-contribute-mod-manifests)

To otherwise contribute to the project, please see [HOW TO CONTRIBUTE ANYTHING ELSE](#how-to-contribute-anything-else)

For a raw overview of the Schema, please check [Validation Schema.](./ValidationSchema.json)

For full detailed overview of the Schema, please continue reading.

## JSON Manifest Properties


> ## `id` <sub>`string` (<ins>Required</ins>)</sub>
> - Unique identifier for your mod.
> - It is recommended to keep this identifier consistent between this property, the file name of the JSON manifest, and the name of your mod's .DLL assembly (not including versioning in the assembly name if desired).
> ```json
> {
>     "id": "com.nikkorap.EditorPlus"
> }
> ```
> 
> > [!TIP]
> > To ensure your mod's identifier is unique, try one of the following:
> > - `<author>.<yourModName>`
> >   - *i.e., `aryx.f22`*
> > - `<topLevel>.<domain>.<yourModName>`
> >   - *i.e., `com.nikkorap.blueprinter`*
> > 
> > If your mod is not a **BepInEx Plugin**, but rather a content or utility add-on, it should conform to this structure instead:
> >
> > - `<parentModID>.<yourModName>`
> >   - *i.e., `NOBlackBox.VanillaTacviewAssetPack`*

> ## `displayName` <sub>`string` (<ins>Required</ins>)</sub>
> - Human-readable name of the mod.
> ```json
> {
>     "displayName": "Collimated HUD"
> }
> ```

> ## `description` <sub>`string` (<ins>Required</ins>)</sub>
> - A brief description of the mod.
> ```json
> {
>     "description": "The craziest mod anyone's ever seen..."
> }
> ```

> ## `tags` <sub>`array[string]`</sub>
> - Relevant tags for your mod.
> ```json
> {
>     "tags": [
>         "QoL",
>         "aircraft",
>         "blueprinter"
>     ]
> }
> ```

> ## `urls` <sub>`array[object{"name": string, "url": string}]` (<ins>Required</ins>)</sub>
> - Array of objects where each object contains `name` and `url`.
> - At least one entry with `"name": "info"` and `"url"` set to a URL is required.
> - Additional entries are optional.
> ```json
> {
>     "urls": [
>         {
>             "name": "info",
>             "url": "https://github.com/clumzy/NO_Tactitools"
>         }
>     ]
> }
> ```

> ## `authors` <sub>`array[string]`</sub>
> - Array of strings containing authors of the mod.
> ```json
> {
>     "authors": [
>         "RehabRocket"
>     ]
> }
> ```

> ## `isClientOrServer` <sub>`"string<"Client"|"Server"|"Both">`</sub>
> - A string describing if the mod is client-side, server-side, or both.
> - The only valid entries are:
>   - `Client`
>   - `Server`
>   - `Both`
> ```json
> {
>     "isClientOrServer": "Client" 
> }
> ```

> ## `autoUpdateArtifacts` <sub>`string<"True"|"False">` (<ins>Required for auto-updating</ins>)</sub>
> ## `githubOwner` <sub>`string` (<ins>Required for auto-updating</ins>)</sub>
> ## `githubRepoName` <sub>`string` (<ins>Required for auto-updating</ins>)</sub>
> - You only need these if you want to set up automatic updating from your repository.
> - `githubOwner` and `githubRepoName` should be the same as in the URL for your repository.
> 
> > [!IMPORTANT]
> > `autoUpdateArtifacts` is a string and not a boolean... you have to set it to `"True"` or `"False"` instead of `true` or `false`.
> 
> ```json
> {
>     "autoUpdateArtifacts": "True",
>     "githubOwner": "clumzy",
>     "githubRepoName": "NO_Tactitools"
> }
> ```

> ## `imageUrl` <sub>`string`</sub>
> ## `imageHash` <sub>`string`</sub>
> - Exact URL for an image that represents the mod. This is used for display purposes only.
> - The image should be in JPEG or PNG format and at most 512x512, though as low as 128x128 should still look fine.
> - The image hash is a SHA256 hash with no prefix, just the raw output as a string.
>
> ```json
> {
>     "imageUrl": "https://github.com/SolarDyn/Assets/blob/main/icon/CollimatedHUD.png",
>     "imageHash": "a50838ad4f1c39eea75fbde2cb0f6769659dc69c25550478eff20a082e3f4d56"
> }
> ```
## Artifact Object Properties

### type

- REQUIRED
- Format: string
- type of Mod. currently considering following types to be supported:
    - plugin: BepInEx Plugin
    - addOn: Add-On or extension for another Mod, such as a voice or texture pack etc.

### fileName

- REQUIRED
- Format: string
- name of the actual downloadable content file. Should be an archive, such as zip, rar, 7z.

### downloadUrl

- REQUIRED
- Format: string url
- download url to the latest release

### gameVersion

- REQUIRED
- Format: Version as string
- This is the latest game version the mod supports e.g. ```"0.32"```

### version

- REQUIRED
- Format: Version as string
- THIS MUST MATCH THE VERSION IN THE DLL IN ITS METADATA IF Artifact Type = MOD

### category

- REQUIRED
- Format: string
- This is the category if the release e.g. Release or Pre-Release
- This is to allow users to optionally download a perhaps Unstable Pre-Release, or get the latest stable version.

### hash

- REQUIRED
- Format: string
- This is the file hash of the github release file the downloadUrl is pointing at
- It can be acquired from the github release's page, looks something like "sha256:very long hex string"
- please include the full value, use the "copy to clipboard" button thats right beside it, and paste it into your manifest

### extends

- REQUIRED IF
    - CATEGORY IS addOn
- Format: Object
```
id : Mod id as string
version : Mod version as string
```
id must be the known Mod id of the mod this extends, as seen in the Manifest

version must be the minimum version this extension is compatible with

### dependencies

- REQUIRED IF
    - MOD HAS DEPENDENCIES
- Format: Array of Objects
```
id : Mod id as string
version : Mod version as string
```
id must be the known Mod id of the mod this depends on, as seen in the Manifest

version must be the minimum version this extension is compatible with

### incompatibilities

- REQUIRED IF
    - MOD IS KNOWN TO BE INCOMPATIBLE WITH OTHER MODS
- Format: Array of Objects
```
id : Mod id as string
version : Mod version as string
```
id must be the known Mod id of the mod this is incompatible with, as seen in the Manifest

version must be the latest known version this mod is incompatible with

## HOW TO CONTRIBUTE MOD MANIFESTS

Before you proceed, please ensure you familiarize yourself with the [manifest structure](#nomnom-schema), including [mod](#mod-object-properties) and [artifact](#artifact-object-properties) object properties.

1. Fork the repository
2. Create your own mod manifest(s) in the modManifests directory, based on the schema described above
3. Submit a Pull Request to ```main``` branch
4. Github Actions Workflow will validate the Schema and Content, then declare the Pull Request allowed to merge if successful
5. A Human will review and approve the merge if no additional issues found

## HOW TO CONTRIBUTE ANYTHING ELSE
1. Fork the repository (check fork all branches)
2. Check out the ```dev``` branch to make sure you are working on the correct branch
3. Submit a Pull Request WITH DETAILED EXPLANATION of your changes to the ```dev``` branch
4. Your Pull Request will be discussed and approved to merge if appropriate
