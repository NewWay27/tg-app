<template>
    <div v-if="debugVisible" class="debug-overlay" :style="{ top: posY + 'px', left: posX + 'px' }"
        @mousedown="startDrag">
        <div class="debug-header">
            <button @click.stop="clearLogs">Очистить</button>
            <button @click.stop="hideDebug">Закрыть</button>
        </div>
        <div v-for="(log, i) in logs" :key="i">{{ log }}</div>
    </div>

    <button v-else class="debug-toggle" @click="debugVisible = true">
        debug
    </button>
</template>


<script>
export default {
    data() {
        return {
            logs: [],
            debug: true,
            debugVisible: true,
            posX: 10,
            posY: 10,
            isDragging: false,
            dragOffsetX: 0,
            dragOffsetY: 0,
        };
    },
    mounted() {
        const originalLog = console.log;
        console.log = (...args) => {
            this.logs.push('[log] ' + args.map(arg =>
                typeof arg === 'object' ? JSON.stringify(arg, null, 2) : String(arg)
            ).join('\n'));
            originalLog(...args);
        };

        const originalErr = console.error;
        console.error = (...args) => {
            this.logs.push('[error] ' + args.map(arg =>
                typeof arg === 'object' ? JSON.stringify(arg, null, 2) : String(arg)
            ).join('\n'));
            originalErr(...args);
        };

        window.onerror = (msg, src, line, col, err) => {
            this.logs.push(`[window.onerror] ${msg} at ${line}:${col}`);
        };

        window.onunhandledrejection = (e) => {
            this.logs.push(`[unhandledrejection] ${e.reason}`);
        };

        window.addEventListener('mousemove', this.onDrag);
        window.addEventListener('mouseup', this.stopDrag);
    },
    beforeDestroy() {
        window.removeEventListener('mousemove', this.onDrag);
        window.removeEventListener('mouseup', this.stopDrag);
    },
    methods: {
        startDrag(e) {
            this.isDragging = true;
            this.dragOffsetX = e.clientX - this.posX;
            this.dragOffsetY = e.clientY - this.posY;
        },
        onDrag(e) {
            if (this.isDragging) {
                this.posX = e.clientX - this.dragOffsetX;
                this.posY = e.clientY - this.dragOffsetY;
            }
        },
        stopDrag() {
            this.isDragging = false;
        },
        clearLogs() {
            this.logs = [];
        },
        hideDebug() {
            this.debugVisible = false;
        }
    }
};
</script>

<style>
.debug-overlay {
    position: fixed;
    background: rgba(0, 0, 0, 0.85);
    color: #0f0;
    font-family: monospace;
    font-size: 12px;
    padding: 10px;
    z-index: 9999;
    white-space: pre-wrap;
    max-width: 90vw;
    max-height: 40vh;
    overflow-y: auto;
    border: 1px solid #0f0;
    cursor: move;
    user-select: none;
    min-width: 200px;
}

.debug-header {
    display: flex;
    justify-content: space-between;
    margin-bottom: 5px;
}

.debug-header button {
    background: none;
    border: 1px solid #0f0;
    color: #0f0;
    padding: 2px 6px;
    cursor: pointer;
    font-size: 12px;
}

.debug-toggle {
    position: fixed;
    bottom: 10px;
    right: 10px;
    z-index: 9999;
    background: black;
    color: #0f0;
    border: 1px solid #0f0;
    font-family: monospace;
    padding: 5px 10px;
    cursor: pointer;
}
</style>