<template>
  <div>
    <router-view></router-view>
  </div>
</template>

<style>
body {
  margin: 0 !important;
}
.icon {
  font-size: 12px;
  cursor: pointer;
}

@font-face {
  font-family: 'icomoon';
  src:  url('../assets/icomoon/icomoon.eot?xn63nf');
  src:  url('../assets/icomoon/icomoon.eot?xn63nf#iefix') format('embedded-opentype'),
    url('../assets/icomoon/icomoon.ttf?xn63nf') format('truetype'),
    url('../assets/icomoon/icomoon.woff?xn63nf') format('woff'),
    url('../assets/icomoon/icomoon.svg?xn63nf#icomoon') format('svg');
  font-weight: normal;
  font-style: normal;
  font-display: block;
}

[class^="icon-"], [class*=" icon-"] {
  /* use !important to prevent issues with browser extensions that change fonts */
  font-family: 'icomoon' !important;
  speak: never;
  font-style: normal;
  font-weight: normal;
  font-variant: normal;
  text-transform: none;
  line-height: 1;

  /* Better Font Rendering =========== */
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

.icon-note-add:before {
  content: "\e902";
}
.icon-note-add1:before {
  content: "\e903";
}
.icon-folder-add:before {
  content: "\e904";
}
.icon-folder-add1:before {
  content: "\e905";
}
.icon-folder-open-o:before {
  content: "\e900";
}
.icon-folder-o:before {
  content: "\e901";
}
.icon-home:before {
  content: "\e906";
}
.icon-bin:before {
  content: "\e9ac";
}
.icon-arrow-left2:before {
  content: "\ea40";
}
</style>
