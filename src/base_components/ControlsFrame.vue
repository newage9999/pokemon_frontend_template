<template>
    <img key="gameboyAdvanceFront"
        alt=""
        id="gameboyAdvanceFront"
        :src="gameboyAdvanceFrontImgSrc"
    />
    <img v-for="button in [
        {action: 'A', prefix: 'onButton'},
        {action: 'B', prefix: 'onButton'},
        {action: 'Menu', prefix: 'onButton'},
        {action: 'Up', prefix: 'onMovement'},
        {action: 'Right', prefix: 'onMovement'},
        {action: 'Down', prefix: 'onMovement'},
        {action: 'Left', prefix: 'onMovement'}
    ]"
        :key="`gameboyAdvanceButton${button.action}`"
        :id="`gameboyAdvanceButton${button.action}`"
        class="gameboyAdvanceButton"
        :src       ="gameboyAdvanceButtonImgSrc    (button.action)"
        :style     ="gameboyAdvanceButtonStyle     (button.action)"
        @mouseenter="gameboyAdvanceButtonMouseEnter(button.action)"
        @mouseleave="gameboyAdvanceButtonMouseLeave(button.action)"
        @mousedown ="gameboyAdvanceButtonMouseDown (button.action, button.prefix)"
        @mouseup   ="gameboyAdvanceButtonMouseUp   (button.action, button.prefix)"
    />
    <div id="exteriorWrapper">
        <div id="topFrame"    class="frame" />
        <div id="bottomFrame" class="frame" />
        <div id="leftFrame"   class="frame" />
        <div id="rightFrame"  class="frame" />
    </div>
</template>

<script>
import {
    gameboyAdvanceBack,
    gameboyAdvanceFront,
    gameboyAdvanceButtonAHovered,
    gameboyAdvanceButtonAPressed,
    gameboyAdvanceButtonBHovered,
    gameboyAdvanceButtonBPressed,
    gameboyAdvanceButtonUpHovered,
    gameboyAdvanceButtonUpPressed,
    gameboyAdvanceButtonLeftHovered,
    gameboyAdvanceButtonLeftPressed,
    gameboyAdvanceButtonDownHovered,
    gameboyAdvanceButtonDownPressed,
    gameboyAdvanceButtonRightHovered,
    gameboyAdvanceButtonRightPressed,
    gameboyAdvanceButtonStartHovered,
    gameboyAdvanceButtonStartPressed
} from '@/base_globals';

export default {
    name: 'ControlsFrame',
    emits: [
        'onButtonA',
        'onButtonB',
        'onButtonMenu',
        'onMovementUp',
        'onMovementDown',
        'onMovementLeft',
        'onMovementRight'
    ],
    data() {
        return {
            buttonsHovered: {
                A: false,
                B: false,
                Up: false,
                Left: false,
                Down: false,
                Right: false
            },
            buttonsPressed: {
                A: false,
                B: false,
                Up: false,
                Left: false,
                Down: false,
                Right: false
            },
            buttonUpHovered: false,
            buttonUpPressed: false
        };
    },
    methods: {
        gameboyAdvanceButtonImgSrc(button) {
            const buttonToImgSrcPressed = {
                A: gameboyAdvanceButtonAPressed,
                B: gameboyAdvanceButtonBPressed,
                Menu: gameboyAdvanceButtonStartPressed,
                Up: gameboyAdvanceButtonUpPressed,
                Left: gameboyAdvanceButtonLeftPressed,
                Down: gameboyAdvanceButtonDownPressed,
                Right: gameboyAdvanceButtonRightPressed,
            };
            const buttonToImgSrcHovered = {
                A: gameboyAdvanceButtonAHovered,
                B: gameboyAdvanceButtonBHovered,
                Menu: gameboyAdvanceButtonStartHovered,
                Up: gameboyAdvanceButtonUpHovered,
                Left: gameboyAdvanceButtonLeftHovered,
                Down: gameboyAdvanceButtonDownHovered,
                Right: gameboyAdvanceButtonRightHovered
            };
            return this.buttonsPressed[button] ? buttonToImgSrcPressed[button] : buttonToImgSrcHovered[button];
        },
        gameboyAdvanceButtonMouseEnter(button) {
            this.buttonsHovered[button] = true;
        },
        gameboyAdvanceButtonMouseLeave(button) {
            this.buttonsHovered[button] = false;
        },
        gameboyAdvanceButtonMouseDown(button, prefix) {
            this.buttonsPressed[button] = true;
            this.$emit(`${prefix}${button}`, true);
        },
        gameboyAdvanceButtonMouseUp(button, prefix) {
            this.buttonsPressed[button] = false;
            this.$emit(`${prefix}${button}`, false);
        },
        gameboyAdvanceButtonStyle(button) {
            const hide = !this.buttonsHovered[button] && !this.buttonsPressed[button];
            return `${hide ? 'opacity: 0;' : ''}`;
        }
    },
    computed: {
        gameboyAdvanceBackImgSrc() {
            return gameboyAdvanceBack;
        },
        gameboyAdvanceFrontImgSrc() {
            return gameboyAdvanceFront;
        }
    }
};
</script>

