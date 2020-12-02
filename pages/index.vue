<template>
  <div class="sidebar-layout">
    <b-sidebar :reduce="!showSidebar" position="static" fullheight open>
      <div class="buttons sidebar-buttons" :class="{ reduced: !showSidebar }">
        <b-button
          :label="showSidebar ? 'Save' : ''"
          icon-left="save"
          type="is-success"
        />
        <b-button
          :label="showSidebar ? 'Download' : ''"
          icon-left="cloud-download-alt"
          type="is-primary"
        />
      </div>

      <b-menu class="is-custom-mobile padded-sidebar">
        <b-menu-list>
          <template slot="label">
            <b-button
              :icon-left="sideBarIcon(showSidebar)"
              :label="showSidebar ? 'My Project' : ''"
              type="is-text"
              size="is-small"
              @click="showSidebar = !showSidebar"
            />
          </template>

          <div v-show="showSidebar" class="buttons sidebar-buttons">
            <b-tooltip
              always
              label="Add Folder"
              position="is-right"
              type="is-primary is-light"
            >
              <b-button
                icon-left="folder-plus"
                type="is-primary is-light"
                size="is-small"
              />
            </b-tooltip>
          </div>

          <template v-for="file in filesys">
            <b-menu-item
              v-if="file.type === 'folder'"
              :key="file.name"
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
              :active.sync="openFile"
              icon="file-code"
            />
          </template>
        </b-menu-list>
      </b-menu>
      <b-tooltip
        :active="!showSidebar"
        label="Upload Files"
        position="is-right"
        append-to-body
      >
        <b-button
          :label="showSidebar ? 'Upload' : ''"
          icon-left="file-upload"
          type="is-primary is-light"
          expanded
        />
      </b-tooltip>
    </b-sidebar>

    <section class="app-wrapper">
      <Editor v-model="fileCode" class="code-editor" />

      <div class="draggable">
        <b-icon icon="arrows-alt-h" />
      </div>

      <div class="browsers">
        <Browser
          v-for="browser in browsers"
          :key="browser.name"
          :browser="browser"
          :code="fileCode"
          class="browser"
        />

        <b-tooltip label="Add Browser" type="is-primary is-light">
          <b-button icon-left="plus" type="is-primary is-light" expanded />
        </b-tooltip>
      </div>
    </section>
  </div>
</template>

<script>
import Editor from '~/components/Editor'
import Browser from '~/components/Browser'

export default {
  name: 'HomePage',
  components: { Editor, Browser },
  data() {
    return {
      showSidebar: true,
      showWorkspace: true,

      maxColumns: 12,
      contentSize: 10,

      openFile: null,
      fileCode: 'testing123',

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

      browsers: [{ name: 'native' }],
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
// Fix for tooltips in .buttons
.buttons .b-tooltip {
  margin-bottom: 0.5rem;

  & > .tooltip-trigger > .button {
    margin-bottom: 0;
  }
}

.sidebar-content.is-mini {
  .menu-label {
    width: 100%;
    display: flex;
    justify-content: center;
  }

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

<style lang="scss" scoped>
.sidebar-layout {
  display: flex;
  flex-direction: row;
  height: 100%;
}

.padded-sidebar {
  padding: 1em;
}

.menu-label {
  text-decoration: none;
}

.sidebar-buttons {
  margin: 1em 0 0 1.5em;

  &.reduced {
    margin-left: 0;
    width: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;

    & > .button {
      margin-right: 0;
    }
  }
}

.app-wrapper {
  max-height: 100%;
  width: 100%;
  display: flex;
}

.code-editor {
  flex-grow: 1;
  flex-shrink: 1;
  display: flex;
  flex-direction: row;
}

.draggable {
  display: flex;
  align-items: center;
  background-color: hsl(0, 0%, 96%);
}

.browsers {
  max-height: 100%;
  display: flex;
  flex-direction: column;
  padding: 1em;
  // gap: 1.5em;
  background-color: hsl(0, 0%, 96%);

  & > .browser {
    width: 250px;
    flex: 1 1 150px;
    margin: 0;
  }

  & > .browser:not(:last-child) {
    margin-bottom: 1.5em;
  }
}
</style>
