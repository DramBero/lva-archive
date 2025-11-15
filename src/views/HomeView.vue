<script setup lang="ts">
import { onMounted, ref } from 'vue'
import xbbcode from 'xbbcode-parser'
import 'xbbcode-parser/xbbcode.css'

const html = ref<string>()
const textContent = ref<string>()

onMounted(async () => {
  try {
    const res = await fetch('/forum-source/ustav-bagwell.txt')
    if (!res.ok) throw new Error(`HTTP ${res.status}`)
    textContent.value = await res.text()
    textContent.value = textContent.value.replace(/\r?\n/g, 'BREAKLINE')
    const result = xbbcode.process({
      text: textContent.value,
    })
    html.value = result.html
    html.value = html.value.replace(/BREAKLINE/g, '<br>')
    /*     bbCodeParser.setCodes({
      '\\[b\\](.+?)\\[/b\\]': '<strong>$1</strong>',
      '\\[i\\](.+?)\\[/i\\]': '<em>$1</em>',
      '\\[u\\](.+?)\\[/u\\]': '<u>$1</u>',

      '\\[h1\\](.+?)\\[/h1\\]': '<h1>$1</h1>',
      '\\[h2\\](.+?)\\[/h2\\]': '<h2>$1</h2>',
      '\\[h3\\](.+?)\\[/h3\\]': '<h3>$1</h3>',
      '\\[h4\\](.+?)\\[/h4\\]': '<h4>$1</h4>',
      '\\[h5\\](.+?)\\[/h5\\]': '<h5>$1</h5>',
      '\\[h6\\](.+?)\\[/h6\\]': '<h6>$1</h6>',

      '\\[center\\](.+?)\\[/center\\]': '<p style="text-align: center;">$1</p><br>',

      '\\[p\\](.+?)\\[/p\\]': '<p>$1</p>',
      '\\[code\\](.+?)\\[/code\\]': '<pre>$1</pre>',

      '\\[color=(.+?)\\](.+?)\\[/color\\]': '<span style="color:$1">$2</span>',
      '\\[size=([0-9]+)\\](.+?)\\[/size\\]': '<span style="font-size:$1px">$2</span><br>',

      '\\[img\\](.+?)\\[/img\\]': '<img src="$1">',
      '\\[img=(.+?)\\]': '<img src="$1">',

      '\\[email\\](.+?)\\[/email\\]': '<a href="mailto:$1">$1</a>',
      '\\[email=(.+?)\\](.+?)\\[/email\\]': '<a href="mailto:$1">$2</a>',

      '\\[url\\](.+?)\\[/url\\]': '<a href="$1">$1</a>',
      '\\[url=(.+?)\\|onclick\\](.+?)\\[/url\\]': '<a onclick="$1">$2</a>',
      '\\[url=(.+?)\\starget=(.+?)\\](.+?)\\[/url\\]': '<a href="$1" target="$2">$3</a>',
      '\\[url=(.+?)\\](.+?)\\[/url\\]': '<a href="$1">$2</a>',

      '\\[a=(.+?)\\](.+?)\\[/a\\]': '<a href="$1" name="$1">$2</a>',

      '\\[list\\](.+?)\\[/list\\]': '<ul>$1</ul>',
      '\\[\\*\\](.+?)\\[/\\*\\]': '<li>$1</li>',
      '\\[br\\]': '<br>',
      '\r?\n': '<br>',
    }) */
    // html.value = bbCodeParser.parse(textContent.value)
  } catch (err) {
    console.error('Failed to load text file:', err)
  }
})
</script>

<template>
  <main class="main">
    <div v-html="html" class="forum-wrapper"></div>
  </main>
</template>

<style>
.main {
  margin: 0 auto;
  width: 100%;
}

.forum-wrapper {
  max-width: 900px;
  margin: auto;
}

img {
  max-width: 900px;
}
</style>
