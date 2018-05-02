# Initializing WebWorldWind

The `WorldWindow` provides the main entry point to interacting with a WorldWind globe.

1. Add a `<canvas>` element to your webpage and assign a unique id.

2. In your javascript application file create a new WorldWindow with the canvas id as the argument.

<div class="demo" id="initialization">
</div>

<script>
    const initGistUrl = 'https://gist.githubusercontent.com/zglueck/38a9e5fff426e11b8ef2dd1abcdac5fe/raw/6586a7b34c324de9321f04aef0f4af98ea89764c/main.js'

    const htmlGistUrl = 'https://gist.githubusercontent.com/zglueck/be21a881bdc09c80b99b360b064592ff/raw/a3b02dd9fe43d458686de5901fe832c597634635/index.html'

    const cssGistUrl = 'https://gist.githubusercontent.com/zglueck/be21a881bdc09c80b99b360b064592ff/raw/a3b02dd9fe43d458686de5901fe832c597634635/theme.css'

    const createJsFiddle = gists => {
        const form = new FormData();

        for (let i = 0; i < gists.length; i++) {
            if (gists[i].url.endsWith('js')) {
                form.append('js', response.text());
            } else if (gists[i].url.endsWith('html')) {
                form.append('html', response.text());
            } else if (gists[i].url.endsWidth('css')) {
                form.append('css', response.text());
            }
        }

        form.append('title', 'dynamic demo test');
        form.append('wrap', 'd');

        return fetch('https://jsfiddle.net/api/post/library/pure/, {
            method: 'POST',
            body: form
        });
    };

    const updatePage = url => {
        const demoDiv = document.getElementById('initialization');

        const demoIframe = document.createElement('iframe');
        demoIframe.setAttribute('src', url);

        demoDiv.appendChild(demoIframe);
    };

    const initGistPromise = fetch(initGistUrl);
    const htmlGistPromise = fetch(htmlGistUrl);
    const cssGistPromise = fetch(cssGistUrl);

    Promise.all([initGistPromise, htmlGistPromise, cssGistPromise])
        .then(createJsFiddle)
        .then(updatePage);
</script>