<template xmlns:v-touch="http://www.w3.org/1999/xhtml">
  <div>
    <div style="text-align: center;color: #FF5E3A;margin-top: 11px;margin-bottom: 24px;font-size: 16px">{{numberValue}}.0</div>
    <div class="ruler " ref="testAA" style="border: 1px solid #B5C0CD;">
      <div class="main"
           :style="{'-webkit-transform': 'translateX(' +  mainValue + 'px)', 'width': getMainWidth+'px', position: 'relative',  left: '0px'}">
        <ul style="overflow: hidden;  -webkit-transform: translateX(45px);">
          <li v-touch:touchstart="onTouchStart" v-touch:touchmove="onTouchMove" v-touch:touchend="onTouchEnd"
              v-touch:touchcancel="onTouchEnd" v-for="(n,index) in itemCount" :key="n"><span class="num">{{min  + index * 5 }} </span>
          </li>
        </ul>
      </div>
      <div class="arrow"></div>
    </div>
  </div>

</template>
<script>

  export default {
    props: ["value", "min", "max", "onChange"],
    components: {},
    data: function () {
      return {
        mainValue: 0,
        end: 0,
        numberValue: this.value,
        start: 0,
        startX: 0,
        moveX: 0,
        X: 0,
        gap: this.value - this.min
      }
    },
    computed: {
      itemCount() {
        const num = this.max - this.min;
        let value = num / 5;
        return value + 1;
      },
      getMainWidth() {
        return ((this.max - this.min) / 5 + 2) * 104
      }
    },
    created() {

    },
    mounted() {
      this.calcInit(this.value);
    },
    methods: {

      calcInit(value) {
        let startValue = this.min - 5;
        let width = this.$refs.testAA.offsetWidth;

        let centerPX = width / 2 - 40;
        let offsetKe = centerPX / 104;
        while (offsetKe > 0) {
          if (offsetKe < 1) {
            startValue += offsetKe * 5;
          } else {
            startValue += 5
          }
          offsetKe--;
        }

        let offsetPx = ( value - startValue) / 5;
        offsetPx = offsetPx * 105 - offsetPx;
        this.mainValue = -offsetPx;
      },
      onTouchStart(e) {
        const that = this;
        e.stopPropagation();
        that.start = 0;
        that.end = -((this.max - this.min) * 20.8);
        that.startX = e.changedTouches[0].pageX - that.mainValue;
      },
      onTouchMove(e) {
        e.preventDefault();
        const that = this;
        that.moveX = e.changedTouches[0].pageX;
        that.X = that.moveX - that.startX;

        if (that.X > 0) {
          var vv = that.mainValue;
          if (vv >= that.start) {
            that.start = that.X > that.start ? that.start : that.X;
            that.mainValue = that.start;
          } else {
            that.mainValue = that.X;
          }
        } else {
          var vv = that.mainValue;
          if (vv <= that.end) {
            that.end = that.X < that.end ? that.end : that.X;
            that.mainValue = that.end;
          } else {
            that.mainValue = that.X;
          }
        }

        let width = this.$refs.testAA.offsetWidth;
        let startValue = this.min - 5;
        let centerValue = startValue;

        let centerPX = width / 2 - 40;
        let offsetKe = centerPX / 104;
        while (offsetKe > 0) {
          if (offsetKe < 1) {
            centerValue += offsetKe * 5;
          } else {
            centerValue += 5
          }
          offsetKe--;
        }
        let a = Math.abs(that.mainValue) / 20.8;
        that.numberValue = Math.round(centerValue + a);
        if(that.numberValue >= that.max){
          that.numberValue = that.max;
          that.calcInit(that.max);
          return ;
        }
        if(that.numberValue <= that.min){
          that.numberValue = that.min;
          that.calcInit(that.min);
          return ;
        }
      },
      onTouchEnd(var1, var2) {
        this.onChange && this.onChange(this.numberValue);
        this.calcInit( this.numberValue);
      }
    }
  }
