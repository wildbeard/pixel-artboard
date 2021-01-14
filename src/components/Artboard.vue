<template>
    <div>
        <canvas id="canvas"
                ref="canvas"
                @click="handleClick"
                @mousedown="handleDown"
                @mouseup="handleUp"
                @mousemove="handleMove">
        </canvas>
    </div>
</template>

<style scoped>
    #canvas {
        display: block;
	    outline: 1px solid black;
    }
</style>

<script>
export default {
    props: {
        tool: {
            type: String,
            required: true,
        },
        color: {
            type: String,
            required: true,
        },
        data: {
            type: Array,
            required: false,
        },
        width: {
            type: [ String, Number ],
            required: false,
            default: 512,
        },
        height: {
            type: [ String, Number ],
            required: false,
            default: 512,
        },
        cellSize: {
            type: [ String, Number ],
            required: false,
            default: 16,
        },
    },
    data() {
        return {
            on_board: [],
            mouse_down: false,
        };
    },
    computed: {
        canvas() {
            return this.$refs['canvas'];
        },
        ctx() {
            return this.canvas.getContext('2d');
        },
    },
    mounted() {
        this.canvas.width = this.width;
        this.canvas.height = this.height;
        this.on_board = this.data;
        this.drawGrid();
    },
    methods: {
        drawGrid() {
            const rows = this.width / this.cellSize;
            const columns = rows;
            let x = 0;
            let y = this.cellSize;
            // Reset
            this.ctx.clearRect(0, 0, this.canvas.width, this.canvas.height);
            this.on_board.forEach(cell => this.fillCell(cell));
            // Horizontal Lines
            for ( let a = 0; a < rows; a++ ) {
                this.ctx.beginPath();
                this.ctx.moveTo(0, y);
                this.ctx.lineTo(this.width, y);
                this.ctx.stroke();
                this.ctx.closePath();
                y += this.cellSize;
            }
            y = 0;
            // Vertical Lines
            for ( let a = 0; a < columns; a++ ) {
                this.ctx.beginPath();
                this.ctx.moveTo(x, 0);
                this.ctx.lineTo(x, this.height);
                this.ctx.stroke();
                this.ctx.closePath();
                x += this.cellSize;
            }
        },
        fillCell(cell) {
            this.ctx.fillStyle = cell.color;
            this.ctx.fillRect(cell.x, cell.y, this.cellSize, this.cellSize);
        },
        addCellToData(cell) {
            this.on_board.push(cell);
            const payload = {
                on_board: this.on_board,
            }
            this.$emit('updated-board', payload);
        },
        removeCell(index) {
            this.on_board.splice(index, 1);
            const payload = {
                on_board: this.on_board,
            }
            this.$emit('updated-board', payload);
        },
        getCellInfo(event) {
            const clicked = this.getClickedPosition(event);
            const cell = this.findCell(clicked);
            const index = this.findCellIndex(cell, false);
            return { clicked, cell, index };
        },
        handleClick(e) {
            // @todo: Fix the last pixel being removed when dragging
            const { clicked, cell, index } = this.getCellInfo(e);
            console.log(clicked, cell, index);
            if ( index === -1 && this.tool === 'draw' ) {
                this.addCellToData(cell);    
            } else {
                if ( this.tool === 'eraser' || this.on_board[index].color === cell.color ) {
                    this.removeCell(index);
                } else {
                    this.on_board[index].color = cell.color;
                }
            }
            this.drawGrid();
        },
        handleDown(e) {
            this.mouse_down = true;
        },
        handleUp(e) {
            this.mouse_down = false;
        },
        handleMove(e) {
            if ( !this.mouse_down ) return;
            const { clicked, cell, index } = this.getCellInfo(e);
            if ( index === -1 && this.tool === 'draw' ) {
                this.addCellToData(cell);
            } else {
                if ( ( this.on_board[index].color === cell.color && !this.mouse_down ) || this.tool === 'eraser' && this.mouse_down ) {
                    this.removeCell(index);
                } else {
                    this.on_board[index].color = cell.color;
                }
            }
            this.drawGrid();
        },
        getClickedPosition(e) {
            return { x: e.offsetX, y: e.offsetY };
        },
        findCellIndex(cell, withColor = true) {
            if ( withColor ) {
                return this.on_board.findIndex(c => c.x === cell.x && c.y === cell.y && c.color === cell.color);
            } else {
                return this.on_board.findIndex(c => c.x === cell.x && c.y === cell.y);
            }
        },
        findCell(position) {
            const x = Math.floor(position.x/this.cellSize) * this.cellSize;
            const y = Math.floor(position.y/this.cellSize) * this.cellSize;
            return { x, y, color: this.color };
        },
    }
}
</script>