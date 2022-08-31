<template>
  <AposModal
    class="apos-area-menu-expanded"
    :modal="modal"
    modal-title="apostrophe:addContent"
    @inactive="modal.active = false"
    @show-modal="modal.showModal = true"
    @esc="close"
    @no-modal="$emit('safe-close')"
  >
    <template #main>
      <AposModalBody>
        <template #bodyMain>
          <div
            v-for="(item, index) in widgets"
            :key="index"
            class="apos-widget-preview"
          >
            {{ $t(item.label) }}
          </div>
        </template>
      </AposModalBody>
    </template>
  </AposModal>
</template>

<script>
export default {
  name: 'AposAreaExpandedMenu',
  props: {
    options: {
      type: Object,
      required: true
    },
    maxReached: {
      type: Boolean
    },
    disabled: {
      type: Boolean,
      default: false
    }
  },
  emits: [ 'expanded-menu-close', 'safe-close', 'add' ],
  data() {
    return {
      modal: {
        active: false,
        type: 'slide',
        showModal: false,
        width: 'one-third'
      },
      widgets: []
    };
  },
  computed: {
    moduleOptions() {
      return window.apos.area;
    }
  },
  async mounted() {
    this.modal.active = true;

    for (const item of Object.keys(this.options.widgets)) {
      this.widgets.push(apos.modules[`${item}-widget`]);
    }

  },
  methods: {
    close() {
      this.modal.showModal = false;
    },
    async add(item) {
      this.$emit('add', {
        ...item,
        index: this.index
      });
      this.$emit('expanded-menu-close');
    }
  }
};
</script>

<style lang="scss" scoped>
.apos-area-menu-expanded {
  @include type-base;
}
</style>
