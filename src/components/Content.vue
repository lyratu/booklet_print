<script setup lang="ts">
import { ref, onMounted } from 'vue'
import * as pdfjsLib from 'pdfjs-dist';
import "pdfjs-dist/build/pdf.worker.mjs";

const uploadFile = ref()
const upload = () => {
    uploadFile.value.click()
}
const afterUpload = (event: any) => {
    // 处理上传逻辑
    const file = event.target.files[0];
    const url = URL.createObjectURL(file);
    renderPDF(url, 'pdf-canvas');
}

async function renderPDF(url: any, canvasId: any) {
    // 加载 PDF 文档
    const loadingTask = pdfjsLib.getDocument(url);
    const pdf = await loadingTask.promise;

    // 获取第一页
    const page = await pdf.getPage(1);

    // 设置缩放比例
    const scale = 1.5;
    const viewport = page.getViewport({ scale });

    // 准备画布
    const canvas: any = document.getElementById(canvasId);
    const context = canvas.getContext('2d');
    canvas.height = viewport.height;
    canvas.width = viewport.width;

    // 渲染 PDF 页面到画布
    const renderContext = {
        canvasContext: context,
        viewport: viewport
    };
    await page.render(renderContext);
}


</script>
<template>
    <div class="content">
        <div class="left">
            <wired-card>
                <canvas id="pdf-canvas"></canvas>
            </wired-card>
            <div class="line">
                <wired-icon-button>
                    <img height="16" width="16" src="../assets/icon/left-arrow.svg" />
                </wired-icon-button>
                <input type="number" class="text-input" min="1"> /2
                <wired-icon-button>
                    <img height="16" width="16" src="../assets/icon/right-arrow.svg" />
                </wired-icon-button>
            </div>
        </div>
        <div class="right">
            <div class="line">
                <label>选择文件：</label>
                <wired-icon-button @click="upload">
                    <input id="pdf-upload" ref="uploadFile" type="file" hidden class="text-input"
                        accept="application/pdf" @change="afterUpload">
                    <img height="16" width="16" src="../assets/icon/upload.svg" />
                </wired-icon-button>
            </div>
            <div class="line">
                <label>小册子子集：</label>
                <wired-combo selected="two">
                    <wired-item value="one">仅正面</wired-item>
                    <wired-item value="two">仅背面</wired-item>
                </wired-combo>
            </div>
            <div style="margin-bottom: -12px;">页面大小：</div>
            <div class="line">
                <wired-combo selected="two">
                    <wired-item value="one">A4</wired-item>
                    <wired-item value="two">A5</wired-item>
                    <wired-item value="three">B5</wired-item>
                </wired-combo>
                <wired-input type="number" min="1" /> x
                <wired-input type="number" min="1" />
                mm
            </div>

            <div class="line">
                <label>页面：</label>
                <wired-combo selected="two">
                    <wired-item value="one">仅正面</wired-item>
                    <wired-item value="two">仅背面</wired-item>
                </wired-combo>
            </div>
            <div class="line">
                <label>装订：</label>
                <wired-combo selected="two">
                    <wired-item value="one">仅正面</wired-item>
                    <wired-item value="two">仅背面</wired-item>
                </wired-combo>
            </div>
            <div class="line">
                <wired-checkbox>逆序打印</wired-checkbox>
            </div>
            <div class="line">
                <label>页面范围：</label>
            </div>
        </div>
    </div>
</template>
<style lang="scss" scoped>
.content {
    width: 100vw;
    display: flex;
    height: calc(100vh - 62px);

    .text-input {
        border: none;
        outline: none;
        max-width: 46px;
        font-size: 16px;
        text-align: center;
        vertical-align: middle;
        border-bottom: 1px solid #333;
    }

    img {
        z-index: -1;
        position: relative;
    }

    wired-icon-button {
        margin: 0 12px;
        margin-top: 12px;
    }

    wired-input {
        width: 72px;
    }

    wired-card {
        width: 100%;
        aspect-ratio: 300/200;
        display: block;
        #pdf-canvas{
            width: 50%;
            height: 100%;
        }
    }

    .line {
        display: flex;
        margin-top: 16px;
        align-items: center;
    }

    .left {
        padding: 32px;
        width: 100%;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
    }

    .right {
        flex: 1;
        margin-bottom: 32px;
        padding: 16px;

    }
}
</style>