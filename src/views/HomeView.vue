<script setup lang="ts">
import { onMounted, ref, watch } from 'vue'
import xbbcode from 'xbbcode-parser'
import 'xbbcode-parser/xbbcode.css'

const html = ref<string>()
const textContent = ref<string>()

const files01 = [
  {
    title: '«Зона 51» FAQ новобранцам  Безопасность вашего аккаунта',
    path: '/forum-source/01/faq.txt',
  },
  {
    title: 'FAQ по вступлению в армию',
    path: '/forum-source/01/faq-newbies.txt',
  },
  {
    title: 'Благотворительный фонд',
    path: '/forum-source/01/fund.txt',
  },
  {
    title: 'Взвод обеспечения',
    path: '/forum-source/01/vo.txt',
  },
  {
    title: 'Воинские медали и ачивки',
    path: '/forum-source/01/medals.txt',
  },
  {
    title: 'Дембельский альбом',
    path: '/forum-source/01/albom.txt',
  },
  {
    title: 'Дни рождения игроков 01',
    path: '/forum-source/01/01-birthdays.txt',
  },
  {
    title: 'Книга жалоб',
    path: '/forum-source/01/plak-plak.txt',
  },
  {
    title: 'Комендатура',
    path: '/forum-source/01/comendante.txt',
  },
  {
    title: 'Контрактная служба',
    path: '/forum-source/01/contract.txt',
  },
  {
    title: 'Новости армии [NEW 14.01.18]',
    path: '/forum-source/01/news.txt',
  },
  {
    title: 'Ночное дежурство',
    path: '/forum-source/01/night-shift.txt',
  },
  {
    title: 'ОСН Рысь',
    path: '/forum-source/01/ris.txt',
  },
  {
    title: 'Отпуск военнослужащих',
    path: '/forum-source/01/vacation.txt',
  },
  {
    title: 'Охранная рота',
    path: '/forum-source/01/bomzhy.txt',
  },
  {
    title: 'Предложения по улучшению армии',
    path: '/forum-source/01/ideas.txt',
  },
  {
    title: 'СВСС',
    path: '/forum-source/01/portovye-kurtizanki.txt',
  },
  {
    title: 'СОБР',
    path: '/forum-source/01/sobr.txt',
  },
  {
    title: 'Состав армии',
    path: '/forum-source/01/sostav.txt',
  },
  {
    title: 'Стальной берет (Keller-Rainer)',
    path: '/forum-source/01/steel.txt',
  },
  {
    title: 'Строевой устав армии',
    path: '/forum-source/01/stroevoy.txt',
  },
  {
    title: 'Тест молодого бойца (ТМБ)',
    path: '/forum-source/01/tmb.txt',
  },
  {
    title: 'ТЧ',
    path: '/forum-source/01/t4.txt',
  },
  {
    title: 'Устав армии',
    path: '/forum-source/01/ustav.txt',
  },
  {
    title: 'Черный список и выход из него',
    path: '/forum-source/01/4s.txt',
  },
  {
    title: 'Школа прапорщиков (Mad-Keller-Rainer)',
    path: '/forum-source/01/prapory.txt',
  },
  {
    title: 'Штрафное отделение (Mad-Rainer)',
    path: '/forum-source/01/penalty.txt',
  },
]

const fileUrl = ref<string>('/forum-source/01/ustav.txt')

async function showFile() {
  try {
    const res = await fetch(fileUrl.value)
    if (!res.ok) throw new Error(`HTTP ${res.status}`)
    textContent.value = await res.text()
    textContent.value = textContent.value.replace(/\r?\n/g, 'BREAKLINE')
    const result = xbbcode.process({
      text: textContent.value,
    })
    html.value = result.html || ''
    html.value = html.value?.replace(/BREAKLINE/g, '<br>')
  } catch (err) {
    console.error('Failed to load text file:', err)
  }
}

const showSidebar = ref(false)

watch(
  fileUrl,
  () => {
    showFile()
  },
  {
    immediate: true,
  },
)
</script>

<template>
  <main class="main">
    <div class="header-mobile">
      <button @click="showSidebar = !showSidebar">
        {{ showSidebar ? 'CLOSE' : 'MENU' }}
      </button>
    </div>
    <div class="sidebar sidebar-mobile" v-if="showSidebar">
      01 server:
      <div v-for="file in files01" :key="file.path" class="sidebar-link">
        <button
          class="sidebar-button"
          :class="{ 'sidebar-button_active': file.path === fileUrl }"
          @click="
            () => {
              fileUrl = file.path
              showSidebar = false
            }
          "
        >
          {{ file.title }}
        </button>
      </div>
    </div>
    <div class="sidebar">
      01 server:
      <div v-for="file in files01" :key="file.path" class="sidebar-link">
        <button
          class="sidebar-button"
          :class="{ 'sidebar-button_active': file.path === fileUrl }"
          @click="fileUrl = file.path"
        >
          {{ file.title }}
        </button>
      </div>
    </div>
    <div v-html="html" class="forum-wrapper"></div>
  </main>
</template>

<style>
.main {
  margin: 0 auto;
  width: 100%;
  display: flex;
  gap: 10px;
  justify-content: space-between;
  position: relative;
  height: 100dvh;
  overflow-y: auto;
}

.forum-wrapper {
  max-width: 900px;
  margin: 20px auto;
  width: 100dvw;
}

img {
  max-width: 95dvw;
}

.sidebar {
  display: flex;
  flex-direction: column;
  position: sticky;
  top: 0;
  padding: 15px;
  background-color: rgba(255, 255, 255, 0.1);
  overflow-y: auto;
  max-width: 400px;
  gap: 1px;
}

.sidebar-mobile {
  display: none;
  position: fixed;
  top: 0;
  left: 0;
  width: 100dvw;
  height: 100dvh;
  background-color: rgba(30, 30, 30, 1);
}

.header-mobile {
  display: none;
  position: fixed;
  top: 10px;
  right: 10px;
  z-index: 100;
}

.header-mobile button {
  padding: 5px;
}

@media (max-width: 1000px) {
  .sidebar {
    display: none;
  }
  .sidebar-mobile {
    display: flex;
  }
  .forum-wrapper {
    padding: 10px;
  }
  .header-mobile {
    display: block;
  }
}

.sidebar-button {
  font-size: 18px;
  text-align: left;
  width: 100%;
  background-color: rgba(255, 255, 255, 0.1);
  color: white;
  border: none;
  outline: none;
  cursor: pointer;
  padding: 5px;
  &:hover {
    background-color: rgba(255, 255, 255, 0.2);
  }
}

.sidebar-button_active {
  background-color: aquamarine;
  color: black;
  pointer-events: none;
  &:focus {
    background-color: aquamarine;
  }
}
</style>