<style>
body {
  --map-block-size: 48px;
  --minus-half-map-block-size: calc(0 - (var(--map-block-size)/2))px;
  --composition-block-size: calc(var(--map-block-size) / 2);
  --screen-height: calc(var(--map-block-size) * 9);
  --screen-width: calc(var(--map-block-size) * 10);
  --height-offset: calc(50% - (var(--screen-height) / 2));
  --vertical-offset: 50px;
}
img {
  image-rendering: pixelated;
}
</style>

<style scoped>
#gameboyAdvanceFront {
    position: absolute;
    z-index: 100;
    top: calc(var(--height-offset) - 210px);
    left: calc(var(--height-offset) - 492px);
    height: 836px;
}
.gameboyAdvanceButton {
    position: absolute;
    z-index: 110;
}
#gameboyAdvanceButtonA {
    top: calc(var(--height-offset) + 62px);
    right: calc(var(--height-offset) - 467px);
}
#gameboyAdvanceButtonB {
    top: calc(var(--height-offset) + 109px);
    right: calc(var(--height-offset) - 333px);
}
#gameboyAdvanceButtonMenu {
    top: calc(var(--height-offset) + 315px);
    left: calc(var(--height-offset) - 221px);
}
#gameboyAdvanceButtonUp {
    top: calc(var(--height-offset) + 41px);
    left: calc(var(--height-offset) - 324px);
}
#gameboyAdvanceButtonLeft {
    top: calc(var(--height-offset) + 105px);
    left: calc(var(--height-offset) - 387px);
}
#gameboyAdvanceButtonDown {
    top: calc(var(--height-offset) + 163px);
    left: calc(var(--height-offset) - 324px);
}
#gameboyAdvanceButtonRight {
    top: calc(var(--height-offset) + 105px);
    left: calc(var(--height-offset) - 266px);
}
#exteriorWrapper {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: 20;
}
.frame {
    position: absolute;
    background-color: #262620;
}
#topFrame {
    top: 0;
    height: calc(var(--height-offset) - var(--vertical-offset));
    width: 100%;
}
#bottomFrame {
    bottom: 0;
    height: calc(var(--height-offset) + var(--vertical-offset));
    width: 100%;
}
#horizontalMovementButtons {
    display: flex;
    gap: 30px;
}
#leftFrame {
    left: 0;
    top: calc(var(--height-offset) - var(--vertical-offset));
    height: var(--screen-height);
    width: var(--height-offset);
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
}
#rightFrame {
    right: 0;
    top: calc(var(--height-offset) - var(--vertical-offset));
    height: var(--screen-height);
    width: calc(var(--height-offset) - var(--map-block-size));
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 30px;
}
</style>
