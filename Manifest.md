# Freedeck Manifest Documentation

This document outlines all three versions of the Freedeck Manifest format, from newest to oldest.

## Manifest V3

A JSON-based format supporting multiple channels and repository types.

### Channel Types

#### Repository (`type: "repository"`)
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

#### External Repository (`type: "repository_external"`)
```json
{
  "type": "repository_external",
  "title": string,
  "message": string (optional),
  "catalog": string
}
```

#### Release Channel (`type: "release"`)
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

### Example
```json
{
  "channels": {
    "main": {
      "type": "repository",
      "title": "Example Plugin Repository",
      "message": "Example repository message",
      "catalog": {
        "timer": {
          "source": "github:example/timer",
          "title": "Timer Plugin",
          "author": "Example Dev",
          "version": "1.0.0",
          "description": "A simple timer plugin",
          "download": "https://example.com/plugins/timer.Freedeck"
        }
      }
    },
    "external": {
      "type": "repository_external",
      "title": "Example External Repository",
      "catalog": "https://example.com/external-repo.json"
    },
    "releases": {
      "type": "release",
      "description": "Example Release Channel",
      "catalog": [
        {
          "current": true,
          "version": "1.0.0",
          "desc": "Example current release"
        },
        {
          "version": "0.9.0",
          "desc": "Example previous release"
        }
      ]
    }
  }
}
```

## Manifest V2

A multi-line text format (`.fdr.txt`) that contains repository information and multiple plugin entries.

### Structure
```
repositoryName
pluginDownload,!pluginGithubRepo,!pluginName,!pluginAuthor,!pluginVersionNumber,!pluginDescription,!pluginIdentifier
[additional plugins on new lines...]
```

### Fields
1. Repository name (Line 1)
2. Plugin details (Line 2 onwards, each plugin on a new line, fields separated by `,!`):
   - Download URL (no prefix)
   - GitHub repository
   - Plugin name
   - Author
   - Version number
   - Description
   - Identifier

### Example
```
Example Plugin Repository
https://example.com/plugins/timer.Freedeck,!example/timer,!Timer Plugin,!Example Dev,!1.0.0,!A simple timer plugin,!timer
https://example.com/plugins/weather.Freedeck,!example/weather,!Weather Plugin,!Example Dev,!1.0.0,!Shows current weather,!weather
```

## Manifest V1 (deprecated and cannot be used)

A line-based text format (`.idx`) where each line represents a plugin, using `,!` as a separator.

### Structure
Each line follows this format:
```
downloadPath,!githubRepo,!pluginName,!authorName,!versionNumber,!description,!identifier
```

### Fields (separated by `,!`)
1. Download path (relative or absolute)
2. GitHub repository (organization/repo-name)
3. Plugin name
4. Author name
5. Version number
6. Description
7. Plugin identifier

### Example
```
downloads/timer.Freedeck,!example/timer,!Timer Plugin,!Example Dev,!1.0.0,!A simple timer plugin,!timer
downloads/weather.Freedeck,!example/weather,!Weather Plugin,!Example Dev,!1.0.0,!Shows current weather,!weather
```

## Version Comparison

| Feature | V3 | V2 | V1 |
|---------|----|----|-----|
| Format | JSON | .fdr.txt (multi-line) | .idx (line-based) |
| File Plugin | .json | .fdr.txt | .idx |
| Multiple Plugins | Yes | Yes (one per line) | Yes (one per line) |
| Repository Name | Yes | Yes | No |
| Multiple Repositories | Yes | No | No |
| External Repositories | Yes | No | No |
| Release Tracking | Yes | No | No |
| Warning Messages | Yes | No | No |
| Structure | Flexible | Fixed | Fixed |

## Migration Path

### V1 to V2
1. Add repository name as first line
2. Keep plugin entries on separate lines
3. Update paths to full URLs if needed

### V2 to V3
1. Convert to JSON format
2. Add channel structure with `type: "repository"`
3. Transform plugin lines into catalog entries
4. Add any additional metadata or messages
5. Optional: Add additional channels for external repositories or releases
