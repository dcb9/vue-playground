<template>
  <div>
    <div class="container" id="container"></div>
    <div>
      <button @click="horizontalEvenly">Horizontal Evenly</button>
      <button @click="alignTop">Align Top</button>
      <button @click="alignBottom">Align Bottom</button>
    </div>
  </div>
</template>

<script>
  import Konva from 'konva'
  // 点击空白处时解散对应的 group
  let shiftKeyActive = false

  function getRandomInt(min, max) {
    min = Math.ceil(min);
    max = Math.floor(max);
    return Math.floor(Math.random() * (max - min)) + min;
  }

  export default {
    name: 'app',
    data () {
      return {
        layer: undefined,
        selected: [],
      }
    },
    mounted() {
      this.listenShiftKey()
      let stage = new Konva.Stage({
        container: 'container',
        width: 500,
        height: 500,
      })

      let layer = new Konva.Layer()

      let colors = ['red', 'orange', 'yellow', 'green']
      for (let i = 0; i < 4; i++) {
        let box = new Konva.Rect({
          x: i * 30 + getRandomInt(10, 80),
          y: i * 18 + getRandomInt(30, 80),
          width: getRandomInt(20, 40),
          height: getRandomInt(90, 130),
          name: colors[i],
          fill: colors[i],
          stroke: 'black',
          strokeWidth: 4,
          draggable: true,
        })
        box.on('mouseover', () => this.pointerCursor())
        box.on('mouseout', () => this.defaultCursor())

        box.on('click', (evt) => {
          if (shiftKeyActive) {
            console.log('shift click')
            this.addToGroup(evt.target)
          } else {
            console.log('click')
            this.disbandSelected()
          }
        })

        layer.add(box)
      }

      stage.add(layer)
      this.layer = layer
    },
    methods: {
      listenShiftKey() {
        document.addEventListener('keydown', (event) => {
          const keyName = event.key
          if (keyName === 'Shift') {
            shiftKeyActive = true
          }
        }, false)

        document.addEventListener('keyup', (event) => {
          const keyName = event.key
          if (keyName === 'Shift') {
            shiftKeyActive = false
          }
        }, false)
      },
      addToGroup (shape)  {
        if (this.selected.indexOf(shape) === -1) {
          this.addGroupBorder(shape)
          this.selected.push(shape)
          this.layer.draw()
        }
      },
      disbandSelected () {
        // 解散 selected
        let borders = this.layer.get('.border')
        for (let i = 0; i < borders.length; i++) {
          borders[i].destroy()
        }
        // https://davidwalsh.name/empty-array
        this.selected.length = 0
        this.layer.draw()
      },
      addGroupBorder (shape) {
        let color = 'red'
        let {x, y} = shape.position()
        let leftLine = new Konva.Line({
          x: x - 1,
          y: y - 1,
          points: [0, 0, 0, shape.getHeight() + 2],
          stroke: color,
          tension: 0.5,
          name: 'border',
        })
        this.layer.add(leftLine)
      },
      horizontalEvenly() {
        let len = this.selected.length
        if (len < 2) return

        let selected = this.selected.slice()

        selected.sort((a, b) => {
          let [ax, bx] = [a.getX(), b.getX()]
          if (ax < bx) return -1
          if (ax > bx) return 1
          return 0
        })

        let left = selected.shift()
        let right = selected.pop()

        let endX = right.getX()
        let startX = left.getX() + left.getWidth()
        let sumWidth = 0

        selected.map(shape => {
          sumWidth += shape.getWidth()
        })

        if (endX - startX < sumWidth) return

        let averageSpace = (endX - startX - sumWidth) / (selected.length + 1)

        selected.map(shape => {
          let pos = shape.position()
          pos.x = startX + averageSpace
          shape.position(pos)
          startX = pos.x + shape.getWidth()
        })

        this.layer.draw()
      },
      alignTop() {
        console.log('align top')
        if (this.selected.length < 2) return

        let selected = this.selected.slice()
        selected.sort((a, b) => {
          let [ay, by] = [a.getY(), b.getY()]
          if (ay < by) return -1
          if (ay > by) return 1
          return 0
        })

        let {y: minY} = selected.shift().position()

        selected.map(shape => {
          let pos = shape.position()
          pos.y = minY
          shape.position(pos)
        })

        this.layer.draw()
      },
      alignBottom() {
        console.log('align bottom')
        if (this.selected.length < 2) return

        let selected = this.selected.slice()
        selected = selected.map((item) => {
          let {y: y} = item.position()
          item.bottomPos = y + item.getHeight()
          return item
        })

        selected.sort(({bottomPos: a}, {bottomPos: b}) => {
          if (a < b) return -1
          if (a > b) return 1
          return 0
        })

        let {bottomPos} = selected.pop()

        selected.map(shape => {
          let pos = shape.position()
          pos.y = bottomPos - shape.getHeight()
          shape.position(pos)
        })

        this.layer.draw()
      },
      pointerCursor() {
        document.body.style.cursor = 'pointer'
      },
      defaultCursor() {
        document.body.style.cursor = 'default'
      },
    },
  }
</script>
