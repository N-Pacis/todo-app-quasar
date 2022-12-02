<template>
<q-layout view="lHh Lpr lFf">
    <q-header>
        <q-toolbar>
            <q-btn flat dense round icon="menu" aria-label="Menu" @click="toggleLeftDrawer" />
        </q-toolbar>
        <div class="q-px-lg q-pt-xl q-mb-md">
            <div class="text-h3">Todo</div>
            <div class="text-subtitle1">{{todaysDate()}}</div>
        </div>
        <q-img src="/mountains.jpeg" class="header-image absolute-top" />
    </q-header>

    <q-drawer v-model="leftDrawerOpen" show-if-above :width="200" :breakpoint="600">
        <q-scroll-area style="height: calc(100% - 150px); margin-top: 150px; border-right: 1px solid #ddd">
            <q-list padding>
                <q-item 
                  clickable 
                  v-ripple
                  to="/"
                  exact
                >
                    <q-item-section avatar>
                        <q-icon name="list" />
                    </q-item-section>

                    <q-item-section>
                        Todo
                    </q-item-section>
                </q-item>
                <q-item 
                  clickable 
                  v-ripple
                  to="/help"
                  exact
                >
                    <q-item-section avatar>
                        <q-icon name="help" />
                    </q-item-section>

                    <q-item-section>
                        Help
                    </q-item-section>
                </q-item>
            </q-list>
        </q-scroll-area>

        <q-img class="avatar-background absolute-top" src="/mountains.jpeg">
            <div class="absolute-bottom bg-transparent">
                <q-avatar size="56px" class="q-mb-sm">
                    <img src="https://cdn.quasar.dev/img/boy-avatar.png">
                </q-avatar>
                <div class="text-weight-bold">Nkubito Pacis</div>
                <div>@pacis_30</div>
            </div>
        </q-img>
    </q-drawer>
    <q-page-container>
        <router-view v-slot="{ Component }">
          <keep-alive>
              <component :is="Component" />
          </keep-alive>
        </router-view>

    </q-page-container>
</q-layout>
</template>

<script>
import {
    defineComponent,
    ref
} from 'vue'
import {
    date
} from 'quasar'

export default defineComponent({
    name: 'MainLayout',

    setup() {
        const leftDrawerOpen = ref(false)

        return {
            leftDrawerOpen,
            toggleLeftDrawer() {
                leftDrawerOpen.value = !leftDrawerOpen.value
            },
            todaysDate() {
                let timestamp = Date.now()
                return date.formatDate(timestamp, 'dddd D MMMM')
            }
        }
    }
})
</script>

<style lang="scss">
.header-image {
    height: 100%;
    z-index: -1;
    opacity: 0.2;
    filter: grayscale(100%);
}

.avatar-background {
    filter: grayscale(50%);
    z-index: -1;
    height: 150px;
}
</style>
