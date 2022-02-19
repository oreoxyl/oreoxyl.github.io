---
layout: page
title: Spotify Integration - Almost There
---

<h1></h1>
<h2>Whisper the bot your sign up code</h2>

<script>
// yea yea...

function parse_query_string(query) {
  var vars = query.split("&");
  var query_string = {};
  for (var i = 0; i < vars.length; i++) {
    var pair = vars[i].split("=");
    var key = decodeURIComponent(pair[0]);
    var value = decodeURIComponent(pair[1]);
    // If first entry with this name
    if (typeof query_string[key] === "undefined") {
      query_string[key] = decodeURIComponent(value);
      // If second entry with this name
    } else if (typeof query_string[key] === "string") {
      var arr = [query_string[key], decodeURIComponent(value)];
      query_string[key] = arr;
      // If third or later entry with this name
    } else {
      query_string[key].push(decodeURIComponent(value));
    }
  }
  return query_string;
}


window.addEventListener('load', function() {
    var query = window.location.search.substring(1);
    var qs = parse_query_string(query);
    var res = document.getElementById('spotify-code');
    res.textContent = '';

    var p;
    
    if (qs.state !== localStorage.getItem('spotify_state')) {

        p = document.createElement('p');
        p.className = 'error';
        p.textContent = 'Failed security verification.';
        res.appendChild(p);

        p = document.createElement('p');
        p.textContent = 'It looks like you didn\'t get here through the sign up page (it\'s possible you refreshed, used a different browser or clicked on a malicious link), please start over.';

    } else if (qs.error) {

        p = document.createElement('p');
        p.className = 'error';
        p.textContent = 'Spotify Error: ' + qs.error;

    } else {

        p = document.createElement('p');
        p.textContent = 'To complete the sign up whisper OreoxylBot on Twitch the following code in the next few minutes. Do not share this code with anyone else.';
        res.appendChild(p);

        p = document.createElement('p');
        p.textContent = "It can't whisper back due to current Twitch limits so it will answer in your own channel's chat.";
        res.appendChild(p);

        p = document.createElement('p');
        p.textContent = 'Click on the code to automatically copy it:';
        res.appendChild(p);

        p = document.createElement('textarea');
        p.setAttribute('readonly', '');
        p.setAttribute('spellcheck', 'false');
        p.textContent = 'spotify-signup ' + qs.code;
        p.onclick = function() {
          this.select();
          document.execCommand('copy');
          console.log('copied');
        };

        localStorage.setItem('spotify_state', null);
    }

    res.appendChild(p);
});

</script>


<div id="spotify-code">Processing... (You're not supposed to be seeing this)</div>
