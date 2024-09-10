<script></script>
<template>
    <wired-card class="footer">
        <wired-button>取消</wired-button>
        <wired-button>另存为</wired-button>
        <wired-button>打印</wired-button>
    </wired-card>
</template>
<style lang="scss" scoped>
.footer {
    left: -4px;
    bottom: -4px;
    width: 100%;
    height: 50px;
    display: flex;
    position: fixed;
    align-items: center;
    justify-content: flex-end;
    // background-color: #e6e6e6;

    wired-button{
        margin-right: 32px;
    }
}
</style>