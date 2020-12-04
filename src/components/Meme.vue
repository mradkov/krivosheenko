<template>
  <div class="hello">
    <canvas
      ref="canvas"
      id="canvas"
      class="bg"
      width="500"
      height="500"
    ></canvas>
  </div>
</template>

<script>
// import { shenko, santa, thug, thugSanta } from '@/data';
import { shenko, santa, thug, thugSanta } from '@/data';
export default {
  name: 'Meme',
  mounted() {
    const text = { full: '', config: shenko, topText: '', bottomText: '' };
    const configUrl = new URL(window.location).pathname.split('/');
    if (configUrl.length == 3) {
      text.full = decodeURI(configUrl[2]);
      text.config =
        configUrl[1] == 'santa'
          ? santa
          : configUrl[1] == 'gangsta'
          ? thug
          : shenko;
    } else if (configUrl.length == 4) {
      text.full = decodeURI(configUrl[3]);
      const test = [configUrl[1], configUrl[2]]
        .map(x => x.toLowerCase())
        .filter(x => {
          if (x == 'santa') {
            return true;
          }
          if (x == 'gangsta') {
            return true;
          }
        });

      text.config =
        test.length == 2
          ? thugSanta
          : test.length == 1
          ? test[0] == 'santa'
            ? santa
            : test[0] == 'gangsta'
            ? thug
            : shenko
          : shenko;

      console.log('get', test);
    } else {
      text.full = decodeURI(configUrl[1]);
    }

    text.topText = text.full.split('&')[0] || ''.toUpperCase();
    text.bottomText = text.full.split('&')[1] || ''.toUpperCase();

    console.log(configUrl);
    console.log(new URL(window.location));

    let image = new Image();
    image.src = text.config;

    console.log(text);

    setTimeout(function() {
      let canvas = document.getElementById('canvas');
      let ctx = canvas.getContext('2d');

      ctx.drawImage(image, 0, 0);
      canvas.width = image.width;
      canvas.height = image.height;
      ctx.clearRect(0, 0, canvas.height, canvas.width);
      ctx.drawImage(image, 0, 0);
      let e = (((canvas.width + canvas.height) / 2) * 4) / 100;
      ctx.font = e + 'pt sans-serif';
      ctx.textAlign = 'center';
      ctx.textBaseline = 'top';
      ctx.lineWidth = e / 5;
      ctx.strokeStyle = 'black';
      ctx.fillStyle = 'white';
      ctx.lineJoin = 'round';
      ctx.strokeText(text.topText, canvas.width / 2, 0.05 * canvas.height);
      ctx.fillText(text.topText, canvas.width / 2, 0.05 * canvas.height);
      ctx.strokeText(text.bottomText, canvas.width / 2, 0.9 * canvas.height);
      ctx.fillText(text.bottomText, canvas.width / 2, 0.9 * canvas.height);
    }, 1);
  },
};
</script>

<style lang="css" scoped>
.bg {
  width: 512px;
  max-width: 100%;
}
</style>
