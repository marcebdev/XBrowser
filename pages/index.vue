<template>
  <div class="sidebar-layout">
    <b-sidebar :reduce="!showSidebar" position="static" fullheight open>
      <b-menu class="is-custom-mobile padded-sidebar">
        <b-menu-list>
          <template slot="label">
            <b-button
              :icon-left="sideBarIcon(showSidebar)"
              :label="showSidebar ? 'My Project' : ''"
              type="is-text"
              size="is-small"
              class="menu-label"
              @click="showSidebar = !showSidebar"
            />
          </template>

          <template v-for="file in filesys">
            <b-menu-item
              v-if="file.type === 'folder'"
              :key="file.name"
              :active="file.open"
              icon="folder"
            >
              <span slot="label" slot-scope="props">
                {{ file.name }}
                <b-icon
                  v-if="file.contains.length"
                  class="is-pulled-right"
                  :icon="dropDownIcon(props.expanded)"
                />
              </span>

              <b-menu-item
                v-for="file in file.contains"
                :key="file.name"
                :label="file.name"
                icon="file-code"
              />
            </b-menu-item>

            <b-menu-item
              v-else
              :key="file.name"
              :label="file.name"
              icon="file-code"
            />
          </template>
        </b-menu-list>
      </b-menu>
    </b-sidebar>

    <section class="section columns">
      <div class="columns is-mobile">
        <card title="Free" icon="github">
          Open source on <a href="https://github.com/buefy/buefy"> GitHub </a>
        </card>

        <card title="Responsive" icon="cellphone-link">
          <b class="has-text-grey"> Every </b> component is responsive
        </card>

        <card title="Modern" icon="alert-decagram">
          Built with <a href="https://vuejs.org/"> Vue.js </a> and
          <a href="http://bulma.io/"> Bulma </a>
        </card>

        <card title="Lightweight" icon="arrange-bring-to-front">
          No other internal dependency
        </card>
      </div>
    </section>
  </div>
</template>

<script>
import Card from '~/components/Card'

export default {
  name: 'HomePage',
  components: {
    Card,
  },
  data() {
    return {
      showSidebar: true,
      showWorkspace: true,

      maxColumns: 12,
      contentSize: 10,

      filesys: [
        {
          name: 'src',
          type: 'folder',
          open: false,
          contains: [{ name: 'test.js', type: 'file' }],
        },
        {
          name: 'empty',
          type: 'folder',
          open: false,
          contains: [],
        },
        { name: 'README.md', type: 'file' },
      ],
    }
  },
  computed: {
    sideBarSize() {
      return this.maxColumns - this.contentSize
    },
  },
  methods: {
    openSideBar(openSize, closeSize = 1) {
      this.showWorkspace = !this.showWorkspace
      this.contentSize = this.showWorkspace
        ? this.maxColumns - openSize
        : this.maxColumns - closeSize
    },
    sideBarIcon(active) {
      return active ? 'chevron-left' : 'chevron-right'
    },
    dropDownIcon(active) {
      return active ? 'chevron-up' : 'chevron-down'
    },
    getIcon(type) {
      return type === 'folder' ? 'folder' : 'file-code'
    },
  },
}
</script>

<style lang="scss">
.sidebar-content.is-mini {
  &:not(.is-mini-expand),
  &.is-mini-expand:not(:hover) {
    .menu-label:not(:last-child) {
      margin-bottom: 0.25em;
    }

    .menu-list {
      li {
        a span:nth-child(2) {
          display: none;
        }

        ul {
          padding-left: 0;
          li a {
            display: inline-block;
          }
        }
      }
    }
  }
}
</style>

<style scoped>
.sidebar-layout {
  display: flex;
  flex-direction: row;
  min-height: 100%;
}

.padded-sidebar {
  padding: 1em;
}

.menu-label {
  text-decoration: none;
}
</style>
