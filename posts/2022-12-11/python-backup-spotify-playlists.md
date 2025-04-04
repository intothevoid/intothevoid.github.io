---
title: "Spotify Playlist Backups using Python"
description: "Backup Spotify Playlists using Python and Javascript"
tags : ["python", "javascript", "automation", "webapp"]
date: 2022-12-11T11:43:47+10:30
featured_image: "/images/python.jpg"
---

To create a web application that backs up your Spotify playlists as a JSON file, you will need to do the following:

1.  First, you will need to install the `spotipy` library, which provides a Python interface for the Spotify Web API. You can do this by running the following command:
```bash
pip install spotipy 
```
2. Next, you will need to create a Spotify app and obtain a client ID and client secret for the app. You can do this by logging in to the [Spotify Developer Dashboard](https://developer.spotify.com/dashboard) and following the instructions on the website.

## Creating the Python Backend
Once you have obtained your client ID and client secret, you can use the `spotipy.Spotify` class to authenticate yourself and access the Spotify Web API. An example of how you can do this is shown below:

```python
import spotipy

client_id = "your-client-id"
client_secret = "your-client-secret"

spotify = spotipy.Spotify(
    client_id=client_id,
    client_secret=client_secret
)
```

After authenticating, you can use the `spotify.user_playlists()` method to retrieve a list of all your Spotify playlists. This method returns a paginated list of playlists, so you will need to iterate through the pages and collect the playlists from each page. Here is an example of how you can do this:

```python
playlists = []

response = spotify.user_playlists()
playlists.extend(response["items"])

while response["next"]:
    response = spotify.next(response)
    playlists.extend(response["items"])

```

Once you have collected all your playlists, you can use the `json` module to convert the list of playlists to a JSON string, and then write the JSON string to a file. Here is an example of how you can do this:

```python
import json

with open("playlists.json", "w") as file:
    json.dump(playlists, file)

```

Finally, you can create a simple Flask app that exposes a `/backup` endpoint, which accepts `POST` requests and uses the code from the previous steps to back up your playlists as a JSON file. Here is an example of how you can do this:

```python
from flask import Flask, request
import spotipy
import json

app = Flask(__name__)

client_id = "your-client-id"
client_secret = "your-client-secret"
spotify = spotipy.Spotify(
    client_id=client_id,
    client_secret=client_secret
)

@app.route("/backup", methods=["POST"])
def backup():
    playlists = []

    response = spotify.user_playlists()
    playlists.extend(response["items"])

    while response["next"]:
        response = spotify.next(response)
        playlists.extend(response["items"])

    with open("playlists.json", "w") as file:
        json.dump(playlists, file)

    return "Playlists backed up successfully!"

```

To run the Flask app, you will need to first start the Flask development server by running the following command:

```bash
export FLASK_APP=app.py
flask run

```

This will start the Flask development server and make your app available at `http://localhost:5000`. To make a `POST` request to the `/backup` endpoint, you can use a tool like `curl` or `Postman` to send a `POST` request to `http://localhost:5000/backup`.

Here is an example of how you can use `curl` to make a `POST` request:
```bash
curl -X POST http://localhost:5000/backup

```

Once you have made the request, your playlists should be backed up as a JSON file named `playlists.json` in the current directory.

Note: This code is provided for illustration purposes only, and is not intended for production use. It does not include error handling or other best practices for building a Flask app.

## Creating the Web Frontend
Here's how you could create a web page to create a nice frontend for the Python program that backs up your Spotify playlists:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Spotify Playlist Backup</title>
    <style>
        /* This is some CSS that styles the page */
        body {
            font-family: sans-serif;
            text-align: center;
        }
        input, button {
            padding: 10px 20px;
            font-size: 16px;
        }
        input {
            width: 300px;
        }
        button {
            background-color: #1DB954;
            color: white;
            cursor: pointer;
        }
        pre {
            text-align: left;
            margin: 20px;
            padding: 20px;
            border: 1px solid #ccc;
            background-color: #f1f1f1;
        }
    </style>
</head>
<body>
    <h1>Spotify Playlist Backup</h1>
    <p>Enter your Spotify credentials and username below to backup your playlists as a JSON file.</p>
    <form>
        <input type="text" id="client_id" placeholder="Your Spotify client ID">
        <input type="text" id="client_secret" placeholder="Your Spotify client secret">
        <input type="text" id="username" placeholder="Your Spotify username">
        <button type="submit">Backup Playlists</button>
    </form>
    <pre id="output"></pre>
    <script>
        // This is the JavaScript code that runs when the page is loaded

        // This gets the form element
        const form = document.querySelector("form");

        // This adds an event listener that runs when the form is submitted
        form.addEventListener("submit", async (e) => {
            // This prevents the page from reloading
            e.preventDefault();

            // This gets the input elements
            const clientIdInput = document.querySelector("#client_id");
            const clientSecretInput = document.querySelector("#client_secret");
            const usernameInput = document.querySelector("#username");

            // This gets the values from the input elements
            const clientId = clientIdInput.value;
            const clientSecret = clientSecretInput.value;
            const username = usernameInput.value;

            // This shows a message while the playlists are being backed up
            const output = document.querySelector("#output");
            output.innerHTML = "Backing up your playlists... please wait.";

            // This sends a request to the server to backup the playlists
            const response = await fetch("/backup", {
                method: "POST",
                headers: {
                    "Content-Type": "application/json",
                },
                body: JSON.stringify({
                    clientId,
                    clientSecret,
                    username,
                }),
            });

            // This gets the response from the server
            const data = await response.json();

            // This displays the response from the server
            output.innerHTML = JSON.stringify(data, null, 4);
        });
    </script>
</body>

```

## Spotify backup playlist as a standalone script

```python
import json
import spotipy
from spotipy.oauth2 import SpotifyClientCredentials

# This is your Spotify client ID and secret
client_id = "YOUR_CLIENT_ID"
client_secret = "YOUR_CLIENT_SECRET"

# This is your Spotify username
username = "YOUR_USERNAME"

# This is the path to the JSON file where your playlists will be saved
json_file = "playlists.json"

# This creates a Spotify client using your client ID and secret
client_credentials_manager = SpotifyClientCredentials(client_id=client_id, client_secret=client_secret)
spotify = spotipy.Spotify(client_credentials_manager=client_credentials_manager)

# This gets your user ID
user_id = spotify.current_user()["id"]

# This gets all your playlists
playlists = spotify.user_playlists(user_id)

# This creates an empty list where your playlists will be saved
playlists_data = []

# This iterates over your playlists
for playlist in playlists["items"]:
    # This gets the playlist ID and name
    playlist_id = playlist["id"]
    playlist_name = playlist["name"]

    # This gets the tracks in the playlist
    tracks = spotify.user_playlist_tracks(user_id, playlist_id)

    # This creates an empty list where the tracks will be saved
    tracks_data = []

    # This iterates over the tracks
    for track in tracks["items"]:
        # This gets the track data
        track_data = track["track"]

        # This saves the track data
        tracks_data.append({
            "id": track_data["id"],
            "name": track_data["name"],
            "artists": [artist["name"] for artist in track_data["artists"]],
            "album": track_data["album"]["name"],
        })

	# This saves the playlist data
	playlists_data.append({
	    "id": playlist_id,
	    "name": playlist_name,
	    "tracks": tracks_data,
	})

# This saves the playlists data to the JSON file
with open(json_file, "w") as f: 
	json.dump(playlists_data, f, indent=4)

print("Successfully backed up your playlists to", json_file)
```

In this code, we use the `spotipy` library to access the Spotify API and retrieve the data for your playlists and tracks. We then save this data to a JSON file using the `json` library.

To use this program, you will need to replace the `client_id`, `client_secret`, and `username` variables with your own Spotify credentials and username. You will also need to specify the path to the JSON file where you want your playlists to be saved.

Once you have done this, you can run the program and it will retrieve your playlists and tracks and save them to the JSON file. You can then use this file to backup your playlists or to transfer them to another Spotify account.