</script>
<style>
  ul,li {
    list-style: none;
    padding: 0;
  }
  .ruler {
    height: 90px;
    overflow: hidden;
    position: relative;
    width: 100%;
    margin-left: auto;
    margin-right: auto;
    background-color: #F8FAFF;
  }

  .ruler li {
    float: left;
    width: 104px;
    height: 60px;
    background: url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAL8AAAAxCAYAAACLUHiuAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyFpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuNi1jMTQyIDc5LjE2MDkyNCwgMjAxNy8wNy8xMy0wMTowNjozOSAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENDIChXaW5kb3dzKSIgeG1wTU06SW5zdGFuY2VJRD0ieG1wLmlpZDpENzkxQzI5Q0VDQjExMUU4QTRDNDhDRDFEQjY1RjE4MCIgeG1wTU06RG9jdW1lbnRJRD0ieG1wLmRpZDpENzkxQzI5REVDQjExMUU4QTRDNDhDRDFEQjY1RjE4MCI+IDx4bXBNTTpEZXJpdmVkRnJvbSBzdFJlZjppbnN0YW5jZUlEPSJ4bXAuaWlkOkQ3OTFDMjlBRUNCMTExRThBNEM0OENEMURCNjVGMTgwIiBzdFJlZjpkb2N1bWVudElEPSJ4bXAuZGlkOkQ3OTFDMjlCRUNCMTExRThBNEM0OENEMURCNjVGMTgwIi8+IDwvcmRmOkRlc2NyaXB0aW9uPiA8L3JkZjpSREY+IDwveDp4bXBtZXRhPiA8P3hwYWNrZXQgZW5kPSJyIj8+Rxi/xQAAAK9JREFUeNrs3LEJgDAURdEfyUQuYO++WusAupKC4gCGIEjOGeAV8RKskqZlO+KWoh6bNj/ZHIf+9eC87tdmF9Ao8SN+ED+IH8QP4gfxg/hB/CB+ED+IH8QP4gfxg/hB/CB+ED+IH8QP4gfxQ4EcdR8YerS8Gc7zH9/IzY/fHhA/iB/ED+IH8YP4QfwgfviL7AiqS47AzQ/iB/GD+EH8IH4QP4gfxA/iB/GD+KHcKcAAOSwPsO1PsqYAAAAASUVORK5CYII=) left top no-repeat;
    background-size: 94px;
    padding-top: 30px;
    text-align: right;
    padding-left: 0;
  }

  .ruler li .num {
    display: inline-block;
    position: relative;
    margin-right: -6px;
  }

  .ruler .arrow {
    width: 0;
    height: 0;
    border: 8px solid transparent;
    border-top: 8px solid #ffa657;
    position: absolute;
    left: 50%;
    top: 0;
    margin-left: -8px;
  }

  .main li {
    background: url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAANMAAAAxCAYAAABAi5xQAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyFpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuNi1jMTQyIDc5LjE2MDkyNCwgMjAxNy8wNy8xMy0wMTowNjozOSAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENDIChXaW5kb3dzKSIgeG1wTU06SW5zdGFuY2VJRD0ieG1wLmlpZDpERDMwOEQ3MEVDQjExMUU4OEVFMDk2RDM3RkE1OEMyRCIgeG1wTU06RG9jdW1lbnRJRD0ieG1wLmRpZDpERDMwOEQ3MUVDQjExMUU4OEVFMDk2RDM3RkE1OEMyRCI+IDx4bXBNTTpEZXJpdmVkRnJvbSBzdFJlZjppbnN0YW5jZUlEPSJ4bXAuaWlkOkREMzA4RDZFRUNCMTExRTg4RUUwOTZEMzdGQTU4QzJEIiBzdFJlZjpkb2N1bWVudElEPSJ4bXAuZGlkOkREMzA4RDZGRUNCMTExRTg4RUUwOTZEMzdGQTU4QzJEIi8+IDwvcmRmOkRlc2NyaXB0aW9uPiA8L3JkZjpSREY+IDwveDp4bXBtZXRhPiA8P3hwYWNrZXQgZW5kPSJyIj8+sPzlxgAAAOlJREFUeNrs3DEKwjAUgOFEciIv4Obgfeusm4texgNUsBTcJNAX0/p9c3mEF35aF/NwuY9pktN3W362Rg9zPdvBvZ0O+3S+Pt5zd4nPRY8rmktn9yYmWIiYQEwgJhATICYQE4gJxASICcQEYgLEBGICMYGYADGBmEBMICZATPB7JS3/d0ozc839q7neTOAzD8QEYgLEBGKCtSiVzz+DzmFuLPttcG+1MR2DzmFuLPttcG+1Md2CzmFuLPttcG9+M4GYQEywScUKupKtwJsJxGQFICYQE4gJEBOICcQEYgLEBGICMQGTlwADAJ6bLbi/IMcnAAAAAElFTkSuQmCC) left top no-repeat;
    background-size: 100px auto;
    color: #b5d0e1;
  }


</style>
