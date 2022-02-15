---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

title: About
layout: home
---

<div id="main-page" style="text-align: center;">
    <div>
        <img class="wiggle" alt="OreoxylBot" style="display: block; height: 7.5em; margin: 0 auto 2em;" src="{{ " /assets/logo.png" | relative }}" />
    </div>
    <h2>Top Secret Twitch Chat Bot</h2>
</div>

<script>
    var lol = (function () {
        var status = 0;
        var container = document.getElementById('main-page');
        var dontClickEl = null;

        return function _lol() {
            var message = '';

            switch (status) {
                case 0:
                    message = 'Available in select channels';
                    break;

                case 2:
                    message = 'Artwork acquired from a professional Painter';
                    break;

                case 3:
                    message = "Isn't still running on a laptop";
                    break;

                case 4:
                    message = 'Fully functional and has zero bugs';
                    break;

                case 5:
                    message = 'What are you still doing here?';
                    break;

                case 6:
                    message = "Whatever you do, don't click the line above the line above 🎁 🙃";
                    dontClickEl.style.cursor = 'pointer';
                    dontClickEl.addEventListener('click', function () {
                        var imgs = document.querySelectorAll('img');
                        var els = document.querySelectorAll('h2,h3,a,header');

                        document.getElementsByTagName('body')[0].style.overflow = 'hidden';

                        for (var i = 0; i < imgs.length; i++) {
                            imgs[i].classList.add('barrel-roll');
                        }
                        
                        for (var i = 0; i < els.length; i++) {
                            els[i].style.animationDelay = Math.floor(Math.random() * 500) + 'ms';
                            els[i].classList.add(
                                'shake-constant',
                                Math.random() < 0.333 ? 'shake-chunk'
                                    : (Math.random() < 0.5 ? 'shake-slow' : 'shake')
                            );
                        }

                        setTimeout(function () {
                            for (var i = 0; i < imgs.length; i++) {
                                imgs[i].classList.remove('barrel-roll');
                            }

                            for (var i = 0; i < els.length; i++) {
                                els[i].classList.remove('shake-constant', 'shake-chunk', 'shake-slow', 'shake');
                            }

                            document.getElementsByTagName('body')[0].style.overflow = null;
                        }, 3000);

                    });
                    break;

                case 7:
                    dontClickEl.classList.add('shake-little', 'shake-constant', 'shake-constant--hover');
                    break;
            }

            if (message.length > 0) {
                var el = document.createElement('h3');
                el.textContent = message;
                container.appendChild(el);

                if (status === 4) {
                    dontClickEl = el;
                }
            }

            status++;

            if (status < 8) {
                setTimeout(_lol, 2000);
            }
        }
    })();

    window.onload = setTimeout(lol, 2000);
</script>