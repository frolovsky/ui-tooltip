<script lang="ts">
import { h, defineComponent, Teleport } from 'vue';

export default defineComponent({
  name: 'ui-tooltip',

  data: () => ({
    showContent: false,
    contentPos: {
      x: 0,
      y: 0,
    },
  }),

  props: {
    appendToBody: {
      type: Boolean,
      default: false,
    },
  },

  methods: {
    showVisibleContent() {
      this.showContent = true;
    },
    hideVisibleContent(e: MouseEvent) {
      if (
        e.relatedTarget !== this.$refs.content &&
        e.relatedTarget !== this.$refs.activator
      ) {
        this.showContent = false;
      }
    },
  },

  watch: {
    contentPos: {
      handler(pos) {
        if (!this.$refs.content || !this.appendToBody) return;
        (this.$refs.content as HTMLElement).style.top = `${pos.y}px`;
        (this.$refs.content as HTMLElement).style.left = `${pos.x}px`;
      },
      deep: true,
    },
  },

  render() {
    return h(
      'div',
      {
        class: 'ui-tooltip',
        onMouseover: this.showVisibleContent,
        onMouseleave: this.hideVisibleContent,
      },
      [
        h(
          'div',
          {
            class: 'ui-tooltip__activator',
            ref: 'activator',
            onVnodeMounted: ({ el: activator }) => {
              if (!activator || !this.appendToBody) return false;
              const { x, y, width, height } = activator.getBoundingClientRect();
              this.contentPos.x = x + width / 2;
              this.contentPos.y = y + height;
            },
          },
          this.$slots.activator!()
        ),
        h(Teleport, { to: 'body', disabled: !this.appendToBody }, [
          h(
            'div',
            {
              class: [
                'ui-tooltip__content',
                { 'ui-tooltip__content--visible': this.showContent },
              ],
              ref: 'content',
              onMouseleave: this.hideVisibleContent,
            },
            this.$slots.content!()
          ),
        ]),
      ]
    );
  },
});
</script>

<style lang="scss" scoped>
.ui-tooltip {
  position: relative;

  &__activator {
    cursor: pointer;
  }

  &__content {
    display: none;

    &--visible {
      position: absolute;
      display: block;
      padding: 10px 30px;
      border-radius: 8px;
      top: 100%;
      left: 50%;
      transform: translateX(-50%);
      background-color: rgba(0, 0, 0, 0.5);
      color: #fff;
    }
  }
}
</style>
