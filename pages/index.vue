<template>
  <div class="sidebar-layout">
    <b-sidebar :reduce="!showSidebar" position="static" fullheight open>
      <div class="buttons sidebar-buttons" :class="{ reduced: !showSidebar }">
        <b-button type="is-success">Save</b-button>
      </div>

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
      <div class="code-editor">
        <ul class="line-list">
          <li class="line-number">1</li>
        </ul>
        <b-input
          class="editor-input"
          custom-class="has-fixed-size editor-custom"
        />
      </div>

      <div class="draggable">
        <b-icon icon="arrows-alt-h" />
      </div>

      <div class="browsers">
        <div v-for="browser in browsers" :key="browser" class="box browser">
          {{ browser }}
        </div>

        <b-tooltip label="Add Browser" type="is-primary is-light">
          <b-button icon-left="plus" type="is-primary is-light" expanded />
        </b-tooltip>
      </div>
    </section>
  </div>
</template>

<script>
export default {
  name: 'HomePage',
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

      browsers: ['Firefox', 'Chrome', 'Safari'],
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

.editor-custom {
  background-color: transparent !important;
  border: none !important;
  box-shadow: none !important;
  border-radius: 0;

  &:focus {
    background-color: hsl(0, 0%, 86%) !important;
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
    margin-left: auto !important;
    margin-right: auto;
  }
}

.app-wrapper {
  max-height: 100%;
  width: 100%;
  display: flex;
}

.code-editor {
  flex-grow: 1;
  display: flex;
  flex-direction: row;
  background-color: hsl(0, 0%, 93%);

  .line-list {
    border-right: 1px solid hsl(0, 0%, 86%);
  }

  .line-number {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 2.5em;
    padding-left: 0.5em;
    padding-right: 1.25em;
  }

  .editor-input {
    flex-grow: 1;
  }
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

  & > .browser:not(:last-child) {
    margin-bottom: 1.5em;
  }
}

.browser {
  width: 250px;
  flex: 1 1 150px;
  margin: 0;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
