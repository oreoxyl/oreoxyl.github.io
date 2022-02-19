---
layout: page
title: Spotify Integration
---

<script>
    function initSpotifyRegistration() {
        var array = new Uint32Array(1);
        self.crypto.getRandomValues(array);
        localStorage.setItem('spotify_state', array[0]);
        console.log('Set spotify_state');
    }

    function getSpotifyRegistrationScopes() {
        var checkboxesForScopes = [
            ['spotify-scope-top', 'user-top-read'],
            ['spotify-scope-read', 'user-read-playback-state'],
            ['spotify-scope-modify', 'user-modify-playback-state']
        ];

        var scopes = [];

        for (var i = 0; i < checkboxesForScopes.length; i++) {
            if (document.getElementById(checkboxesForScopes[i][0]).checked) {
                scopes.push(checkboxesForScopes[i][1]);
            }
        }

        return scopes;
    }

    function authorizeSpotify() {
        var errorEl = document.getElementById('spotify-error');
        errorEl.textContent = '';

        var scopes = getSpotifyRegistrationScopes();

        if (scopes.length === 0) {
            errorEl.textContent = 'You need to select at least one option or there would be no reason to connect your Spotify.';
            return;
        }

        var array = new Uint32Array(1);
        self.crypto.getRandomValues(array);
        localStorage.setItem('spotify_state', array[0]);

        var url = ("https://accounts.spotify.com/authorize?client_id=6468b6a5931a4760924d203d9b8a4f44&response_type=code&redirect_uri="
            + encodeURIComponent("https://oreoxyl.github.io/bot/spotify/callback")
            + "&scope=" + encodeURIComponent(scopes.join(' '))
            + "&state=" + array[0]
            + "&show_dialog=true"
        );

        location = url;
    }

    function toggleCheckbox(ancestor, clickEvent) {
        if (clickEvent.target.matches('input,label,a')) return;
        var checkbox = ancestor.querySelector('input[type=checkbox]');
        checkbox.checked = !checkbox.checked;
    }
</script>

<h1 class="error">NOT YET - COMING SOON</h1>

<h2>Review the permissions</h2>

<p>You can always come back later to change these options and relink your Spotify account.</p>

<table class="spotify-settings">
    <thead>
        <tr>
            <th>Selected</th>
            <th>Capability</th>
            <th>Example</th>
        </tr>
    </thead>
    <tbody>
        <tr onclick="toggleCheckbox(this, event)">
            <td><input id="spotify-scope-top" type="checkbox" checked="checked" /></td>
            <td>
                <label for="spotify-scope-top">Get your top played tracks & artists</label>
            </td>
            <td>
                <p>You: <code>ob spotify top artists short</code></p>
                <pre>{%- include bot-handle.html -%}Your top played artists (short term) are: 1. Coldplay 2. SUGR? 3. WILLOW 4. Yeju 5. Joji</pre>
            </td>
        </tr>
        <tr onclick="toggleCheckbox(this, event)">
            <td><input id="spotify-scope-read" type="checkbox" checked="checked" /></td>
            <td>
                <label for="spotify-scope-read">Get your currently playing track and player state</label>
            </td>
            <td>
                <p>You: <code>ob spotify</code></p>
                <pre>{%- include bot-handle.html -%}Currently on your spotify ▶ Strange by Celeste [00:42/04:16] → <a href="https://open.spotify.com/track/7sq2z9oX2S0CvgTqCZ0ko4" target="_blank">https://open.spotify.com/track/7sq2z9oX2S0CvgTqCZ0ko4</a></pre>
            </td>
        </tr>
        <tr onclick="toggleCheckbox(this, event)">
            <td><input id="spotify-scope-modify" type="checkbox" /></td>
            <td>
                <label for="spotify-scope-modify">Modify your player state (Spotify Premium required)</label>
            </td>
            <td>
                <p>You: <code>ob spotify yoink</code></p>
                <pre>{%- include bot-handle.html -%}yoinked last shared track in this channel: Never Gonna Give You Up by Rick Astley → <a href="https://open.spotify.com/track/4cOdK2wGLETKBW3PvgPWqT" target="_blank">https://open.spotify.com/track/4cOdK2wGLETKBW3PvgPWqT</a></pre>
            </td>
        </tr>
    </tbody>
</table>

<button onclick="authorizeSpotify()" class="spotify-button"><span class="spotify-icon"></span> Sign up with Spotify</button>

<p class="error" id="spotify-error" style="text-align: center;"></p>

<h3>FAQ</h3>

<details>
    <summary>How do I disconnect my Spotify account?</summary>
    <p>You can always revoke the bot's access to your Spotify account:</p>
    <ol>
        <li>Send <code>ob spotify unregister</code> in an offline Twitch channel that it's in.</li>
        <li>Go to Spotify's website → Account → Apps, and remove "OreoxylBot".</li>
    </ol>
</details>

<details>
    <summary>Who will be able to access my Spotify data?</summary>
    <p>Only you will be able to use the bot's commands to share what you're currently listening to and more.</p>
    <p>The bot doesn't let anyone check your Spotify status. If that ever becomes an option it will require your explicit permission.</p>
</details>

<details>
    <summary>How can I change the permissions after I signed up?</summary>
    <p>To add permissions to the bot you can sign up again with the ones you want.</p>
    <p>To remove permissions, like any third party Spotify app, you have to first unlink your Spotify account by going to Spotify's website → Account → Apps, and removing "OreoxylBot". Then you can get back here and sign up again with the permissions you want.</p>
</details>

<details>
    <summary>I have more questions</summary>
    <p>You can ask in an offline Twitch channel that I'm in.</p>
</details>
