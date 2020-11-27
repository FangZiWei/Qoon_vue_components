<!--
    <navbar
        title="标题"
        :left_arrow="true"
        :CB_left="onClickLeft"
        :CB_right="onClickRight"
        >
        <div slot="rightContent">
            <div>
                范德萨发顺丰
            </div>
        </div>
    </navbar>

    参数
    title: Str
    left_arrow: Bool
    CB_left: Fun 外部传入 必须要返回值 true 为禁止自动返回
    CB_right: Fun 自执行方法
 -->
<template>
	<div class="navbar">
        <div class="back" @click="onClickLeft">啊</div>
        <div class="title">{{ title }}</div>
        <div class="right">
            <slot name="right" />
        </div>
	</div>
</template>
<script>
// import { NavBar } from "vant";

export default {
	props: {
		title: {
			type: String,
			default: "",
		},
		left_arrow: {
			type: Boolean,
			default: false,
		},
		CB_left: {
			type: Function,
		},
		CB_right: {
			type: Function,
		}
	},
	components: {},
	methods: {
		onClickLeft: function () {
			console.log(this.$router, this.$route);
            if (this.$props.CB_left) {
                // 返回 ture 则 自动 返回 否则 自定义跳转方法
				var res = this.$props.CB_left();

				if(res == undefined) console.error('CB_left 必须拥有 bool类型 返回值');
				if (res) this.$router.go(-1);
			} else {
				this.$router.go(-1);
			}
		}
	},
};
</script>
<style lang="scss" scoped>
.navbar {
    display: flex;
    .back {
        flex: 1;
        display: flex;
    }
    .title {
        flex: 3;
    }
    .right {
        flex: 1;
        display: flex;
        justify-content: flex-end;
    }
}
</style>
