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

<h2>1. Select permissions</h2>

<table class="spotify-settings">
<thead>
<tr>
    <th>Allow</th>
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
        <pre>{%- include bot-handle.html -%}Currently on your spotify: Never Gonna Give You Up by Rick ▶ 00:50 {spotify link}</pre>
    </td>
</tr>
<tr onclick="toggleCheckbox(this, event)">
    <td><input id="spotify-scope-modify" type="checkbox" /></td>
    <td>
      <label for="spotify-scope-modify">Modify your player state (pause, play...)</label>
      </td>
    <td>
        <p>You: <code>ob spotify yoink</code></p>
        <pre>{%- include bot-handle.html -%}Yoinked last shared track in this channel: Never Gonna Give You Up by Rick Astley <a href="https://open.spotify.com/track/4cOdK2wGLETKBW3PvgPWqT" target="_blank">https://open.spotify.com/track/4cOdK2wGLETKBW3PvgPWqT</a></pre>
    </td>
</tr>
</tbody>
</table>

<p>You can change these options later and relink your Spotify account.</p>

<h2>2. Sign up with Spotify</h2>

<p></p>

<button onclick="authorizeSpotify()" class="spotify-button"><span class="spotify-icon"></span> Sign up with Spotify</button>

<p class="error" id="spotify-error"></p>

<p>Remember, after completing the registration you can always revoke the app's access to your Spotify account by doing <code>ob spotify unregister</code> and removing the App from your settings on Spotify's website.</p>

<h2>3. Whisper the bot your sign up code</h2>

<p>You'll get it after the sign up.</p>