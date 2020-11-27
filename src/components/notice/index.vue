<template>
    <div>
        <ul class="notice_arr">
            <li class="notice" :ref="'notice_'+index" v-for="(item, index) in data" :key="index" @click="click_item(item, index)">
                <span>{{ item }}</span>
            </li>
        </ul>
    </div>
</template>
<script>
export default {
    name: 'Notice',
    props: {
        click_item: {
            type: Function,
            default: null,
        },
        data: {
            type: Array,
            default: new Array(),
        }
    },
    data() {
        return {}
    },
    mounted() {
        let PW = this.$refs['notice_'+0][0].parentElement.clientWidth;
        this.data.forEach((item, index)=>{
            item;
            this.$refs['notice_'+index][0].style.left = PW + 'px';
        })
        this.$refs['notice_'+0][0].style.left = PW-2 + 'px';

        this.scrollNotice();
        window.that = this;
    },
    methods: {
        // 逻辑 漏洞 当全部 列长度之和 小于外部长度 产生
        scrollNotice() {
            try {
                if (this.$refs['notice_'+0].length > 0) {
                    let PW = this.$refs['notice_'+0][0].parentElement.clientWidth;
                    let moveSize = 2;
                    for(let i=0,len=this.data.length;i<len;i++) {
                        // 当前是不是 领头的 是则正常滚动 否则 跟随前面的
                        // > 判断领头条件 前一个 不在滚动区域内
                        let last_i = i == 0 ? (len-1) : (i-1);
                        let CxP = PW;
                        /**
                         * PW: 父级宽度
                         * CmPh: 当前元素头部定位
                         * CmPt: 当前元素尾部定位
                         * CmW: 元素宽度
                         * ClPh: 上级头部定位
                         * ClPt: 上级尾部定位
                         * ClW: 上级宽度
                         */
                        const CmW = this.$refs['notice_'+i][0].clientWidth;
                        const CmPh = parseInt(this.$refs['notice_'+i][0].style.left);
                        // const CmPt = CmPh + CmW;
                        const ClW = this.$refs['notice_'+last_i][0].clientWidth;
                        const ClPh = parseInt(this.$refs['notice_'+last_i][0].style.left);
                        const ClPt = ClPh + ClW;

                        // // 当前元素是否 在 滚动范围内
                        // IF -CmW<CmPh<PW
                        //     MOVE ON
                        // // 父级元素尾部 是否在 滚动范围
                        // ELSE IF 0<ClPt<PW
                        //     POSITION PW-1
                        // // 父级元素 不在滚动状态
                        // ELSE IF PW<ClPt<PW+CmW
                        //     POSITION PW
                        // ELSE
                        //     CONTINUE;

                        // 第三版(修复 总长度不足 父级长度 错误滚动)
                        if((-1*CmW)<CmPh && CmPh<PW) {
                            CxP = CmPh - moveSize;
                        }
                        else if(0<ClPt && ClPt<PW) {
                            CxP = PW-1;
                        }
                        else if(PW<ClPt && ClPt<(PW+ClW)) {
                            CxP = PW;
                        }
                        else {
                            continue;
                        }
        
                        this.$refs['notice_'+i][0].style.left = CxP + 'px';
                    }
    
                    setTimeout(() => {
                        this.scrollNotice();
                    }, 60);
                }

            } catch (error) {
                console.log(error)
            }
        }
    }
}
</script>
<style lang="scss" scoped>
.notice_arr {
    list-style: none;
    padding: 0;
    margin: 0;
    background-color: red;

    display: flex;
    align-items: center;
	overflow: hidden;
	white-space: nowrap;
	align-self: center;
	height: 25px;
	color: blue;
	overflow: hidden;
	position: relative;
}
.notice {
	position: absolute;
	left: 100vw;
	padding-left: 20px;
    span {
        cursor: pointer;
    }
}
</style>
