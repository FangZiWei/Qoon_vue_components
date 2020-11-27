<!-- <QInput v-model="login.UserPass" :input_type="showPass?'password':'text'" placeholder="用户名">
    <icon slot="front_icon" name="https://b.yzcdn.cn/vant/icon-demo-1126.png" size="25" />
    <div slot="behind_icon" style="display:flex;" @click="showPass=!showPass">
        <icon v-if="showPass" :name="password_show" size="25" />
        <icon v-else :name="password_hide" size="25" />
    </div>
</QInput> -->
<template>
    <div class="input_box">
        <slot name="front_icon" />
        <div class="input_pos">
            <input ref="input" :type="input_type" v-model="input_value" @focus="input_focus" @blur="input_blur">
            <label :class="input_active?'label_top':'label_middle'" @click="handleLabelClick" class="placeholder">{{ placeholder }}</label>
        </div>
        <slot name="behind_icon" />
    </div>
</template>
<script>
export default {
    name: 'input_2',
    props: {
        placeholder: String,
        value: String,
        input_type: {
            type: String,
            default: 'text',
        }
    },
    data() {
        return {
            input_active: false,
        }
    },
    computed: {
        input_value: {
            get () {
                return this.value
            },
            set (value) {
                this.$emit('input', value)
            },
        }
    },
    watch: {
        value(value) {
            if(value == '') {
                this.input_active = true;
            }
        }
    },
    mounted() {
        if(this.$props.value)
            this.input_active = true;
        else
            this.input_active = false;
    },
    methods: {
        handleLabelClick () {
            this.$refs.input.focus()
        },
        input_focus() {
            this.input_active = true;
        },
        input_blur() {
            if(!this.input_value)
                this.input_active = false;
        },
    },
}
</script>
<style lang="scss" scoped>
input {
    background-color: transparent;
    font-size: 1.2em;
}
.input_box {
    display: flex;
    align-items: center;
    padding: 0 4vw;
    justify-content: space-between;
    
    background-color: #fff;
    border-radius: 10vw;
    >i {
        padding-right: 4vw;
    }
}

.input_pos {
    flex: 1;
    position: relative;
    height: 12vw;
    display: flex;
    align-items: center;
    .placeholder {
        position: absolute;
    }
    label {
        transition: all .5s ease;
        transform: translate(0, -50%);

        &.label_top {
            top: 20%;
            font-size: 10px;
        }
        &.label_middle {
            top: 50%;
        }
    }
}
</style>