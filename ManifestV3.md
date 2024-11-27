# Freedeck Manifest V3 Documentation

The Freedeck Manifest V3 is a JSON specification that defines different types of channels for managing repositories, external repositories, and releases. This document outlines the structure and components of the manifest.

## Manifest Structure

The root of the manifest contains a single `channels` object that holds multiple channel entries. Each channel is identified by a unique ID and contains specific properties based on its type.

## Channel Types

### Repository Channel (`type: "repository"`)

A repository channel defines a local plugin repository.

```json
{
  "type": "repository",
  "title": string,
  "message": string (optional),
  "catalog": {
    [pluginId: string]: {
      "message": string (optional),
      "source": string,
      "author": string,
      "title": string,
      "description": string,
      "version": string,
      "download": string
    }
  }
}
```

#### Properties:
- `title`: Display name of the repository
- `message`: Optional warning or informational message about the repository
- `catalog`: Object containing plugin entries
  - Each plugin entry contains:
    - `message`: Optional warning or informational message about the plugin
    - `source`: GitHub repository reference in format "github:username/repository"
    - `author`: Plugin author's name
    - `title`: Plugin display name
    - `description`: Brief description of the plugin
    - `version`: Plugin version string
    - `download`: Direct download URL for the plugin file

### External Repository Channel (`type: "repository_external"`)

An external repository channel references a repository hosted elsewhere.

```json
{
  "type": "repository_external",
  "title": string,
  "message": string (optional),
  "catalog": string
}
```

#### Properties:
- `title`: Display name of the external repository
- `message`: Optional warning or informational message
- `catalog`: URL pointing to an external repository manifest file

### Release Channel (`type: "release"`)

A release channel tracks different versions of Freedeck.

```json
{
  "type": "release",
  "description": string,
  "catalog": [
    {
      "current": boolean (optional),
      "version": string,
      "desc": string
    }
  ]
}
```

#### Properties:
- `description`: Description of the release channel
- `catalog`: Array of release entries
  - Each release entry contains:
    - `current`: Optional boolean indicating if this is the current release
    - `version`: Version string of the release
    - `desc`: Description of the release

## Example Usage

The manifest supports multiple channels of different types simultaneously. Here's a minimal example:

```json
{
  "channels": {
    "main-repo": {
      "type": "repository",
      "title": "Main Plugin Repository",
      "catalog": {
        "plugin-1": {
          "source": "github:developer/plugin",
          "author": "Developer Name",
          "title": "Plugin Name",
          "description": "Plugin description",
          "version": "1.0.0",
          "download": "https://example.com/download/plugin.freedeck"
        }
      }
    }
  }
}
```

## Version Information

This is version 3 of the Freedeck Manifest specification. Ensure your implementation follows this version's structure and requirements.
