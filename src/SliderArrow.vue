<script>
import { PROP_KEYS, canGoNext } from './innerSliderUtils'
import { mergeVNodeData, setVNodeData } from './vNodeUtils'

export default {
  name: 'SliderArrow',
  props: [...PROP_KEYS.ARROW, 'type'],
  render() {
    let classes = { 'lbxslick-arrow': true }
    let clickable = true
    let arrow
    let option = {
      currentSlide: this.currentSlide,
      slideCount: this.slideCount,
    }
    if (this.type === 'previous') {
      classes['lbxslick-prev'] = true
      if (
        !this.infinite &&
        (this.currentSlide === 0 || this.slideCount <= this.slidesToShow)
      ) {
        classes['lbxslick-disabled'] = true
        clickable = false
      }

      option.key = '0'
      arrow = this.prevArrow ? (
        this.prevArrow(option)[0]
      ) : (
        <button type="button" data-role="none" style="display: block;">
          Previous
        </button>
      )
    } else {
      classes['lbxslick-next'] = true
      if (!canGoNext(this.$props)) {
        classes['lbxslick-disabled'] = true
        clickable = false
      }

      option.key = '1'
      arrow = this.nextArrow ? (
        this.nextArrow(option)[0]
      ) : (
        <button type="button" data-role="none" style="display: block;">
          Next
        </button>
      )
    }
    setVNodeData(arrow, 'key', option.key)
    mergeVNodeData(arrow, 'class', classes)
    mergeVNodeData(arrow, 'on', {
      click: () => {
        if (clickable) {
          this.$emit('arrowClicked', { message: this.type })
        }
      },
    })

    return arrow
  },
}
</script>
<style scoped>
.lbxslick-arrow.lbxslick-hidden {
  display: none;
}
</style>
