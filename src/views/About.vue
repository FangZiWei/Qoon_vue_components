<template>
	<div class="about">
		<Navbar
			title="标题"
			:left_arrow="true"
			:CB_left="click_Left">
			<template #right>
				<div @click="clickIcon">
					icon
				</div>
			</template>
		</Navbar>
		<Notice :data="data_notice" :click_item="click_item"/>
	</div>
</template>
<script>
import Navbar from '@/components/navbar';
import Notice from '@/components/notice';

export default {
	name: 'about',
	components: {
		Navbar,
		Notice
	},
	data() {
		return {
			data_notice: [
				'a.注册V型在重新注册自行查找',
				'b.去玩儿育佛挡杀佛是',
				'c.防守打法发水电费等所发生的',
				'd.poio哦破婆婆',
				'e.路口见客户可好看了和刻录机',
				'f.面内部模板模板模板美女呢',
			]
		}
	},
	methods: {
		click_Left() {
			console.log('click_Left');

			return true;
		},
		clickIcon() {
			console.log('clickIcon');
		},
		click_item(item, index) {
			console.log(item, index);
		}
	}
}
</script>