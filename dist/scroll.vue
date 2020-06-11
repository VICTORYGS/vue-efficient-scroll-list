<template>
    <div class="scrollViewContainer">
        <drawArea :tip="pullDownTip" type="top" ref="drawAreaUp"></drawArea>
        <div class="scrollView" ref="scrollView" @scroll.prevent="" @touchmove.stop="touchmove" @touchstart.stop="touchstart"
             @touchend.stop="touchend">
            <slot :list="list"></slot>
        </div>
        <drawArea :tip="noMoreMsgTip" type="bottom" ref="drawAreaDown" :isNoMore="isNoMore"></drawArea>
    </div>
</template>

<script>
    import drawArea from "./drawArea";
    export default {
        name: "effScrollView",
        mounted() {
            this.scrollViewNode = this.$refs.scrollView
            this.viewHeight = this.scrollViewNode.clientHeight
            this.upCss=window.getComputedStyle(this.$refs.drawAreaUp.$el)
            this.downCss=window.getComputedStyle(this.$refs.drawAreaDown.$el)
        },
        data() {
            return {
                moveId: null,
                scrollViewNode: null,
                viewHeight: 900,
                canLoadMsg: true,
                preLoadDistance: 1,
                screenY: 0,
                sliderStatus: null,
                isNoMore: false,
                drawAreaIsExit: false,
                slideVal: 0,
                distanceRatio: 0.4,
                animate:window.requestAnimationFrame.bind(window),
                upCss:null,
                downCss:null,
            }
        },
        methods: {
            init() {
                this.drawIds = {}
                this.canLoadMsg=true
                this.isNoMore = false
            },
            noMore() {
                this.isNoMore = true
            },
            scroll() {
                const {scrollTop, scrollHeight} = this.scrollViewNode;
                if (scrollHeight - this.viewHeight - scrollTop < this.preLoadDistance) {//scrollToBottom
                    if (this.slideVal<0&&this.canLoadMsg) {
                        this.translateY(this.slideVal);
                        // console.log('isBottom')
                        //emit event when list updated
                        this.$emit('arriveBottom')
                        this.canLoadMsg = false
                    }
                } else if ( this.slideVal>0&&!scrollTop) {//scrollToTop
                    // console.log('show top')
                    this.translateY(this.slideVal);
                }
            },
            touchmove(e) {
                if (this.moveId) return;
                this.moveId = this.animate(() => {
                    this.slideVal = e.touches[0].screenY - this.screenY;

                    this.scroll()

                    this.moveId = null
                })
            },
            touchstart(e) {
                this.screenY = e.touches[0].screenY
            },
            touchend() {
                this.animate(()=>{
                    if (this.drawAreaIsExit) this.releaseAnimate();
                })
            },
            translateY(distance) {
                let moveVal = this.calculateDistance(distance)
                let transform = `translateY(${moveVal}px)`
                this.scrollViewNode.style.transform = transform
                const up = v => this.$refs.drawAreaUp.$el.style.transform = v
                const down = v => {
                    this.isNoMore=false
                    this.$refs.drawAreaDown.$el.style.transform = v
                }
                if (moveVal) {
                    moveVal > 0 ? up(transform) : down(transform)
                    this.drawAreaIsExit = true
                }
                // remark up or down
                this.sliderStatus = moveVal > 0 ? 'up' : 'down'
            },
            releaseAnimate() {
                const moveUpVal = this.upCss.height
                const moveDownVal = this.downCss.height
                switch (this.sliderStatus) {
                    case 'up':
                        // emit pullAndRelease event
                        this.$refs.drawAreaUp.toggleTip(false);
                        // drawAreaUp is showed completely?
                        console.log('hide0')

                        if (this.slideVal * this.distanceRatio >= parseFloat(moveUpVal)) {
                            this.$emit('pullAndRelease');
                        } else return this.hideDrawArea();// hide immediately

                        var transform = `translateY(${moveUpVal})`
                        this.$refs.drawAreaUp.$el.style.transform = transform
                        break
                    case 'down':
                        var transform = `translateY(-${moveDownVal})`
                        this.$refs.drawAreaDown.$el.style.transform = transform
                        break
                }
                this.scrollViewNode.style.transform = transform;
                this.isNoMore && this.finishUpdate();
            },
            calculateDistance(distance) {
                let sign = 1
                if (distance < 0) {
                    sign = -1
                }
                distance *= this.distanceRatio
                let count = Math.abs(distance / 10), res = 0;
                while (count > 0) {
                    res += (count >= 1 ? 10 : 10 * count);
                    count--
                }
                return res * sign
            },
            finishUpdate() {
                this.$nextTick(() => {
                    this.isNoMore ? setTimeout(() => {
                        this.hideDrawArea()
                        this.isNoMore=false
                        this.canLoadMsg=true
                    }, 800) : this.hideDrawArea();
                })
            },
            hideDrawArea() {
                let transform = `translateY(0px)`
                this.$refs.drawAreaUp.$el.style.transform = transform
                this.$refs.drawAreaDown.$el.style.transform = transform
                this.scrollViewNode.style.transform = transform
                this.drawAreaIsExit = false;
                this.sliderStatus === 'up' && this.$refs.drawAreaUp.toggleTip(true);
            }
        },
        watch: {
            list: {
                handler() {
                    if (this.sliderStatus === 'down') {
                        // dom haven't updated presently
                        this.canLoadMsg = true
                    } else { // after updated
                        this.init()
                        this.$refs.drawAreaUp.toggleTip(true);
                    }
                }
            }
        },
        components: {drawArea},
        props: {
            noMoreMsgTip: {
                type: String,
                default: "no more data"
            },
            pullDownTip: {
                type: String,
                default: "release to refresh"
            },
            list: {
                type: Array,
                default: []
            },
            singleCount: {
                type: Number,
                default: 20
            }
        }
    }
</script>
<style scoped>
    .scrollViewContainer {
        border: 1px solid red;
        width: 100%;
        height: 100%;
        overflow: hidden;
        position: relative;
        box-sizing: border-box;
    }

    .scrollView {
        transition: translateY .5s;
        width: 100%;
        height: 100%;
        overflow: auto;
    }
</style>