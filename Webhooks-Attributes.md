Configuring Webhooks is optional but can allow you to receive notifications when certain events happen.

A `webhooks` mapping is in the root of the config file.

Below is a `webhooks` mapping example and the full set of attributes:

```yaml
webhooks:
  error: https://www.myspecialdomain.com/pmm
  run_start:
  run_end:
  changes:
```

| Name | Attribute | Global | Library | Collection |
| :--- | :--- | :---: | :---: | :---: |
| [Error](#error-notifications) | `error` | :heavy_check_mark: | :heavy_check_mark: | :x: |
| [Run Start](#run-start-notifications) | `run_start` | :heavy_check_mark: | :x: | :x: |
| [Run End](#run-end-notifications) | `run_end` | :heavy_check_mark: | :x: | :x: |
| [Collection/Playlist Changes](#changes-notifications) | `changes` | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |

* Each Attribute can be either a webhook url as a string or a comma-separated list of webhooks urls.
* To send notifications to [Notifiarr](https://github.com/meisnate12/Plex-Meta-Manager/wiki/Notifiarr-Attributes) just add `notifiarr` to a webhook instead of the webhook url.

## Error Notifications

The Error notification will be sent whenever an error occurs. The payload that is sent is different Depending on which level the error occurs.

#### Global JSON Payload

```yaml
{
  "error": str,         // Error Message
  "critical": bool      // Critical Error
}
```

#### Library JSON Payload

```yaml
{
  "error": str,         // Error Message
  "critical": bool,     // Critical Error
  "server_name": str,   // Server Name
  "library_name": str   // Library Name
}
```

#### Collection JSON Payload

```yaml
{
  "error": str,         // Error Message
  "critical": bool,     // Critical Error
  "server_name": str,   // Server Name
  "library_name": str,  // Library Name
  "collection": str     // Collection Name
}
```

#### Playlist JSON Payload

```yaml
{
  "error": str,         // Error Message
  "critical": bool,     // Critical Error
  "server_name": str,   // Server Name
  "library_name": str,  // Library Name
  "playlist": str       // Playlist Name
}
```

## Run Start Notifications

The Run Start notification will be sent at the beginning of every run.

#### JSON Payload

```yaml
{
  "start_time": str,            // Time Run is started Format "YY-mm-dd HH:MM:SS"
}
```

## Run End Notifications

The Run End notification will be sent at the end of every run with statistics.

#### JSON Payload

```yaml
{
  "start_time": str,            // Time Run started Format "YY-mm-dd HH:MM:SS"
  "end_time": str,              // Time Run ended Format "YY-mm-dd HH:MM:SS"
  "run_time": str,              // Time Run took to complete Format "HH:MM"
  "collections_created": int,   // Number of Collections/Playlists Created
  "collections_modified": int,  // Number of Collections/Playlists Modified
  "collections_deleted": int,   // Number of Collections/Playlists Removed
  "items_added": int,           // Number of Items added across all Collections/Playlists
  "items_removed": int,         // Number of Items removed across all Collections/Playlists
  "added_to_radarr": int,       // Number of Items added to Radarr
  "added_to_sonarr": int        // Number of Items added to Sonarr
}
```

## Changes Notifications

The Changes Notification will be sent after each collection/playlist containing the following payload if the collection/playlist has been created, has new items, or has had items removed.

#### JSON Payload

```yaml
{
  "server_name": str,           // Server Name
  "library_name": str,          // Library Name
  "collection": str,            // Collection Name only in payload for a collection
  "playlist": str,              // Playlist Name only in payload for a playlist
  "created": bool,              // Was the Collection/Playlist Created on this run
  "deleted": bool,              // Was the Collection/Playlist Deleted on this run
  "poster": str,                // Base64 Encoded Collection/Playlist Poster if no poster_url is found
  "poster_url": str,            // Collection/Playlist Poster URL if avaiable
  "background": str,            // Base64 Encoded Collection/Playlist Background if no poster_url is found
  "background_url": str,        // Collection/Playlist Background URL if avaiable
  "additions": [
    "title": str,               // Title of addition
    "tmdb_id": int              // TMDb ID of addition only appears if it's a Movie
    "tvdb_id": int              // TVDb ID of addition only appears if it's a Show
  ],
  "removals": [
    "title": str,               // Title of removal
    "tmdb_id": int              // TMDb ID of removal only appears if it's a Movie
    "tvdb_id": int              // TVDb ID of removal only appears if it's a Show
  ],
}
```
