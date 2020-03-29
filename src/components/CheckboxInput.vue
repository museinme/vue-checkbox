<template>
  <div class="checkbox-input">
    <div class="container" :class="{ pressed: isMouseDown }">
      <input :name="name" :id="id" :disabled="disabled" :checked="checked" class="native" type="checkbox" @mousedown="onMouseDown">
      <span class="custom">
        <span class="ripple" />
      </span>
      <label :for="id" class="label" v-if="$slots.default" @mousedown="onMouseDown">
        <slot />
      </label>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Checkbox',
  data() {
    return {
      isMouseDown: false,
    };
  },
  props: {
    disabled: {
      type: Boolean,
      default: false,
    },
    checked: {
      type: Boolean,
      default: false,
    },
    name: {
      type: String,
      required: true,
    },
    id: {
      type: String,
      required: true,
    }
  },
  methods: {
    onMouseDown(event) {
      this.isMouseDown = true;

      const onMouseUp = (event) => {
        setTimeout(this.onMouseUp,100);
        document.removeEventListener('mouseup', onMouseUp);
      };

      document.addEventListener('mouseup', onMouseUp);
    },
    onMouseUp() {
      this.isMouseDown = false;
    },
  }
};
</script>

<style lang="less">
  // variables
  @checkbox--width: 18px;
  @checkbox--background-color: #0e5ef6;
  @checkbox--border-color: @checkbox--background-color;
  @checkbox_label--font-size: 18px;
  @checkbox_label--line-height: @checkbox_label--font-size;
  @checkbox_disabled--background-color: #9e9e9e;
  @checkbox_disabled--border-color: @checkbox_disabled--background-color;
  @checkbox--border-color: #666666;
  @checkbox_checked--border-color: #009BFF;

  // mixins
  .lib-centered {
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
  }

  .lib-centered-Y {
    left: 0;
    top: 50%;
    transform: translateY(-50%);
  }

  // styles
  .checkbox-input {
    & {
      display: inline-block;
      font-size: 0;
    }

    > .container {
      & {
        display: inline-block;
        position: relative;
        margin: auto;
        cursor: pointer;
        min-width: @checkbox--width;
        min-height: @checkbox--width;
        font-size: 0;
      }

      &:hover {
        > .native:checked:not(:focus) ~ .custom > .ripple {
          background-color: fadeout(@checkbox--background-color, 95%);
        }

        > .custom > .ripple,
        > .native:checked:disabled:not(:focus) ~ .custom > .ripple {
          background-color: rgba(0, 0, 0, .04);
        }
      }

      &.pressed {
        > .custom > .ripple:after {
          animation: ripple .1s ease;
          opacity: 1;
        }
      }

      > .custom > .ripple {
        & {
          position: absolute;
          width: 40px;
          height: 40px;
          border-radius: 50%;
          .lib-centered();
        }

        &:after {
          transition: opacity .1s ease-in-out;
          content: '';
          position: absolute;
          .lib-centered-Y();
          background-color: rgba(51, 102, 204, .3);
          width: 40px;
          height: 40px;
          opacity: 0;
          border-radius: 100%;
        }
      }

      > .custom {
        & {
          position: absolute;
          .lib-centered-Y();
          height: @checkbox--width;
          width: @checkbox--width;
          box-sizing: border-box;
          background-color: transparent;
          border-radius: 2px;
          transition: all 0.2s ease-out;
          border: 2px solid @checkbox--border-color;
          z-index: 1;
        }

        &:after {
          position: absolute;
          content: "";
          left: 12px;
          top: 12px;
          height: 0px;
          width: 0px;
          border-color: @checkbox_checked--border-color;
          border-width: 0 3px 3px 0;
          transform: rotate(45deg) scale(0);
          opacity:1;
          transition: scale 0.2s ease-out;
        }
      }

      > .native {
        & {
          position: absolute;
          cursor: pointer;
          .lib-centered-Y();
          width: @checkbox--width;
          height: @checkbox--width;
          margin: 0;
          opacity: 0;
          z-index: 2;
        }

        &:disabled {
          + .custom > .ripple:after {
            display: none;
          }

          + .custom {
            border-color: @checkbox_disabled--border-color;
          }
        }

        &:disabled:checked + .custom {
          border-color: @checkbox_disabled--border-color;
          background-color: @checkbox_disabled--background-color;
        }

        &:focus + .custom > .ripple {
          background-color: rgba(0, 0, 0, .1);
        }

        &:checked:focus + .custom > .ripple {
          background-color: fadeout(@checkbox--background-color, 80%);
        }

        &:checked + .custom {
          & {
            background-color: @checkbox--background-color;
            border-radius: 2px;
            transform: translateY(-50%) scale(1);
            opacity:1;
            border: 2px solid @checkbox--border-color;
          }

          &:after {
            transform: translateX(-50%) rotate(45deg) scale(1);
            opacity:1;
            top: -2px;
            left: 50%;
            width: 5px;
            height: 12px;
            border: solid #FFFFFF;
            border-width: 0 2px 2px 0;
            background-color: transparent;
            border-radius: 0;
          }
        }
      }

      > .label {
        cursor: pointer;
        font-size: @checkbox_label--font-size;
        line-height: @checkbox_label--line-height;
        margin-left: @checkbox--width * 2;
        white-space: nowrap;
      }
    }
  }

  @keyframes ripple {
    0% {
      width: 20px;
      height: 20px;
    }
    100% {
      width: 40px;
      height: 40px;
    }
  }
</style>
