<template>
    <div class="slide" @mouseenter="stop" @mouseleave="start">
        <div class="list clearfix" ref="list">
            <div class="item" ref="item">
                <img src="./../assets/1.jpg"/>
                <img src="./../assets/2.jpg"/>
                <img src="./../assets/3.jpg"/>
                <img src="./../assets/4.jpg"/>
                <img src="./../assets/5.jpg"/>
            </div>
        </div>
    </div>
</template>
<style>
    .slide {
        width: 615px;
        height: 200px;
        margin: 0 auto;
        overflow: hidden;
    }

    .list {
        width: auto;
    }

    .item {
        font-size: 0;
        float: left;
    }

    .clearfix:after {
        display: block;
        content: '';
        height: 0;
        visibility: hidden;
        clear: both;
    }
</style>
<script>
    export default {
        name: 'Slide',
        mounted() {
            var self = this;
            var dom = (function () {
                return {
                    item: self.$refs.item,
                    list: self.$refs.list
                }
            })();
            this.itemWidth = this.setItemWidth(dom.item);
            this.copyItem(dom.list);
            this.setListWidth(dom.list, this.itemWidth);
            this.scroll();
        },
        props: {
            speed: {
                type: Number,
                default: 30
            }
        },
        data() {
            return {
                timer: null,
                itemWidth: 0
            }
        },
        methods: {
            setListWidth(element, itemWidth) {
                var length = element.querySelectorAll('.item').length;
                element.style.width = (itemWidth * length) + 'px'
            },
            setItemWidth(element) {
                var imageList = element.querySelectorAll('img');
                //alert(imageList[0].style.width)
                var imageWidth = 323;
                var itemWidth = null;
                for (var i = 0; i < imageList.length; i++) {
                    itemWidth += imageWidth;
                }
                element.style.width = itemWidth + 'px';
                return itemWidth;
            },
            copyItem(element) {
                var html = '';
                for (var i = 0; i < 2; i++) {
                    html += element.innerHTML;
                }
                element.innerHTML = html;
            },
            scroll() {
                var element = this.$el;
                var self = this;
                this.timer = setInterval(function () {
                    if (element.scrollLeft >= self.itemWidth) {
                        element.scrollLeft = 0;
                    } else {
                        element.scrollLeft += 1;
                    }
                }, this.speed);
            },
            start() {
                this.scroll();
            },
            stop() {
                if (this.timer) {
                    clearInterval(this.timer);
                }
            }
        }
    };
</script>